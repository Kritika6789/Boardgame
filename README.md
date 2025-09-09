# BoardgameListingWebApp

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## Working of Project ##
This Project include complete Devops Setup to deploy the application on EKS Cluster. Before deployment certain operations are performed like static code analysis, building image, storing artifacts, scanning image using trivy and pushing image to ECR. After this application is deployed on EKS Cluster.

## Components ##
EKS Cluster- Application is deployed on it
Jenkins - Handles CI/CD Pipeline
SonarQube - Used to Scan image and perform Static Code analysis 
Nexus - Used to Store Artifacts. Two Repos are created one is Nexus Snapshot and other one is Nexus release.
Trivy - Used to scan image and find vulnerabilties in it
ECR - Push Image to ECR

## Jenkins-CI Pipeline: ##

- Clone the repo
- Perform sonarqube analysis to scan the code and check vulnerabilities, duplicate lines of code realred information on Sonarqube UI. It runs on port 9000
- Quality Gate: Addedd the quality checks and when these checks will be passed then only image will be build
- Build the package using mvn clean package
- Nexus: Deploy the artifacts on Nexus. Created 2 repos on nexus, one is maven release and other one is maven snapshot.Changing the version in pom.xml to 7.0.0-SNAPSHOT will store    artifacts in maven snapshot.and if version id 7.0.0 in pom.xml then artifacts will be stored in maven-release repo.Nexus is running on port 8081.
- Install Docker
- Build Docker image
- Use trivy to scan image and find vulnerabilties in image
- Push image to ECR

## Jenkins-Deployment: ##

** Prequisist:**
** EKS-CLUSTER Setup **
- Create a EKS Cluster on AWS and create NODE Groups
- While creating Cluster IAM role and NODE IAM role is used
- Run aws eks update-kubeconfig --region your-region --name cluster-name
- Create a Access point in EKS Cluster to connect ec2 instance with Cluster
- Check the connection using kubectl get nodes

** In Jenkins-Deployment Pipeline : **
- clone the repo in instance
- Run kubectl apply -f deployment-service.yml. After running this pods, deployment and service is created. Service type is load balancer and can access applicvation using this load balancer created on AWS.

## MONITORING ##
- Prometheus and grafana is used to monitor Jenkins and EKS Cluster.
- Prometehus runs on port 9090 and Grafana on port 3000

## Security Group ##
- Jenkins - Allow  HTTPS(443), Jenkins(8080), Sonarqube (9000), Nexus(8081)
- Sonarqube - Allow sonarqube(9000), HTTPS(443), Jenkins(8080)
- Nexus - Allow Nexus(8081), HTTPS(443), Jenkins(8080)
- Prometheus - Allow Port 9090
- Grafana- Allow Port 3000

## Quick verification  ##
After deployment:

- Check the app loads through the load balancer
- Look at Grafana dashboards for metrics
- Make sure no critical security issues in the latest scan
- Verify your JAR ended up in the right Nexus repo
