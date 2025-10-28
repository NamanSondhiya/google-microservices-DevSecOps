# Google Microservices DevSecOps

![Continuous Integration](https://github.com/GoogleCloudPlatform/microservices-demo/workflows/Continuous%20Integration%20-%20Main/Release/badge.svg)

**Online Boutique** is a cloud-native microservices demo application built for AWS deployment. This web-based e-commerce app demonstrates modern DevSecOps practices with 10 microservices (excluding loadgenerator) communicating via gRPC.

This application showcases enterprise-grade microservices architecture, containerization, and CI/CD pipelines ready for production deployment on AWS EKS.

## Architecture

**Online Boutique** consists of 10 microservices written in multiple languages, designed for cloud-native deployment.

[![Architecture of microservices](/docs/img/architecture-diagram.png)](/docs/img/architecture-diagram.png)

Find **Protocol Buffers Descriptions** at the [`./protos` directory](/protos).

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](/src/frontend)                           | Go            | Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically. |
| [cartservice](/src/cartservice)                     | C#            | Stores the items in the user's shopping cart in Redis and retrieves it.                                                           |
| [productcatalogservice](/src/productcatalogservice) | Go            | Provides the list of products from a JSON file and ability to search products and get individual products.                        |
| [currencyservice](/src/currencyservice)             | Node.js       | Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service. |
| [paymentservice](/src/paymentservice)               | Node.js       | Charges the given credit card info (mock) with the given amount and returns a transaction ID.                                     |
| [shippingservice](/src/shippingservice)             | Go            | Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)                                 |
| [emailservice](/src/emailservice)                   | Python        | Sends users an order confirmation email (mock).                                                                                   |
| [checkoutservice](/src/checkoutservice)             | Go            | Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.                            |
| [recommendationservice](/src/recommendationservice) | Python        | Recommends other products based on what's given in the cart.                                                                      |
| [adservice](/src/adservice)                         | Java          | Provides text ads based on given context words.                                                                                   |

## Screenshots

| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Screenshot of store homepage](/docs/img/online-boutique-frontend-1.png)](/docs/img/online-boutique-frontend-1.png) | [![Screenshot of checkout screen](/docs/img/online-boutique-frontend-2.png)](/docs/img/online-boutique-frontend-2.png) |

## CI/CD Pipeline Overview

This repository implements the Continuous Integration (CI) pipeline for the Google Microservices DevSecOps project.

### CI Pipeline Features
- **Multi-language Support**: Builds for Go, Python, Node.js, Java, and .NET services
- **Docker Image Building**: Creates optimized container images for each microservice
- **Security Scanning**: Integrates DevSecOps practices with vulnerability scanning
- **Automated Testing**: Runs unit and integration tests across all services
- **Artifact Management**: Pushes images to container registry for CD pipeline

### Jenkins Pipeline Structure
- `Jenkinsfile/` contains service-specific build configurations
- Supports parallel builds for faster CI execution
- Includes Slack notifications for build status updates
- Email notifications planned for future implementation

### Local Development
Use Docker Compose for local testing and development:

```bash
docker-compose up
```

This starts all microservices locally with proper service discovery and dependencies.

### Deployment
The CI pipeline feeds into the CD repository (`google-microservices-DevSecOps-CD`) which handles production deployments to AWS EKS using GitOps with ArgoCD.

## Project Structure

The project is organized as follows:

- **src/**: Contains the source code for all microservices.
  - **adservice/**: Java-based ad service.
  - **cartservice/**: C#-based cart service using Redis.
  - **checkoutservice/**: Go-based checkout service.
  - **currencyservice/**: Node.js-based currency conversion service.
  - **emailservice/**: Python-based email service.
  - **frontend/**: Go-based web frontend with HTML templates and static assets.
  - **loadgenerator/**: Python/Locust-based load testing service.
  - **paymentservice/**: Node.js-based payment service.
  - **productcatalogservice/**: Go-based product catalog service.
  - **recommendationservice/**: Python-based recommendation service.
  - **shippingservice/**: Go-based shipping service.

- **Jenkinsfile/**: Jenkins pipeline definitions for CI builds of each service.

- **docker-compose.yaml**: Local development setup with all services.

- **docs/**: Documentation files, including development guides and architecture details.

- **protos/**: Protocol Buffer definitions for gRPC services.

This structure supports a microservices architecture with clear separation of concerns, enabling independent development and testing of each service.

## Skills Demonstrated

- **Multi-language Development**: Proficiency in Go, Python, Node.js, Java, and C#
- **Microservices Design**: Service decomposition, API design, and inter-service communication
- **Containerization**: Docker best practices for multi-language applications
- **CI/CD**: Jenkins pipeline development for automated builds and testing
- **DevSecOps**: Integration of security scanning and compliance checks
- **Cloud-Native**: AWS EKS deployment with GitOps and Helm

## Documentation

- [Development Guide](/docs/development-guide.md) for local setup and development
- [Product Requirements](/docs/product-requirements.md) for project guidelines
- [Purpose](/docs/purpose.md) for project objectives


