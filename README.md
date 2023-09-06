# Weather data state machine

![weather_monitor drawio](https://github.com/konami99/aws-microservice-data-fetcher/assets/166879/43487afb-b5ec-4b2c-84b8-a1ec66d29812)

This repo is the "state machine" part of the weather monitor. It is comprised of Step Functions and DynamoDB. Step Functions subscribes the weather data event in Event Bridge, and then insert the data into DynamoDB.

## Step Functions

This part says Step Functions subscribes the `demo.event` (I haven't got time to update this to a more appropriate name, but it's the weather event) from the default Event Bridge.

<img width="1023" alt="Screen Shot 2023-09-06 at 8 01 00 pm" src="https://github.com/konami99/aws-microservice-state-machine/assets/166879/a227879b-7475-4163-bddd-d26f8ad81837">
