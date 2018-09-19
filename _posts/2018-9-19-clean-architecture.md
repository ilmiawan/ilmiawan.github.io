---
layout: post
title: Clean Architecture
---

Architecture in software engineering is the blueprint of an applications. A must known, or must be decided first, for every engineer in a team of development before they are started to even touch the code for development process.

An architecture define the structure, layers, components, and it's relationships in a software applications. There are many different types of architecture in software developments. [Monolithic](https://en.wikipedia.org/wiki/Monolithic_application), [Data-Centric](https://en.wikipedia.org/wiki/Database-centric_architecture), [SOA](https://en.wikipedia.org/wiki/Service-oriented_architecture), [microservices](https://en.wikipedia.org/wiki/Microservices) is to name a few of architectural patterns and and styles. 

One of the concept is _Clean Architecture_. An architecture that is designed for the inhabitants of the architecture, not for the architect ... or even for the machine. A clean architecture should be simple, understandable, flexible for any changes, emergent, testable and maintainable. 

_"The first concern of the architect is to make sure that the application is usable, it isn't to ensure what it was made of."-uncle bob-_

**A Clean Architecture** - should be based on the domain centric with a clear separation of concerns. Layers of concerns are kept or made for a clear separations. i.e: 

**1. Presentation Layer**: Deals directly with user, taking in requests and sending out response to the users.

**2. Application (API) Layer**
Place for business applications, where all the business processes should be placed.

**3. Persistence Layer**
Connect to the database, places for data access and data connections to surrounding systems.

**4. Infrastructure Layer**
Is the places where the application make connection to the system surroundings and infrastructure.

**5. Domain Layer**
Is the model layers where the application is mapping its domain.







![_config.yml](/images/clean_architecture.png)
