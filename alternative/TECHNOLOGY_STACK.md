# Deliverable 5: Technology Stack (AWS Cloud-First)

> **Project Scope**: See `PROJECT_SCOPE.md` for feature list.

---

## Frontend Technologies

| Technology        | Purpose                                         |
|-------------------|-------------------------------------------------|
| **Angular**       | Frontend framework                              |
| **TypeScript**    | Type-safe JavaScript                            |
| **RxJS**          | Reactive programming                            |
| **WebSocket API** | Native WebSocket client (API Gateway WebSocket) |

---

## Backend Technologies

| Technology      | Purpose                                               |
|-----------------|-------------------------------------------------------|
| **AWS Lambda**  | Serverless compute (Node.js)                          |
| **AWS SAM**     | Serverless Application Model (Infrastructure as Code) |
| **Node.js**     | Lambda runtime languages                              |
| **AWS SDK**     | AWS service integration                               |
| **AWS Cognito** | Authentication & authorization (OAuth 2.1/OIDC)       |

---

## API & Integration

| Technology                  | Purpose                                         |
|-----------------------------|-------------------------------------------------|
| **API Gateway (HTTP/REST)** | REST API endpoints                              |
| **API Gateway (WebSocket)** | Real-time WebSocket connections                 |
| **EventBridge**             | Event-driven architecture, message distribution |

---

## Database & Storage

| Technology          | Purpose                             |
|---------------------|-------------------------------------|
| **Amazon DynamoDB** | NoSQL database (primary data store) |
| **Amazon S3**       | Object storage for uploaded files   |

---

## Authentication & Authorization

| Technology                | Purpose                                        |
|---------------------------|------------------------------------------------|
| **AWS Cognito**           | User management, authentication, authorization |
| **Cognito User Pool**     | User directory, OAuth 2.1/OIDC support         |
| **Cognito Identity Pool** | Optional: AWS resource access                  |
| **IAM**                   | Access control for AWS resources               |

---

## Infrastructure & Deployment

| Technology         | Purpose                                               |
|--------------------|-------------------------------------------------------|
| **AWS SAM**        | Serverless Application Model (Infrastructure as Code) |
| **CloudFormation** | Infrastructure provisioning (used by SAM)             |
| **SAM CLI**        | Local development and deployment                      |
| **CloudFront**     | CDN for frontend static assets and file delivery      |
| **S3**             | Frontend static asset hosting                         |

---

## Monitoring & Observability

| Technology                | Purpose             |
|---------------------------|---------------------|
| **CloudWatch Logs**       | Centralized logging |
| **CloudWatch Metrics**    | Performance metrics |
| **CloudWatch Alarms**     | Automated alerting  |
| **CloudWatch Dashboards** | Custom dashboards   |

---

## Security

| Technology              | Purpose                            |
|-------------------------|------------------------------------|
| **AWS Secrets Manager** | Secrets management                 |
| **AWS KMS**             | Key management for encryption      |
| **AWS WAF**             | Optional: Web Application Firewall |
---


**Note:** I only listed the technologies that I'm experienced with and would fit the project. There might be new or better fitting alternative AWS services for different parts of the project.