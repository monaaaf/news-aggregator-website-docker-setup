# Fullstack Project with Laravel and React (Dockerized)

This project consists of two applications:
- **Backend**: Laravel 11.31
- **Frontend**: React.js with TypeScript

Both applications are Dockerized, and a single `docker-compose` environment is provided for easy setup and execution.

## Prerequisites
- Docker and Docker Compose installed on your machine.

---

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Repository Structure](#repository-structure)
3. [Prerequisites](#prerequisites)
4. [Setup Guide](#setup-guide)
   - [Clone the Repositories](#1-clone-the-repositories)
   - [Environment Configuration](#2-environment-configuration)
   - [Build and Run Containers](#3-build-and-run-containers)
5. [Accessing the Applications](#accessing-the-applications)
6. [Running Commands](#running-commands)
   - [Backend Commands](#backend-commands)
   - [Frontend Commands](#frontend-commands)
7. [Data Scraping](#data-scraping-and-filtering)
8. [Best Practices](#best-practices)
9. [Troubleshooting](#troubleshooting)

---

## **Project Overview**
This project demonstrates a full-stack architecture:
- **Backend**: Handles API endpoints and business logic using Laravel.
- **Frontend**: Provides a responsive UI built with React TypeScript.
- **Database**: MySQL is used for persistent data storage.
- **Containerization**: All services are Dockerized, managed via `docker-compose`.

---

## **Repository Structure**
Here’s the structure and roles of the repositories:
1. **Docker Setup Repository**:
   - Contains `docker-compose.yml` and shared configurations for the backend and frontend.
   - Link: [Docker Setup Repository](https://github.com/monaaaf/news-aggregator-website-docker-setup.git)

2. **Backend Repository**:
   - Laravel application code.
   - Link: [Backend Repository](https://github.com/monaaaf/news-aggregator-website-backend.git)

3. **Frontend Repository**:
   - React TypeScript application code.
   - Link: [Frontend Repository](https://github.com/monaaaf/news-aggregator-website-frontend.git)

---

## **Prerequisites**
Before you begin, ensure you have the following installed:
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/downloads)

---

## **Setup Guide**

### 1. Clone the Repositories
Start by cloning all three repositories into a common directory:
```bash
# Clone the Docker setup repository
git clone https://github.com/monaaaf/news-aggregator-website-docker-setup.git

# Clone the Backend repository
git clone https://github.com/monaaaf/news-aggregator-website-backend.git

# Clone the Frontend repository
git clone https://github.com/monaaaf/news-aggregator-website-frontend.git
```

Ensure your folder structure looks like this:      
project-root/   
├── docker-setup/   
├── backend/  
├── frontend/

### 2. Environment Configuration
#### Backend
1. Run composer install to install all project dependencies
```bash
cd news-aggregator-website-backend/
composer install
```
2. Copy the .env.example file in the backend repository and rename it to .env:
```bash
cp .env.example .env
```
3. Generate the application key:
```bash
php artisan key:generate
```
4. Update the database variables in the .env file to match the database service in Docker:
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=news_aggregator_db
DB_USERNAME=news_aggregator_user
DB_PASSWORD=123Aggregator987
```
5. Ensure the following variables are updated in your .env file to enable the scraping functionality:
```
THE_GUARDIAN_API_TOKEN=<your_guardian_api_token_here>
NEWS_API_ORG_API_TOKEN=<your_news_api_org_api_token_here>
NEW_YORK_TIMES_API_TOKEN=<your_nyt_api_token_here>
```
6. Clear cache:
```bash
php artisan optimize
```

#### Frontend
1. Create a .env file in the frontend repository with the following content:
```
VITE_APP_NAME="News Aggregator Website"
VITE_APP_API_URL=http://localhost:9001/api
```
2. Run npm install to install all required dependencies
```bash
cd news-aggregator-website-frontend/
npm install
```
3. Adjust other frontend environment variables as needed

### 3. Build and Run Containers
Navigate to the docker-setup directory and run:
```bash
cd news-aggregator-website-docker-setup
docker-compose up --build
docker-compose exec backend php artisan config:cache
```

This command will:
1. Build and run the backend, frontend, and database containers. 
2. Expose the backend on port 9001 and the frontend on port 3000.

---

## **Accessing the Applications**
1. Backend: http://localhost:9001
2. Frontend: http://localhost:3000

---

## **Running Commands**

### Backend Commands
[Action Required] Run Laravel-specific commands inside the backend container:
```bash
# Access the backend container
docker exec -it laravel-backend bash

# Run Artisan commands
php artisan migrate
php artisan db:seed

# To initiate data scraping, run the command below. Alternatively, you can ignore this and use the manual command provided under [Data Scraping]:
php artisan schedule:run 

docker-compose up --build
```

### Frontend Commands
To run frontend-specific commands inside the frontend container, you can using:
```bash
# Access the frontend container
docker exec -it react-frontend bash
```
---

## **Data Scraping**
To run the scraper manually for testing:
```bash
docker exec -it laravel-backend php artisan articles:scrape-all
```
---

## **Best Practices**
This project follows modern software development principles:
1. DRY (Don't Repeat Yourself): Common logic is reused across the codebase.
2. KISS (Keep It Simple, Stupid): Simplicity is prioritized to ensure maintainability.
3. SOLID Principles
---

## **Troubleshooting**
### Common Issues
1. Database Connection Issues:
   1. Ensure the MySQL service is running in Docker. 
   2. Verify the DB_HOST, DB_USERNAME, and DB_PASSWORD in the backend .env file. 
2. Port Conflicts:
   1. Check if ports 9000 or 3000 are already in use. Change the ports in docker-compose.yml if needed.
3. Container Fails to Start:
   1. View logs using:
   ```bash
   docker-compose logs <service_name>
   ```
4. Frontend API Errors:
   1. Ensure the VITE_APP_API_URL in the frontend .env is pointing to the backend URL.