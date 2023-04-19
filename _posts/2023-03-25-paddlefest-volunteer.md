---
layout: post
title: "Making a bulletproof volunteer registration system"
date: 2023-03-25 11:44:36 -0400
image:
  path: https://og-image.xyz/og/Hardening Paddlefest/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

## The Task

Last year, I was a volunteer coordinator for Paddlefest, a 3-day event that brings together 100+ volunteers to help run a 3-day event. Due to the complexity of the event and the multitude of moving parts of volunteer participation, we needed a custom solution to manage our volunteers. We needed a system that could:

- Allow volunteers to sign up for shifts under specific roles
- Sort said shifts under different locations
- Allow volunteers to sign up for multiple shifts
- Collect waiver permissions

Last year we took a really good stab at an initial solution. The UI was excellent but the backend was not as reliable as we would have liked, leading to a lot of data loss and manual work to recover from oddities. This year, I wanted to further develop the system to create a more robust solution that could handle more traffic and be more resilient.

## The Problems

The first problem was that the system was just generally unreliable. This was caused by a few different factors:

- Rushed deployment led to cutting corners in development
- Poor testing that didn't cover a wide range of scenarios and potential mistakes
- Utilization of raw MongoDB instead of using a relational database or ORM to enforce data consistency.
- Complete lack of error handling and logging. High risk operations were not wrapped in try/catch blocks and errors were not logged.
- Deployment on a new, unstable kubernetes cluster that was not properly configured for production workloads.

## The Solution

The solution was to completely rewrite the backend and completely re-imagine the way data was stored and handled. The new system was built with the following goals in mind:

- **Reliability**: The system should be able to handle a large amount of traffic and be able to recover from errors without manual intervention.
- **Scalability**: The system should be able to handle a large amount of traffic without breaking.
- **Maintainability**: The system should be easy to understand and modify.
- **Security**: The system should be secure and not allow for any data tampering or unauthorized access.
- **Autonomy**: The system should be able to run without any developer intervention in future years.

This was no small task. The backend has been completely rewritten with tons of tiny new features and improvements.

### Backend is now written exclusively in node.js without any external shell scripts

Last year, we used a shell script to handle tasks like sending emails and managing database migrations and checks. This year, everything has been verticalized into node.js, allowing for a more reliable and debuggable system.

### App is now deployed on a stable kubernetes cluster with proper configuration for production workloads

Deployment on a Kubernetes cluster provides a lot of benefits, including scaling, health checks, deployment, and fatal error recovery out of the box. Kubernetes is not new to the Paddlefest volunteer system but this year we are using a stable cluster that has been properly configured for production workloads.

### Now using a relational database (MySQL) instead of MongoDB

Last year we used a database called MongoDB, which you could think of as saving data in independent files. These files do not connect to one another and are not guaranteed to be consistent. This led to some really weird bugs and behaviors last year where the way some data was stored randomly changed a few times throughout the cycle as a side-effect of unrelated code changes. This year we are using a relational database called MySQL, which is a lot more stable and predictable.

### Now using an ORM (Prisma) to enforce data consistency

We are not writing raw SQL queries anymore. Instead, we are using an ORM called Prisma to handle all of our database interactions. This allows us to write code that is more readable and maintainable while also enforcing data consistency. It also makes the developer experience far better because we can use autocomplete and type checking to ensure that we are writing valid code, which abstracts the frustration and risk of writing raw SQL queries.

### Now using a proper logging system and error handling system (Sentry) to proactively catch errors and fix them even without a bug report

Last year we were depending on users to report bugs and errors. This year, we are using a proper logging system (Sentry) to proactively catch errors and fix them even without a bug report. This will allow us to catch errors before they become a problem and fix them before they become a problem. This will help reduce the probability of major bugs going unnoticed and causing downtime or a reduced user experience.

### Improved API coverage to reduce the number of requests needed to get the same data, reducing load and improving performance

Previously, it took hundreds of API calls to get the basic information needed to render the volunteer registration system. This year, it has been drastically improved to 3 API calls. This will reduce the load on the server and improve performance.

### Improved API error handling to provide more useful error messages

Previously, the API would return a generic error message if something went wrong. This year, the API will return a more useful error message that will help know what went wrong and how to fix it, as well as providing the user with a reason that something went wrong.

### Revamped onboarding experience after a user clicks register

From the moment a user clicks register, we register their information in the database, claim the shifts they have registered for, and send them an email and text with detailed information about the event and their registration, as well as a link for them to view their registration.

### Rewritten email handler to send more accurate, readable, and beautiful emails

Last year emails were positively _okay_. They showed all the necessary information, but everything was text-based and hard to understand. This year we are using a new custom email handler that sends emails with a beautiful HTML template that is easy to read and understand, as well as a link to view the same information in a browser.

### Implemented a database migration strategy to allow for database schema updates without the risk of downtime or data loss

Last year, we had to manually update the database schema and manually migrate the data. This year, we are using a database migration strategy that allows us to update the database schema without the risk of downtime or data loss.

### Implemented hourly data backups to prevent data loss in the event of a database or app failure

This year we are starting to back-up data. The data will be backed up on the application server, the database server, and a storage bucket, allowing for both automatic and manual recovery in the event of a database or app failure.

### Implemented uptime monitoring to notify me if the app goes down

I have added the volunteer registration system to my uptime monitoring system, which will notify me if the app goes down. This will allow me to quickly fix the issue and get the app back up and running.

### Added a check at the beginning of the registration process to ensure that the server is healthy and ready to accept new users

This will prevent users from spending the time to register only to find out that the server is down or not ready to accept new users.

### Implemented automated cloud build and deployment to improve the time-to-production of new features

This year I implemented a GitHub action to build the docker container and automatically deploy it to the Kubernetes cluster. This will allow me to quickly deploy new features and fixes to the production environment.

### Implemented automated internal testing to ensure that new features don't break existing functionality

This year I implemented a testing library that will automatically run tests on the backend to ensure that new features don't break existing functionality.

## Remaining steps

The only big step that is left is to implement an admin panel. This will allow us to view, manage, and edit volunteers and shifts without messing with the database directly. This will also allow us to view and manage the data in a more user-friendly way.

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
