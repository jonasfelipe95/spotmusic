```
# provider.tf
provider "aws" {
  region = "us-east-1"
}

# variables.tf
variable "region" {
  description = "AWS Region"
  default     = "us-east-1"
}

variable "project_name" {
  description = "Nome do projeto"
  default     = "SpotMusic"
}

# vpc.tf
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  name    = "${var.project_name}-vpc"
  cidr    = "10.0.0.0/16"
  azs     = ["${var.region}a", "${var.region}b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.3.0/24", "10.0.4.0/24"]
  enable_nat_gateway = true
  enable_vpn_gateway = false
}

# ecs.tf
resource "aws_ecs_cluster" "spotmusic" {
  name = "${var.project_name}-ecs-cluster"
}

# ecr.tf
resource "aws_ecr_repository" "spotmusic" {
  name                 = "${var.project_name}-repository"
  image_tag_mutability = "MUTABLE"
  image_scanning_configuration {
    scan_on_push = true
  }
}

# cloudwatch.tf
resource "aws_cloudwatch_log_group" "spotmusic" {
  name = "/ecs/${var.project_name}"
  retention_in_days = 30
}

# iam.tf
resource "aws_iam_role" "ecs_task_execution_role" {
  name = "ecsTaskExecutionRole"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Action = "sts:AssumeRole",
      Effect = "Allow",
      Principal = {
        Service = "ecs-tasks.amazonaws.com"
      }
    }]
  })
}

# security_groups.tf
resource "aws_security_group" "spotmusic_sg" {
  name        = "${var.project_name}-sg"
  description = "Security group for SpotMusic ECS tasks"
  vpc_id      = module.vpc.vpc_id

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# ecs_task_definition.tf
resource "aws_ecs_task_definition" "spotmusic_task" {
  family                   = "spotmusic-task"
  requires_compatibilities = ["FARGATE"]
  network_mode             = "awsvpc"
  cpu                      = "256"
  memory                   = "512"
  execution_role_arn       = aws_iam_role.ecs_task_execution_role.arn

  container_definitions = jsonencode([
    {
      name      = "spotmusic-container"
      image     = "${aws_ecr_repository.spotmusic.repository_url}:latest"
      cpu       = 256
      memory    = 512
      essential = true
      portMappings = [{
        containerPort = 80
        hostPort      = 80
        protocol      = "tcp"
      }]
      logConfiguration = {
        logDriver = "awslogs"
        options = {
          awslogs-group         = aws_cloudwatch_log_group.spotmusic.name
          awslogs-region        = var.region
          awslogs-stream-prefix = "ecs"
        }
      }
      environment = [
        {
          name  = "AWS_S3_BUCKET"
          value = "spotmusic-bucket"
        },
        {
          name  = "ANALYTICS_SERVICE_URL"
          value = "https://analytics.spotmusic.com"
        }
      ]
    }
  ])
}
```
