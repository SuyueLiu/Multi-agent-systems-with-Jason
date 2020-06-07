# Introduce
This README file is built for the second coursework of intelligent information systems.

We consider agents to be systems that are situated in some environment, which means that agents are capable of sensing their environment(via sensors), and have a repertoire of possible actions that they can perform to modify their environment.

Multi-agent system which contains more than one agent, each agen has a "sphere of influence" in this environment.

# Scenario
*Since the spreading of COVID-19, restaurants are only allowed to provide the takeout service. This multi-agent system is a simulation of ordering food online. The customer agent is responsible for ordering the item he/she would like to have, he/she should take his/her bank balance into account when paying the bill. The restaurant agent is responsible for preparing food once he/she receive the order from customers, then tells the courier to collect the food. The owner of the restaurant should be concerned with the food stock. The courier agent is an agent with reactive behaviors, the courier reacts to the message(communication) from the restaurant — deliver food. Besides, the courier should also take the weather into account when he/she is delivering items.*

# Requirements
* Java SE 14.0.1
* Jason 2.4
* Eclipse 4.15.0

# Running Queries
The project consists of the following files:

| Files | Description|
|:--------:|:----------:|
|customer.asl | The AgentSpeak-style agent program for customer agents.|
|restaurant.asl | The AgentSpeak-style agent program for restaurant agents.|
|courier.asl | The AgentSpeak-style agent program for courier agents.|
|takeout.mas2j| the configuration file of the project|


Please run the takeout.mas2j in the Eclipse.

# Reference 
1. Bordini, R.H., Hübner, J.F. and Wooldridge, M., 2007. Programming multi-agent systems in AgentSpeak using Jason (Vol. 8). John Wiley & Sons.

2. The slides of Intelligent Information Systems in University of Bristol.