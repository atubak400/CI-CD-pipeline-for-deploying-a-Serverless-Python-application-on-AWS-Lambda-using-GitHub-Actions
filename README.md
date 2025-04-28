# 🚀 CI/CD Pipeline for Serverless Python Application on AWS Lambda (Using GitHub Actions, CodePipeline, CodeDeploy)

## 📋 Project Overview

This project implements a complete **CI/CD pipeline** to automatically deploy a **Python-based AWS Lambda function** using **GitHub Actions**, **AWS CodePipeline**, **CodeBuild**, **CodeDeploy**, and **S3** for artifact storage.  
It demonstrates best practices for **serverless deployments**, **infrastructure as code**, and **GitHub-driven DevOps automation**.

---

## 📌 Architecture Diagram

```
GitHub ➔ GitHub Actions ➔ S3 Bucket ➔ AWS CodePipeline ➔ CodeBuild ➔ CodeDeploy ➔ AWS Lambda (Blue/Green deployment with Aliases)
```

---

## 📦 Project Structure

```
├── lambda-app/
│   └── lambda_function.py       # Python Lambda function code
├── appspec.yml                   # CodeDeploy configuration for Lambda deployment
├── buildspec.yml                  # CodeBuild instructions to package and upload artifacts
├── requirements.txt               # Python dependencies (if any)
└── README.md                      # Project documentation (this file)
```

---

## 🛠️ Key AWS Services Used

| Service          | Purpose                                                                          |
| :--------------- | :------------------------------------------------------------------------------- |
| **S3**           | Stores Lambda deployment artifacts                                               |
| **CodePipeline** | Orchestrates the CI/CD stages (Source ➔ Build ➔ Deploy)                          |
| **CodeBuild**    | Packages the Lambda app and uploads artifacts                                    |
| **CodeDeploy**   | Performs the Lambda deployment (manages traffic shifting using aliases)          |
| **IAM**          | Secure role permissions for GitHub OIDC, CodePipeline, CodeBuild, and CodeDeploy |
| **Lambda**       | Serverless compute to run the Python application                                 |

---

## 📄 How the Pipeline Works

1. **Source Stage (GitHub):**

   - A push to the main branch triggers GitHub Actions.
   - GitHub Actions packages the code into a zip file and uploads it to the S3 bucket.

2. **Build Stage (AWS CodeBuild):**

   - CodeBuild reads the `buildspec.yml`.
   - It retrieves the code and prepares the build artifacts (zip file and appspec.yml).

3. **Deploy Stage (AWS CodeDeploy):**

   - CodeDeploy reads the `appspec.yml` configuration.
   - It updates the Lambda function using blue/green deployment strategy:
     - Shifts traffic from the old Lambda version to the new Lambda version using **aliases**.

4. **Lambda Execution:**
   - The new version of the serverless app is live with **zero downtime**.

---

## 📜 Important Configuration Files

### appspec.yml (for Lambda deployment)

```yaml
version: 0.0
Resources:
  - serverless-python-app:
      Type: AWS::Lambda::Function
      Properties:
        Name: serverless-python-app
        Alias: prod
        CurrentVersion: 1
        TargetVersion: 2
```

### buildspec.yml (for CodeBuild)

```yaml
version: 0.2
phases:
  install:
    runtime-versions:
      python: 3.8
  build:
    commands:
      - echo "Zipping Lambda function..."
      - zip -r function.zip .
artifacts:
  files:
    - function.zip
    - appspec.yml
```

---

## 📚 Prerequisites

- AWS Account with admin access
- GitHub repository setup
- S3 bucket created for artifacts
- IAM roles:
  - GitHub OIDC trust role
  - CodePipeline role
  - CodeBuild role
  - CodeDeploy role (with S3, Lambda, CloudWatch permissions)

---

## ⚙️ Steps to Set Up

1. Create and configure S3 bucket for deployment artifacts.
2. Create GitHub Actions workflow to build and upload to S3.
3. Create IAM roles for GitHub OIDC, CodePipeline, CodeBuild, CodeDeploy.
4. Set up CodePipeline to connect GitHub ➔ CodeBuild ➔ CodeDeploy.
5. Create CodeDeploy application and deployment group for Lambda.
6. Configure Lambda function with alias (`prod`).
7. Push your code to GitHub — automatic deployment triggered!

---

## 🧐 Key Concepts Used

- **Blue/Green Deployment** using Lambda aliases (`prod` alias)
- **Zero Downtime Deployment** in Lambda
- **GitHub Actions OIDC Authentication** for secure AWS access
- **Infrastructure as Code** with minimal manual configuration
- **Versioning in Lambda** to safely promote new releases

---

## 📈 Future Improvements

- Implement rollback policies for failed deployments.
- Use AWS Secrets Manager for secure secret storage.
- Add unit tests and integrate with CodeBuild for quality checks.
- Use advanced GitHub Actions workflows (multi-environment deploys: dev, staging, prod).

---

## 🏆 Final Outcome

👉 Fully automated CI/CD deployment of a **Python Lambda** app triggered by GitHub pushes  
👉 Safe, versioned Lambda deployments with traffic shifting  
👉 Professional-grade AWS serverless DevOps pipeline setup

---

## ✨ Author

**Kingsley Atuba**  
DevOps Engineer | AWS Certified | GitOps Enthusiast
