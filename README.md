

# Automate a self-service Knowledge repository with Amazon EventBridge and Zendesk

![Zendesk Guide widget](/images/Screenshot1.png "Zendesk Guide widget")


# amazon-eventbridge-integration-with-auth0

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

## Requirements

* AWS CLI already configured with Administrator permission
* [NodeJS 12.x installed](https://nodejs.org/en/download/)
* A Zendesk account Amazon EventBridge integration configured [Instructions](https://support.zendesk.com/hc/en-us/articles/360043496933-Setting-up-the-events-connector-for-Amazon-EventBridge)
* A front end application  [Fresh Tracks](https://support.zendesk.com/hc/en-us/articles/360043496933-Setting-up-the-events-connector-for-Amazon-EventBridge)

## Installation Instructions

1. [Create an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) if you do not already have one and login.

1. Clone the repo onto your local development machine using `git clone`.

1. From the command line, change directory into the root, then run:
```
sam build
sam deploy --guided
```
## How it works

1. A user submits a question via the web widget, which is received within Zendesk as a support ticket.
1. Events are emitted from Zendesk when a support ticket is resolved.
1. These events are streamed into a custom SaaS event bus in EventBridge.
1. Event rules match events and send them downstream to an AWS StepFunction Express workflow
1. The Express workflow orchestrates Lambda functions to retrieve additional information about the event with the Zendesk API.
1. A Lambda function uses the Zendesk API to publish a new help article from the support ticket data.
1. The new article is instantly available on the website widget for other users to read.

Follow the prompts in the deploy process to set the stack name, AWS Region and other parameters.

## ZendeskEventBusName

* ZendeskEventBusName: A valid custom Event Bus for Zendesk Events (custom event bus names are genrated by the event source).

## ZenDeskDomain

* ZenDeskDomain: Your Zendesk Domain name

## ZenDeskPassword

* ZenDeskPassword: A valid Zebdesk API key

## ZenDeskUsername

* Auth0EventBusName: Your zendesk username

### The AWS Lambda functions

#### CreateZendeskArticle.js

This function Uses Zendsk API to create a new article in your Zendesk Help center.
You will need to replace the values for variables named `section` and `permission_group`  to the corresponding values in your own help center's section and permission group id's

#### GetFullZendeskUser.js

This function Uses Zendsk API to retrieve a full user record

#### GetFullZendeskTicket.js

This function Uses Zendsk API to retrieve the full support ticket data

### StepFunction Express Workflow

![Orchestration Workflow](/images/Screenshot2.png "Orchestration Workflow]")
