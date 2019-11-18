# Entry 2
##### 11/17/2019

## Change of plans
Our idea of making a game has changed since the last blog post. We decided that we didn't really want to make a survival game or even a mobile game altogether. We didn't feel like making a game that needed to be downloaded anymore. We wanted to make a game that requires only a browser where users can start playing as soon as possible without the need of downloading and installing. Since we aren't making a mobile game anymore, our main tool is now [three.js](https://threejs.org/). This decision was heavily influenced by other browser games such as [Krunker](https://krunker.io/) and the three.js web demo [Infinitown](http://demos.littleworkshop.fr/infinitown).

## The New Game Idea
Our new game will still be multiplayer, however it will instead be a game where players race each other in a track/map with obstacles and are able to collect power ups just like in Mario Kart. There will be a singleplayer/career mode, multiplayer with the ability to queue with friends, and a community mode. The community mode allows players to make their own maps and change almost every aspect of the game, including vehicle physics such as torque/acceleration and even the time of day. Players will be able to unlock new vehicles and upgrade them.

## Tools
1. [Node.js](https://nodejs.org/en/) - We chose Node.js because since both me and my partner knows JavaScript, it is an easy choice because Node.js is really lightweight and JavaScript is really easy to use to start prototyping something.
2. [TypeScript](https://www.typescriptlang.org/) - We might also use TypeScript because JavaScript lacks static type checking. Static type checking might be beneficial in our workflow because in some situations we can't always guarantee that the type is what we think it is, so it might be wise to have compile time errors to improve code safety.
3. [Three.js](https://threejs.org/) - It's a framework for WebGL. It will help speed up development since it will eliminate WebGL boilerplates.
4. Databases - We haven't decide on one yet since we want to try something other than MongoDB
5. WebSockets - It's not really a tool, but its a  protocol that allows the game to have a connection that is kept alive with the server, which allows for duplex communcation. In other words, it allows the client and server to talk to each other using one connection. This is the best option for low latency since there is almost no overhead unlike other methods such as HTTP long polling, etc...

## Engineering Design Process
Since this game will be quite large, it wouldn't really make sense to build the backend as a monolithic application. The game will consists of services for authentication, user statistics, maps listings, etc and it will be insane if we built everything as a single application as it will be probably virtually impossible to manage the code and reliability becomes questionable since the logic will be so tightly coupled together which will probably pose a problem later down the line.

### Microservices
Microservices is an architecture where it will eliminate the problems that can occur with a monolithic architecture. The goal of microservices is so that the different parts that make up your application are as loosely coupled as possible. The benefits of this is that we can split each part of the application into its own microservice, so that it is independent and will not affect other parts of our application.

Here is the draft of our microservice architecture
![](https://i.imgur.com/PCf6cCq.png)

The WebSocket gateway is just there temporarily the structure will probably change depending on our needs.

But basically theres 2 really important things here
1. **API Gateway** - The entry point of our microservices. It will forward the request to the corresponding services
2. **Service Discovery** - All microservices will basically register themselves to here so that it becomes discoverable by the API gateway. This allows us to **not** hardcode our microservice location's on the network by IP and port as they can always be changing. It will also be responsible for health checks on each microservice and we will probably make a public dashboard to view the health of each microservice or something.

Another thing to notice is that none of the services share the same database. This means that each service is only responsible for their own data.

What is powerful about this architecture is that no services depend on each other. In a traditional monolithic application, since the logic is tightly coupled, it is not uncommon for something like the statistics API route to break, and the problem bubbles up to other parts of the application. With this architecture, if any one of the services break, you can guarantee that absolutely no other service will be affected.

Additionally, since each service isn't tightly coupled together, they can be written using completely different tools/languages. And because they also don't share the same database, one service can be using MongoDB as a database while another uses PostgreSQL.

In the future, if for whatever reason one service needs to communicate to another, we can implement a message broker or something.

In addition to microservices, we will write our code using the [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) to promote independence and flexibility within our microservices.

![](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

# Skills
The research into microservices develops two skills since the previous entry.

1. Problem Decomposition - By learning about microservices it also shows me how to break down a problem into smaller areas to research in.
2. Communication - By drawing diagrams, it makes it easier to communicate my point across to the audience

[Previous](entry01.md) | [Next](entry03.md)

[Home](../README.md)
