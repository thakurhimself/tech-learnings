# NestJS: a framework for building efficient, scalable Node.js server-side appso
## Introduction
Under the hood, It uses Express (the default) and fastify. It provides a level of
abstraction above these common NodeJS frameworks, but also exposes the API direct-
tly to the developer.

It solves the architecture problem. It provides the out-of-the-box application 
architecture and helps you create highly testable, scalable, loosely coupled, and
easily maintainable apps. It's inspired by Angular.

### Installation
Nest CLI

`
	npm i -g @nestjs/cli
	nest new project-name
`


## Overview
### First Steps
Core fundamentals - essential building blocks of Nest apps.
To create a Nest application instance, we use the core NestFactory class. 
NestFactory exposes a few static methods that allow creating an application 
instance. The create() method returns an application object, which fulfills the 
INestApplication interface. 

### Controllers
Controllers are responsible for handling incoming requests and sending response
back to the client. It's puspose is to handle specific requests for the applicatio
n. The routing mechanism determines which controller will handle each request. 
Often a controller has multiple routes, and each route can perform a different
action.
To create a basic controller, we use classes and decorators. Decorators link 
classes with the necessary metadata, allowing Nest to create a routing map that 
connects requests to their corresponding controllers.
