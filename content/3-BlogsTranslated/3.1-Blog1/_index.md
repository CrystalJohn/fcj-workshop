---
title: "Build a Virtual Meteorologist Using Amazon Bedrock Agents"
date: 2025-02-11
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## By: Salman Ahmed, Ankush Goyal, Sergio Barraza, and Ravi Kumar

**Date:** 11 FEB 2025

**Topics:** [Amazon Bedrock](https://aws.amazon.com/bedrock/), [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/), [Amazon Cognito](https://aws.amazon.com/cognito/), [AWS Amplify](https://aws.amazon.com/amplify/), [AWS Lambda](https://aws.amazon.com/lambda/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), Intermediate (200)

---

## Introduction

The integration of generative AI capabilities is driving transformative changes across multiple industries. While weather information is accessible through various channels, businesses that rely heavily on meteorological data need robust and scalable solutions to efficiently manage and leverage these critical insights while reducing manual processes. The solution in this post demonstrates how to build an AI-powered virtual meteorologist that can answer complex weather-related queries using natural language. We use multiple AWS services to deploy a complete solution that you can use to interact with an API that provides real-time weather data. In this solution, we use [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/).

[Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) help streamline workflows and automate repetitive tasks. [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) can securely connect to your enterprise data sources and supplement user requests with accurate responses. You can use [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) to design an action schema that fits your requirements, thereby controlling whenever the agent initiates a specified action. This flexible approach allows you to integrate and execute business logic in your preferred backend service, creating a harmonious combination of functionality and flexibility. The system also has the ability to remember across interactions, providing a more personalized experience for users.

In this post, we introduce a streamlined approach to deploying an AI-powered agent by combining [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) and a foundation model (FM). We guide you through the process of configuring the agent and implementing the specialized logic required for the virtual meteorologist to provide accurate weather-related responses. Additionally, we use multiple AWS services, including AWS Amplify for hosting the front end, [AWS Lambda](https://aws.amazon.com/lambda/) functions for processing request logic, Amazon Cognito for user authentication, and AWS Identity and Access Management (IAM) for controlling access to the agent.

---

## Solution Overview

The following diagram provides an overall view and highlights the main components of the system.
![virtual-meteorologist-figure-1](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-1.png)
The architecture uses **Amazon Cognito** for user authentication and **AWS Amplify** as the hosting environment for the front-end application.

[**Amazon Bedrock Agents**](https://aws.amazon.com/bedrock/agents/) receives queries from users and passes the information to **action groups**, each of which triggers a corresponding custom **Lambda function**. Each action group and Lambda function handles a specific task:

- **geo-coordinates** – Processes geographic coordinate information to retrieve data about a specific location.
- **weather** – Collects weather information for the provided location.
- **date-time** – Determines the current date and time according to the appropriate time zone.

The solution leverages AWS's auto-scaling capabilities, built-in security, and serverless model to minimize operational costs while ensuring real-time responsiveness.

---

## Prerequisites

To fully deploy this solution, you need to prepare the following components:

- An active **AWS account**.
- Access to a **foundation model (FM)** in [**Amazon Bedrock**](https://aws.amazon.com/bedrock/), specifically [**Anthropic Claude**](https://aws.amazon.com/bedrock/claude/) **3.5 Sonnet**, in the same Region where you will deploy the solution.
- Download the accompanying **[AWS CloudFormation](https://aws.amazon.com/cloudformation/) template** from the [**aws-samples GitHub repository**](https://github.com/aws-samples) to automatically provision resources.

---

## Deploy Resources Using AWS CloudFormation

After preparing the prerequisites, you will deploy all necessary resources using [**AWS CloudFormation**](https://aws.amazon.com/cloudformation/). When you run the CloudFormation template, the system will automatically create the following components (note that the AWS services used in this solution will incur costs):

### Amazon Cognito

- [User pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html): CognitoUserPoolforVirtualMeteorologistApp
- [App client](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html): VirtualMeteorologistApp
- [Identity pool](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html): cognito-identity-pool-vm

### AWS Lambda

- `<Stack name>-geo-coordinates-<auto-generated>`
- `<Stack name>-weather-<auto-generated>`
- `<Stack name>-date-time-<auto-generated>`

### Amazon Bedrock Agents

- Agent: virtual-meteorologist
- Action group (1): obtain-latitude-longitude-from-place-name
- Action group (2): obtain-weather-information-with-coordinates
- Action group (3): get-current-date-time-from-timezone

After the CloudFormation stack is successfully deployed, you need to copy the values from the **Outputs** tab in the [**AWS CloudFormation**](https://aws.amazon.com/cloudformation/) **Console** to configure the application in **AWS Amplify**. The values to save include:

- AWSRegion
- BedrockAgentAliasId
- BedrockAgentId
- BedrockAgentName
- IdentityPoolId
- UserPoolClientId
- UserPoolId
![virtual-meteorologist-figure-2](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-2.png)
---

## Deploy AWS Amplify Application

The user interface application is stored in the AWS-Amplify-Frontend.zip file available on [**GitHub**](https://github.com/aws-samples).

You need to follow these steps to manually deploy the application:

1. Download the **AWS-Amplify-Frontend.zip** source code file from the [**GitHub**](https://github.com/aws-samples) repository.
2. Use this .zip file to [**manually deploy**](https://docs.aws.amazon.com/amplify/latest/userguide/manual-deploys.html) the application in [AWS Amplify](https://aws.amazon.com/amplify/).
3. After deployment is complete, return to the **AWS Amplify Console** page and use the automatically generated **domain** to **access the web application**.

---

## Use Amazon Cognito for User Authentication

[Amazon Cognito](https://aws.amazon.com/cognito/) is an identity service that allows you to authenticate and authorize users. In this solution, we use Amazon Cognito to verify user identity before they can access and use the application. Additionally, we use Identity Pool to provide temporary AWS credentials to users during their interaction with the [**Amazon Bedrock API**](https://aws.amazon.com/bedrock/).

---

## Use Amazon Bedrock Agents to Automate Tasks in the Application

With [**Amazon Bedrock Agents**](https://aws.amazon.com/bedrock/agents/), you can build and configure **automated agents** in your application. An agent helps end users perform actions based on organizational data and information they provide. These agents orchestrate the entire interaction process between **foundation models (FMs)**, data sources, application software, and user conversations. This allows the system to understand and execute complex requests in natural context while efficiently automating data processing workflows.

---

## Use Action Groups to Define Actions Amazon Bedrock Agent Performs

An action group defines a set of related actions that an [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent can perform to assist users. When configuring an action group, you have several options for processing user-provided data, including: adding user input to the agent's action group, passing data to an [AWS Lambda](https://aws.amazon.com/lambda/) for custom business logic processing, or returning control directly to the application through the InvokeAgent method response.

In this application, we create three action groups that enable the [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent to perform core functions: retrieve coordinates for a specific location, get the current date and time, and collect weather data at that location. These action groups allow the agent to access and process critical information, helping enhance the accuracy and comprehensive responsiveness to user queries related to geographic location and weather conditions.

---

## Use AWS Lambda for Amazon Bedrock Action Groups

In this solution, three [AWS Lambda](https://aws.amazon.com/lambda/) functions are deployed to support the action groups defined by the [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent:

- **Location Coordinates Lambda Function** – This function is triggered by the obtain-latitude-longitude-from-place-name action group. It takes a place name as input and returns the corresponding geographic coordinates (latitude and longitude). The function uses a geocoding service or database to look up this information.
- **Date and Time Lambda Function** – This function is triggered by the get-current-date-time-from-timezone action group. It determines the time zone and returns current date and time information.
- **Weather Information Lambda Function** – This function is triggered by the obtain-weather-information-with-coordinates action group. It takes the coordinates provided from the first function and retrieves current weather data or forecasts for the corresponding area through a weather API.

Each Lambda function receives an input event containing metadata and data fields populated from the [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent's API operation or function parameters. These functions process the data, execute their respective tasks, and return responses containing the necessary information. That response is then aggregated by the [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent to form a complete answer for the user.

By using Lambda functions, the [Amazon Bedrock](https://aws.amazon.com/bedrock/) Agent can access external data sources and perform complex calculations, significantly enhancing the agent's capabilities in handling requests related to location, time, and weather conditions.

---

## Use AWS Amplify for Front-End Code

[AWS Amplify](https://aws.amazon.com/amplify/) provides a development environment for secure, scalable web and mobile applications. Developers can focus on writing code instead of managing infrastructure. Amplify also integrates with multiple source code hosting platforms like GitHub, Bitbucket, or GitLab. In this solution, we manually upload the front-end source code to AWS Amplify, following the process described in the previous section of this post.

---

## Experience the Application

After deploying the application in AWS Amplify, you can access the automatically generated URL to open the user interface. When accessing this link, the application will request information related to Amazon Cognito and [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/). This information is necessary to securely authenticate users and allow the front end to interact with the agent on Bedrock. This enables the application to manage user sessions and make authorized API calls to AWS services on behalf of users.

You need to enter the information obtained from the CloudFormation Outputs section, including:

- User Pool ID
- User Pool Client ID
- Identity Pool ID
- Region
- Agent Name
- Agent ID
- Agent Alias ID
![virtual-meteorologist-figure-3](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-3.png)

Sign in with your username and password. A temporary password was automatically generated during deployment and sent to the email you provided when running the CloudFormation template. On your first login, you will be asked to change your password to complete the authentication process.
![virtual-meteorologist-figure-4-2](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-4-2.gif)
After successfully signing in, you can start asking questions to the application, for example:

> "Is today a good day to have a BBQ party in Dallas, Texas?"
![virtual-meteorologist-figure-5](/images/3-BlogsTranslated/3.1-Blog1/virtual-meteorologist-figure-5.gif)
Within just a few seconds, the application will display a detailed answer indicating whether today is suitable for hosting a BBQ party in Dallas.

---

## Sample Query Examples

Below are some questions demonstrating the virtual meteorologist's conversational capabilities:

1. "What's the weather like in New York City today?"
2. "Should I plan an outdoor birthday party in Miami next weekend?"
3. "Will there be snow in Denver on Christmas Day?"
4. "Can I go swimming at the beach in Chicago today?"

These queries demonstrate how the agent can provide current weather information, advise on activities based on forecasts, and predict future weather conditions. Users can ask questions related to specific activities (such as swimming, outdoor parties...), and the system will automatically analyze weather conditions to provide appropriate recommendations.

---

## Clean Up Resources

If you no longer want to use the virtual meteorologist, follow these steps to delete the application and all related resources deployed using [AWS CloudFormation](https://aws.amazon.com/cloudformation/) and [AWS Amplify](https://aws.amazon.com/amplify/):

### 1. Delete CloudFormation Stack

1. In the AWS CloudFormation Console, select **Stacks** in the navigation bar.
2. Identify the stack you created during deployment (the stack name was set during initialization).
3. Select that stack and click **Delete** to remove all related resources.

### 2. Delete Amplify Application and Associated Resources

For detailed instructions, please refer to the [Clean Up Resources](https://aws.amazon.com/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/module-six/) section in the official AWS documentation.

---

## Conclusion

This solution demonstrates the power of combining [Amazon Bedrock Agents](https://aws.amazon.com/bedrock/agents/) with other AWS services to create an intelligent, conversational weather assistant. Through the application of AI and cloud computing technology, businesses can automate complex queries and provide users with practical, valuable information.

---

## Additional Resources

To learn more about [Amazon Bedrock](https://aws.amazon.com/bedrock/), you can refer to:

- [GitHub repo: Amazon Bedrock Workshop](https://github.com/aws-samples/amazon-bedrock-workshop)
- [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)
- Workshop: Using Generative AI on AWS for diverse content types

To learn more about the Anthropic Claude 3.5 Sonnet model, see:

- [Anthropic's Claude in Amazon Bedrock](https://aws.amazon.com/bedrock/claude/)

---

## About the Authors

**Salman Ahmed** is a Senior Technical Account Manager at AWS Enterprise Support. He assists customers in the travel and hospitality industry with designing, deploying, and operating cloud infrastructure. With a passion for networking services and many years of experience, he helps customers effectively adopt AWS Networking solutions. Outside of work, Salman enjoys photography, traveling, and sports.

**Sergio Barraza** is a Senior Enterprise Support Lead at AWS, helping energy industry customers design and optimize cloud solutions. With a passion for software development, he guides customers in leveraging AWS services. Outside of work, Sergio is a musician who plays multiple instruments including guitar, piano, drums, and practices Wing Chun Kung Fu.

**Ravi Kumar** is a Senior Technical Account Manager at AWS Enterprise Support, helping travel and hospitality industry customers optimize their operations on AWS. He has over 20 years of IT experience and always focuses on operational efficiency. Outside of work, Ravi enjoys painting, playing cricket, and exploring new places.

**Ankush Goyal** is an Enterprise Support Lead at AWS Enterprise Support, helping customers simplify cloud operations. With over 20 years of experience, he focuses on optimizing infrastructure and delivering long-term value for businesses.
