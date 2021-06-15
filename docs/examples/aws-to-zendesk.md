
# AWS EC2 & Freshservice CMDB integration

This example will demonstrate how to integrate AWS EC2 Events from EventBridge into Freshservice CMDB. The workflow is:
-Whenever a EC2 instance is stopped or started, events from AWS EventBridge will be sent to Direktiv
-If the event is of type `pending`, `stopping` or `stopped` it will only update the status of the CMDB CI item in Freshservice
-If the event is of type `running` or `stopped` 

## Event Listener Workflow YAML 

```yaml
id: eventbased-greeting
functions:
- id: greeter
  image: vorteil/greeting:v2
start:
  type: event
  state: greeter
  event:
    type: greetingcloudevent
description: "A simple action that greets you" 
states:
- id: greeter
  type: action
  action: 
    function: greeter
    input: '.greetingcloudevent'
  transform: '{ "greeting": .return.greeting }'
```

## GenerateGreeting Workflow YAML
```yaml
id: generate-greeting
description: "Generate greeting event" 
states:
- id: gen
  type: generateEvent
  event:
    type: greetingcloudevent
    source: Direktiv
    data: '{
      "name": "Trent"
    }'
```
