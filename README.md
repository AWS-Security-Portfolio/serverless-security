## AWS Serverless Security Lab 8

Secure Serverless Application with Lambda, API Gateway, Cognito and WAF.

---

## Table of Contents

- [Overview]
- [Real-World Risk]
- [What I Built]
- [Diagram]
- [Objectives]
- [Steps Performed]
  - [1. Created DynamoDB Table]
  - [2. Developed Lambda Function]
  - [3. Configured API Gateway]
  - [4. Set Up Cognito User Pool and App Client]
  - [5. Attached Cognito JWT Authorizer to API Gateway]
  - [6. Configured AWS WAF Web ACL]
  - [7. Scoped IAM Roles]
  - [8. Generated Cognito Tokens with AWS CLI]
  - [9. Tested API Calls with Postman]
  - [10. Cleanup]
- [Screenshots]
- [Known Limitations]
- [Lessons Learned]
- [Authentication Troubleshooting]
- [References]

--- 

## Overview

In this lab, you built a secure serverless application on AWS using Lambda, API Gateway, DynamoDB, Amazon Cognito, and AWS WAF. You configured authentication and authorization with Cognito user pools, protected your API with WAF, and implemented fine-grained IAM policies. The lab demonstrates best practices for securing serverless apps.

---

## Real-World Risk

In modern cloud applications, securing serverless APIs is critical to prevent unauthorized access and data breaches. This lab addresses real-world risks such as unauthorized users invoking backend Lambda functions, which could lead to data exposure or manipulation. It mitigates common web exploits like SQL injection and cross-site scripting by integrating AWS WAF as a protective firewall. The use of fine-grained IAM roles ensures least privilege access, limiting the potential attack surface by restricting permissions only to what is strictly necessary. Additionally, securing API Gateway endpoints with Cognito authentication and JWT tokens prevents public exposure of sensitive resources, safeguarding both the application and its data from malicious actors.

---

## What I Built 

- A secure serverless API application using:
  - AWS Lambda (Python/Node.js) running backend code.
  - API Gateway serving RESTful endpoints with Cognito-based JWT authentication.
  - DynamoDB NoSQL table for data persistence with scoped access.
  - AWS WAF protecting API Gateway from common web exploits.
  - IAM roles with least privilege scoped to Lambda and DynamoDB.
- Comprehensive documentation including architecture diagram, code, and test screenshots.

---

## Diagram

![Architecture Diagram](diagram.png)

---

## Objectives

- Deploy a Lambda-based REST API using API Gateway.
- Use DynamoDB as the backend data store.
- Implement user authentication with Amazon Cognito user pools.
- Secure the API Gateway with JWT authorizer and AWS WAF.
- Apply least privilege IAM role permissions to Lambda.
- Validate input and secure API endpoints against unauthorized access.

---

## Steps Performed

1. Created DynamoDB Table
   - Set up a DynamoDB table named Lab8Items with a partition key id of type String to store items securely.

2. Developed Lambda Function
   - Wrote Python code to handle POST and GET methods, allowing adding and retrieving items.
   - Implemented basic input validation to prevent invalid data.

3. Configured API Gateway
   - Created REST API named Lab8Api.
   - Defined /items resource with GET and POST routes integrated with Lambda function.

4. Set Up Cognito User Pool and App Client
   - Created Cognito user pool Lab8UserPool for managing user authentication.
   - Created an app client with OAuth flows enabled, proper callback URLs, and JWT token issuance.

5. Attached Cognito JWT Authorizer to API Gateway
   - Configured API Gateway to use Cognito JWT authorizer for validating incoming requests.
   - Scoped authorization to secure the /items routes.

6. Configured AWS WAF Web ACL
   - Created WAF Web ACL protecting the API Gateway from common vulnerabilities like SQLi and XSS.
   (Note: Due to regional constraints, WAF integration had limitations documented)

7. Scoped IAM Roles
   - Scoped Lambda execution role to only allow necessary DynamoDB and CloudWatch logging permissions.

8. Generated Cognito Tokens with AWS CLI
   - Used initiate-auth to get JWT access tokens for authenticated API testing.

9. Tested API Calls with Postman
   - Sent authorized API requests with Bearer tokens in Authorization header.
   - Documented expected responses and error scenarios (e.g., 401 Unauthorized)
  
10. Cleanup
   - Delete the Lambda function(s) created for the API.
   - Remove the API Gateway instance(s) used for this lab.
   - Delete the DynamoDB table(s) created.
   - Delete the Cognito user pool and any app clients created.
   - Remove the WAF Web ACLs and associated rules.
   - Delete any IAM roles or custom policies created specifically for this lab.

---

## Screenshots

*All screenshots are included in the screenshots/ folder.

| Step | Filename                       | Description                                                       |
| ---- | ------------------------------ | ----------------------------------------------------------------- |
| 1    | dynamodb-table-created.png     | DynamoDB table creation with partition key configuration          |
| 2    | lambda-iam-policy.png          | IAM policy attached to Lambda function with scoped permissions    |
| 3    | lambda-user-isolation.png      | Lambda function code showing user input isolation and validation  |
| 4    | apigateway-cognito-auth.png    | Cognito authorizer setup attached to API Gateway routes           |
| 5    | apigateway-routes.png          | API Gateway configuration showing GET and POST `/items` routes    |
| 6    | apigateway-stage-url.png       | API Gateway deployed stage URL                                    |
| 7    | cognito-userpool.png           | Cognito user pool and app client setup with OAuth flows           |
| 8    | postman-401-unauthorized.png   | Postman request with invalid token showing 401 Unauthorized error |
| 9    | waf-unavailable-limitation.png | AWS WAF console showing limitations due to regional constraints   |

## Screenshot Explanations

1. dynamodb-table-created.png: Shows the DynamoDB table setup with the partition key configuration for storing items.

2. lambda-iam-policy.png: Displays the IAM policy attached to the Lambda execution role, demonstrating scoped permissions.

3. lambda-user-isolation.png: Shows Lambda function code with user input isolation and basic validation logic.

4. apigateway-cognito-auth.png: API Gateway console screenshot showing Cognito JWT authorizer attached to routes for securing the API.

5. apigateway-routes.png: Configuration of API Gateway routes for GET and POST methods under the /items resource.

6. apigateway-stage-url.png: Deployed API Gateway stage URL used for invoking the Lambda-backed API.

7. cognito-userpool.png: Cognito User Pool and App Client settings showing OAuth flows and client configurations.

8. postman-401-unauthorized.png: Postman request with Authorization header returning a 401 Unauthorized response, indicating security enforcement.

9. waf-unavailable-limitation.png: AWS WAF console screenshot showing regional limitation message preventing full WAF integration.

---

## Known Limitations

- AWS WAF regional scope restrictions limited full integration with API Gateway.  
- See the WAF limitation screenshot for details.

---

## Lessons Learned

- Cognito user pools provide robust user authentication with OAuth 2.0 flows.
- API Gateway JWT authorizers enforce secure access control for REST APIs.
- AWS WAF can block common web attacks like SQL injection and XSS.
- Proper IAM role scoping reduces attack surface for Lambda functions.
- Input validation is crucial to avoid unauthorized data manipulation.
- Testing tokens and API requests is essential for verifying security controls.

---

## Authentication Troubleshooting

- Encountered 401 Unauthorized errors due to token issues during testing.  
- Importance of using valid Access Tokens and correct Authorization header format emphasized.  
- Correct API Gateway authorizer and route setup is critical for successful authentication.

---

## References

- Amazon Cognito User Pools
  https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html

- API Gateway JWT Authorizers
  https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html

- AWS WAF Overview
  https://aws.amazon.com/waf/

- AWS Lambda Security Best Practices
  https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

---

Sebastian Silva C. - July, 2025 - Berlin, Germany.
