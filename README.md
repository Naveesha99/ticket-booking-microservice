# Ticket Booking Microservice

A comprehensive microservices-based ticket booking system built with Spring Boot, designed to handle multiple independent services for managing bookings, inventory, orders, and API routing. The project uses modern DevOps practices including Docker containerization and Keycloak authentication.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Adding to GitHub](#adding-to-github)
- [Building and Running](#building-and-running)
- [Services Documentation](#services-documentation)
- [Docker Setup](#docker-setup)
- [Testing](#testing)
- [Contributing](#contributing)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## 🎯 Overview

This project implements a ticket booking system using a microservices architecture. Each service is independently deployable and scalable, communicating with each other through REST APIs. The system includes:

- **API Gateway**: Routes all incoming requests to appropriate microservices
- **Booking Service**: Manages ticket booking operations
- **Inventory Service**: Manages ticket inventory and availability
- **Order Service**: Handles order processing and management

### Key Features

- ✅ Microservices architecture for scalability
- 🔐 OAuth2 security with Keycloak integration
- 📚 OpenAPI/Swagger documentation
- 🐳 Docker containerization with docker-compose
- 🗄️ MySQL database persistence
- 🔄 Circuit breaker pattern with Resilience4j
- 📊 Actuator endpoints for monitoring
- 🧪 Comprehensive test coverage

### 🔧 Key Technologies

This project leverages modern and industry-standard technologies:

| Technology | Version | Purpose |
|------------|---------|----------|
| **Spring Boot** | 4.0.x | Framework for building microservices |
| **Java** | 17 | Programming language |
| **Docker** | Latest | Containerization for deployment |
| **Kafka** | Latest | Message broker for event streaming |
| **MySQL** | 8.0+ | Relational database management |
| **JPA/Hibernate** | Latest | Object-relational mapping |
| **Flyway** | Latest | Database migration tool |
| **Swagger/OpenAPI** | 2.0.4 | API documentation |
| **Lombok** | Latest | Java boilerplate reduction |
| **Keycloak** | Latest | Identity and access management |
| **Resilience4j** | Latest | Resilience patterns (Circuit Breaker) |
| **Postman** | Latest | API testing tool |

### 📚 Key Concepts

The project implements several important enterprise architecture patterns and concepts:

**Microservices Architecture**
- Decoupled Services - Independent services with separate databases
- Scalability - Each service can be scaled independently
- Technology Diversity - Services can use different technologies if needed

**Security**
- Spring Security - Authentication and authorization framework
- OAuth2 - Token-based authentication protocol
- Keycloak Integration - Centralized identity management
- Resource Server - Validates security tokens for protected resources

**API Design**
- REST API - RESTful endpoints for service communication
- API Gateway Pattern - Single entry point for all client requests
- Swagger/OpenAPI - Machine-readable API specifications
- Functional Endpoint Programming - Alternative to annotation-based controllers

**Resilience & Fault Tolerance**
- Circuit Breaker Pattern - Prevents cascading failures using Resilience4j
- Graceful Degradation - Services can continue with reduced functionality
- Retry Logic - Automatic retry mechanisms for transient failures
- Timeout Management - Prevents hanging requests

**Data Management**
- MySQL Database - Persistent data storage
- JPA/Hibernate ORM - Object-relational mapping
- Flyway Migrations - Version-controlled database schema changes
- DB Schema - Normalized relational model

**Event-Driven Architecture**
- Apache Kafka - Distributed event streaming platform
- Kafka Topics - Channels for event distribution
- Producers - Services publishing events to Kafka topics
- Consumers - Services subscribing to and processing events
- Event Sourcing - Audit trail of all state changes

**Development & Testing**
- Postman Collections - Pre-built API test requests
- Integration Testing - End-to-end service testing
- Unit Testing - Component-level testing with JUnit
- Test Coverage - Comprehensive test scenarios

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│         Client Applications             │
└──────────────────┬──────────────────────┘
                   │
        ┌──────────▼──────────┐
        │   API Gateway       │
        │   (Port 8080)       │
        └──┬──────────────┬───┘
           │              │
    ┌──────▼────┐  ┌─────▼──────┐
    │  Booking  │  │  Inventory │
    │  Service  │  │  Service   │
    │(Port 8081)│  │ (Port 8082)│
    └──────┬────┘  └─────┬──────┘
           │              │
    ┌──────▼────┐  ┌─────▼──────┐
    │   MySQL   │  │  Keycloak  │
    │ Database  │  │ (OAuth2)   │
    └───────────┘  └────────────┘
           │
    ┌──────▼────┐
    │   Order   │
    │  Service  │
    │(Port 8083)│
    └───────────┘
```

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

### Required Software

- **Java Development Kit (JDK) 17 or higher**
  - Download from: https://www.oracle.com/java/technologies/downloads/
  - Verify: `java -version`

- **Maven 3.6.0 or higher**
  - Download from: https://maven.apache.org/download.cgi
  - Verify: `mvn -version`

- **Git**
  - Download from: https://git-scm.com/
  - Verify: `git --version`

- **Docker & Docker Compose** (optional, for containerized setup)
  - Download from: https://www.docker.com/products/docker-desktop
  - Verify: `docker --version` and `docker-compose --version`

- **Postman or Insomnia** (optional, for API testing)
  - For testing REST endpoints

## 📁 Project Structure

```
ticket-booking-microservice/
├── README.md                          # This file
├── apigateway/                        # API Gateway service
│   ├── pom.xml                       # Maven configuration
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/                # Gateway controllers and components
│   │   │   └── resources/           # Configuration files
│   │   └── test/                    # Unit and integration tests
│   ├── SWAGGER_FIX_README.md         # Swagger setup documentation
│   └── verify_fix.sh                 # Verification script
│
├── bookingservice/                    # Booking service
│   ├── pom.xml                       # Maven configuration
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/                # Business logic
│   │   │   └── resources/           # Configuration
│   │   └── test/
│   └── HELP.md                       # Setup help
│
├── inventoryservice/                  # Inventory service
│   ├── pom.xml                       # Maven configuration
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/                # Inventory management code
│   │   │   └── resources/
│   │   └── test/
│   ├── docker-compose.yml            # Docker setup for local development
│   ├── docker/
│   │   ├── keycloak/               # Keycloak configuration
│   │   └── mysql/
│   │       └── init.sql            # Database initialization script
│   └── volume-data/                  # Persistent Docker volumes
│
└── orderservice/                      # Order service
    ├── pom.xml                       # Maven configuration
    ├── src/
    │   ├── main/
    │   │   ├── java/                # Order processing code
    │   │   └── resources/
    │   └── test/
    └── HELP.md
```

## 🚀 Getting Started

### Step 1: Clone the Repository

```bash
# Navigate to your development directory
cd ~/Documents/MyProjects/ticket-booking

# Clone the repository (if not already cloned)
git clone https://github.com/Naveesha99/ticket-booking-microservice.git
cd ticket-booking-microservice
```

### Step 2: Verify Project Setup

```bash
# Check the project structure
ls -la

# Verify Maven is installed
mvn -version

# Verify Java is installed
java -version
```

### Step 3: Build All Services

```bash
# Clean and build all services
mvn clean install

# This will:
# - Download all dependencies
# - Compile the source code
# - Run tests
# - Package the JAR files
```

### Step 4: Start the Services (Local Development)

**Option A: Using Docker Compose (Recommended)**

```bash
# Navigate to inventory service (contains docker-compose.yml)
cd inventoryservice

# Start all services with Docker
docker-compose up -d

# This will start:
# - MySQL database on port 3306
# - Keycloak on port 8180
# - All microservices

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

**Option B: Manual Startup**

```bash
# Terminal 1: Start API Gateway
cd apigateway
mvn spring-boot:run
# Runs on http://localhost:8080

# Terminal 2: Start Booking Service
cd bookingservice
mvn spring-boot:run
# Runs on http://localhost:8081

# Terminal 3: Start Inventory Service
cd inventoryservice
mvn spring-boot:run
# Runs on http://localhost:8082

# Terminal 4: Start Order Service
cd orderservice
mvn spring-boot:run
# Runs on http://localhost:8083
```

## 🌐 Adding to GitHub

### Step 1: Create a New GitHub Repository

1. Go to [GitHub](https://github.com)
2. Click the **+** icon in the top right corner
3. Select **New repository**
4. Fill in the repository details:
   - **Repository name**: `ticket-booking-microservice`
   - **Description**: `A Spring Boot microservices for ticket booking system`
   - **Visibility**: Choose **Public** or **Private**
   - **Initialize repository**: Leave unchecked (we'll push existing code)
5. Click **Create repository**

### Step 2: Add Remote Repository

```bash
# Navigate to your project directory
cd ~/Documents/MyProjects/ticket-booking/ticket-booking-microservice

# Add the GitHub repository as remote
git remote add origin https://github.com/Naveesha99/ticket-booking-microservice.git

# Verify the remote was added
git remote -v
```

### Step 3: Configure Git User (if not already done)

```bash
# Set your GitHub username
git config --global user.name "Your Name"

# Set your GitHub email
git config --global user.email "your.email@example.com"

# Verify configuration
git config --global --list
```

### Step 4: Push Code to GitHub

```bash
# Check current branch
git branch

# If not on main/master, create and switch to main
git checkout -b main

# Add all files to staging
git add .

# Create initial commit
git commit -m "Initial commit: Ticket booking microservice system"

# Push to GitHub
git push -u origin main

# For subsequent pushes, simply use:
git push
```

### Step 5: Create .gitignore File (if not exists)

```bash
# Check if .gitignore exists
ls -la | grep gitignore

# If not, create one with the following content
cat > .gitignore << 'EOF'
# Maven
target/
*.jar
*.war
*.ear
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties

# IDE
.idea/
.vscode/
*.swp
*.swo
*~
.DS_Store
*.iml
*.classpath
*.project
.settings/

# Environment
.env
.env.local
application-local.properties

# Docker volumes
volume-data/

# Logs
*.log
logs/

# Database
*.db
*.sqlite

# Node (if used)
node_modules/
npm-debug.log
EOF

# Add and commit gitignore
git add .gitignore
git commit -m "Add .gitignore file"
git push
```

### Step 6: Create GitHub Documentation Files

Create additional documentation in the repository:

```bash
# Create CONTRIBUTING.md
cat > CONTRIBUTING.md << 'EOF'
# Contributing to Ticket Booking Microservice

## How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Code Standards

- Follow Spring Boot best practices
- Use meaningful variable and method names
- Add Javadoc comments for public methods
- Write unit tests for new features
- Ensure all tests pass before submitting PR

## Commit Message Format

- Use clear, descriptive commit messages
- Format: `[SERVICE] Brief description`
- Example: `[API-Gateway] Fix authentication filter`
EOF

git add CONTRIBUTING.md
git commit -m "Add contribution guidelines"
git push
```

### Step 7: Push Code to GitHub from Terminal

```bash
# If you haven't already pushed, do it now
git push -u origin main

# You should see output like:
# Counting objects: 150, done.
# Compressing objects: 100% (90/90), done.
# Writing objects: 100% (150/150), 2.50 MiB | 1.25 MiB/s, done.
# Total 150 (delta 45), reused 0 (delta 0), pack-reused 0
# To https://github.com/YOUR_USERNAME/ticket-booking-microservice.git
# * [new branch]      main -> main
# Branch 'main' is set to track remote branch 'main' from 'origin'.
```

## 🔨 Building and Running

### Build All Services

```bash
# Full clean build with tests
mvn clean install

# Build without running tests (faster)
mvn clean install -DskipTests

# Build only a specific service
cd apigateway && mvn clean install
```

### Run Individual Services

**API Gateway Service**
```bash
cd apigateway
mvn spring-boot:run
# Access Swagger UI: http://localhost:8080/swagger-ui.html
```

**Booking Service**
```bash
cd bookingservice
mvn spring-boot:run
# Endpoints: http://localhost:8081/api/bookings
```

**Inventory Service**
```bash
cd inventoryservice
mvn spring-boot:run
# Database required (via Docker Compose)
```

**Order Service**
```bash
cd orderservice
mvn spring-boot:run
# Endpoints: http://localhost:8083/api/orders
```

### View Application Logs

```bash
# View recent logs (last 100 lines)
cd <service-directory>
tail -n 100 target/application.log

# Follow logs in real-time
tail -f target/application.log
```

## 📚 Services Documentation

### API Gateway (Port 8090)

The API Gateway serves as the entry point for all client requests and routes them to appropriate microservices.

- **Handles**: Request routing, authentication, load balancing
- **Technologies**: Spring Cloud Gateway, OAuth2 Resource Server
- **Documentation**: [apigateway/SWAGGER_FIX_README.md](apigateway/SWAGGER_FIX_README.md)

### Inventory Service (Port 8080)

Manages ticket inventory and availability.

- **Functionality**: Track available tickets, manage stock levels
- **Database**: MySQL (includes Docker setup)
- **Authentication**: Keycloak OAuth2
- **API Base URL**: `http://localhost:8082/api/inventory`
- **Setup**: See [docker-compose.yml](inventoryservice/docker-compose.yml)

### Booking Service (Port 8081)

Manages all ticket booking operations.

- **Functionality**: Create, read, update, cancel bookings
- **Database**: MySQL (via Inventory Service)
- **API Base URL**: `http://localhost:8081/api/bookings`

### Order Service (Port 8082)

Handles order processing and management.

- **Functionality**: Process orders, track order status
- **API Base URL**: `http://localhost:8083/api/orders`

## 🐳 Docker Setup

The Inventory Service includes a complete Docker setup for local development.

### Start Services with Docker

```bash
cd inventoryservice

# Start all services (MySQL, Keycloak, Inventory Service)
docker-compose up -d

# View running containers
docker ps

# View container logs
docker-compose logs -f

# Stop services
docker-compose down

# Remove volumes (WARNING: deletes data)
docker-compose down -v
```

### Database Access

```bash
# MySQL credentials (from docker-compose.yml)
Host: localhost
Port: 3306
Username: root
Password: password
Database: ticket_booking

# Connect via MySQL CLI
mysql -h localhost -u root -ppassword ticket_booking

# Query example
SELECT * FROM inventory;
```

### Keycloak Access

```bash
# Keycloak Admin Console
URL: http://localhost:8180/admin
Username: admin
Password: admin123
```

## 🧪 Testing

### Run Tests

```bash
# Run all tests
mvn clean test

# Run tests for a specific service
cd apigateway && mvn test

# Run specific test class
mvn test -Dtest=ApigatewayApplicationTests

# Generate coverage report
mvn clean test jacoco:report
```

### Test Reports

```bash
# View test results
# After running tests, check:
cat target/surefire-reports/*.xml

# Example for API Gateway
apigateway/target/surefire-reports/TEST-com.example.apigateway.ApigatewayApplicationTests.xml
```

### Integration Testing

```bash
# Run integration tests (with running services)
mvn verify

# This runs unit tests + integration tests
# Requires services to be running on their respective ports
```

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository** on GitHub
2. **Clone your fork**: `git clone https://github.com/YOUR_USERNAME/ticket-booking-microservice.git`
3. **Create a branch**: `git checkout -b feature/your-feature-name`
4. **Make changes** and commit: `git commit -m "[SERVICE] Description"`
5. **Push to GitHub**: `git push origin feature/your-feature-name`
6. **Create a Pull Request** with detailed description

## 🐛 Troubleshooting

### Service Won't Start

**Problem**: Port already in use
```bash
# Find process using the port
lsof -i :8080

# Kill the process
kill -9 <PID>
```

**Problem**: Database connection failed
```bash
# Check if Docker services are running
docker ps

# Recreate containers
cd inventoryservice
docker-compose down
docker-compose up -d
```

### Build Failures

```bash
# Clear Maven cache
rm -rf ~/.m2/repository

# Rebuild
mvn clean install -U
```

### Maven Issues

```bash
# Update Maven dependencies
mvn dependency:resolve

# Check for conflicts
mvn dependency:tree
```

For more detailed troubleshooting, see:
- [apigateway/TROUBLESHOOTING.md](apigateway/TROUBLESHOOTING.md)
- [apigateway/QUICK_FIX.md](apigateway/QUICK_FIX.md)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📞 Support

For issues, questions, or suggestions:

1. **Check existing documentation** in service directories
2. **Create an Issue** on GitHub with detailed information
3. **Contact maintainers** via GitHub discussions

## 🔗 Quick Links

- GitHub Repository: https://github.com/Naveesha99/ticket-booking-microservice
- Issue Tracker: https://github.com/Naveesha99/ticket-booking-microservice/issues
- Documentation: https://github.com/Naveesha99/ticket-booking-microservice/wiki
- API Documentation: http://localhost:8080/swagger-ui.html (when running)

---

**Happy Coding!** 🚀
