# Weather data state machine

![weather_monitor drawio](https://github.com/konami99/aws-microservice-data-fetcher/assets/166879/43487afb-b5ec-4b2c-84b8-a1ec66d29812)

Part 1 "data fetcher" can be found [here](https://github.com/konami99/aws-microservice-data-fetcher)

This repo is the "state machine" part of the weather monitor. It is comprised of Step Functions and DynamoDB. Step Functions subscribes the weather data event from Event Bridge, and then insert the data into DynamoDB.

## Step Functions

This part says Step Functions subscribes the `demo.event` (I haven't got time to update this to a more appropriate name, but it's the weather event) from the default Event Bridge.

`template.yml`

<img width="1023" alt="Screen Shot 2023-09-06 at 8 01 00 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/a227879b-7475-4163-bddd-d26f8ad81837">

How was the Step Functions definition (`workflow.asl.json`) generated? Well I followed [AWS's suggestion](https://catalog.workshops.aws/stepfunctions/en-US/development/iac/deploy-with-terraform/step-2) to built my first version of flow on Workflow Studio. Here's what it looks like in Workflow Studio:

<img width="512" alt="Screen Shot 2023-09-06 at 8 15 10 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/67751b82-ab75-42bc-94de-bad2c5e9bbb4">

Once I was happy with the flow, I exported the flow to json, and put it in my project folder (`statemachine/workflow.asl.json`).

The rule is simple: if temperature, humidity, time and city exist, insert the weather data into DynamoDB. Otherwise go to end state (do nothing).

`statemachine/workflow.asl.json`

<img width="475" alt="Screen Shot 2023-09-06 at 9 23 49 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/c007d734-fb6b-46a6-9468-fed181efad34">

## Testing Step Functions

There are some resources to test Step Functions. I encourage you to take a look.

1. Data flow simulator

<img width="1043" alt="Screen Shot 2023-09-06 at 9 26 07 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/3f7f31e3-2dde-4712-8f70-36febfe3d0b7">

2. Local development tools

<img width="933" alt="Screen Shot 2023-09-06 at 9 28 20 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/c2828c5b-921f-4720-8c85-1b13c7d8f388">

## Step Functions Transformation

Step Functions has various input and output variables. I found [this blog and its sample application](https://aws.amazon.com/blogs/compute/using-jsonpath-effectively-in-aws-step-functions/) are really helpful explaining them.

## DynamoDB

Weather data is being inserted into DynamoDB by Step Functions. Here's the definition of DynamoDB table:

`template.yml`

<img width="506" alt="Screen Shot 2023-09-06 at 9 33 05 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/55603fab-bd5e-4016-8a4a-29ac91b8bffe">

Hash Key is "city" and Range Key is "datetime" (in unix time) because I want to query the DynamoDB by city, and sort the temperature data by time.

Here's what data looks like in DynamoDB:

<img width="1061" alt="Screen Shot 2023-09-06 at 9 37 24 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/a3d7b30b-502c-4f00-ab77-ec74299b0d53">
