# Sending messages to Service Bus Queues/Topics

This section describes general requirements for all messages sent to Byggtjeneste Service Bus Queues and/or Topics.

## SessionId
For us to be able to process messages from queues in order, and at scale, we have enabled "Message sessions" for all our queues. With sessions enabled we will be able to handle related messages in the order that they where added to the queue.

When sessions is enabled for a queue all messages sent to the queue needs to have a SessionId set, unless the message will fail. The Message object from the Microsoft.Azure.ServiceBus namespace, that should be used for alle messages sent to Service Bus, has a SessionId property that needs to be set before sending the message.

The value for the SessionId property will depend on the messagetype, and is specified for each message type in this repository. For eg. "Item" events the SessionId will be "Nobb number". 

General info about sessions in Azure Service Bus can be found here: https://docs.microsoft.com/en-us/azure/service-bus-messaging/message-sessions

Messages and payloads:
https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messages-payloads

https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.message?view=azure-dotnet

## CorrelationId
To be able to uniquely identify and track a message through the whole process from Avensia Middleware to our receiving endpoints, we would like to have a common identifier that needs to be set by the Avensia Middleware. 

The Message object that will be used for sending messages to our queue already has a built-in property called "CorrelationId" to support these kinds of scenarios. 
https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.message.correlationid?view=azure-dotnet#Microsoft_Azure_ServiceBus_Message_CorrelationId

This property should then be assigned a new Guid when a message is created by the Avensia Middleware.