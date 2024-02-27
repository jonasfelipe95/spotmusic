# SpotMusic
Solution Sprint 4 - Fiap

# SpotMusic Repositories Structure

Each SpotMusic component is housed in its own repository, designed to maintain a clear separation of concerns and facilitate independent development and deployment. Below are the folder structures for each repository:

## spotmusic-web (Frontend Web with Next.js)

/spotmusic-web
├── components/ # Reusable UI components
├── pages/ # Next.js pages and routing
├── public/ # Static files like images, fonts, etc.
├── styles/ # Global styles and theme configurations
├── lib/ # Utility functions and custom hooks
├── tests/ # Unit and integration tests
├── next.config.js # Next.js configuration
└── README.md # Project documentation


## spotmusic-mobile (Mobile App with Flutter)

/spotmusic-mobile
├── lib/
│ ├── models/ # Data models
│ ├── views/ # Screens and widgets
│ ├── services/ # API calls and data manipulation
│ └── main.dart # Entry point
├── assets/ # Images, fonts, and other assets
├── test/ # Unit and widget tests
└── README.md # Project documentation


## spotmusic-bff-web (BFF for Web Frontend)

/spotmusic-bff-web
├── src/
│ ├── controllers/ # Request handling and response formatting
│ ├── routes/ # API endpoints
│ ├── services/ # Business logic
│ └── app.js # Entry point
├── tests/ # Unit and integration tests
├── package.json # NPM dependencies and scripts
└── README.md # Project documentation


## spotmusic-bff-mobile (BFF for Mobile App)

/spotmusic-bff-mobile
├── src/
│ ├── controllers/ # Request handling and response formatting
│ ├── routes/ # API endpoints
│ ├── services/ # Business logic
│ └── app.js # Entry point
├── tests/ # Unit and integration tests
├── package.json # NPM dependencies and scripts
└── README.md # Project documentation


## spotmusic-service-playlists (Playlist Management Service in Go)


/spotmusic-service-playlists
├── cmd/ # Main applications for this project
├── internal/ # Private application and library code
├── pkg/ # Library code that's ok to use by external applications
├── scripts/ # Scripts to perform various build, install, analysis, etc operations
├── go.mod # Module dependencies
└── README.md # Project documentation


Each repository is structured to encourage best practices in software development, ensuring scalability, maintainability, and ease of collaboration.

This README.md provides a comprehensive guide to the folder structure of each project component, promoting a standardized approach to organizing code across the SpotMusic solution.
