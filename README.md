### Why Microservices?

Microservices is an architectural style where an application is built as a collection of small, independent services. Each service focuses on a specific business function and operates on its own. These services communicate with each other through well-defined APIs, making the system more modular and easier to manage.

### E-Commerce Website Using 10 Microservices on EKS Cluster

#### Project Overview

- **Project Name**: 11 Microservice CI/CD Pipeline
- **Deployment Platform**: AWS EKS (Elastic Kubernetes Service)
- **Application Type**: E-Commerce Website

#### Microservices Implemented

1. **Ad Sense Service**: Manages advertisements and promotions displayed on the website.
2. **Cart Service**: Handles user shopping carts, including adding, removing, and updating items.
3. **Checkout Service**: Manages the checkout process, including order summary and confirmation.
4. **Currency Service**: Provides currency conversion and pricing in different currencies.
5. **Email Service**: Sends emails to users for order confirmations, promotions, and notifications.
6. **Frontend Service**: Manages the user interface and user experience of the website.
7. **External Frontend (for load balancing)**: Distributes incoming traffic across multiple frontend instances to ensure high availability and performance.
8. **Payment Service**: Processes payments and handles transactions securely.
9. **Product Catalogue Service**: Manages product listings, details, and inventory.
10. **Recommendation Service**: Suggests products to users based on their browsing and purchase history.

These microservices can be deployed on an EKS cluster, which provides a managed Kubernetes environment to run containerized applications. Each microservice runs in its own container, and Kubernetes manages the deployment, scaling, and operation of these containers.

#### Reasons for Choosing Microservices

1. **Scalability**: Microservices allow individual services to scale independently based on their specific demand. For instance, the Cart Service can be scaled during high traffic periods without affecting other services.
2. **Resilience**: In a microservices architecture, the failure of one service does not necessarily impact the entire system. Each service can be designed to handle failures gracefully, thereby improving the overall resilience of the application.
3. **Development Speed**: Development teams can work on different services simultaneously without waiting for other teams. This parallel development accelerates the overall development process and allows for quicker releases.
4. **Technology Diversity**: Each microservice can be developed using the most suitable technology stack for its specific needs. For example, the Product Catalogue Service can use a NoSQL database for better performance, while the Checkout Service might use a relational database for transaction consistency.
5. **Isolation and Maintenance**: Microservices encapsulate their own logic, making it easier to understand, develop, test, and maintain. Changes in one service do not directly affect others, reducing the risk and complexity of updates.

#### Project Components and Pipeline

- **CI/CD Pipeline Using Jenkins**:
  - **Jenkinsfiles**: Each microservice has its own Jenkinsfile defining the steps for building, testing, and deploying the service.
  - **Docker**: Microservices are containerized using Docker, allowing for consistent environments from development to production.
  - **AWS EKS**: The microservices are deployed on AWS EKS, a managed Kubernetes service, which orchestrates and manages the lifecycle of the containers.

You can find more details in the [GitHub repository](https://github.com/naveensilver/Microservice-Project.git).
