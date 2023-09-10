# Qlik SSE (Node.js)

## What it is SSE

SSE stands for **server-side extension** or also know as **analytics connections**. SSE protocol is based on [gRPC](https://grpc.io/)

From Qlik documentation:

> With analytic connections you are able to integrate external analysis with your business discovery. An analytic connection extends the expressions you can use in load scripts and charts by calling an external calculation engine (when you do this, the calculation engine acts as a server-side extension (SSE))

IMHO I think that Qlik SSE is one of the great features of Qlik (in my personal top 5 at least). It's quite unfortunate that it has so less usage and attention.

## Project History

In the past [Miralem Drek](https://github.com/miralemd/) started this project to bring SSE to JavaScript/NodeJS world. And he did a fantastic job! Unfortunately he left Qlik, at some point after that, and its project was left unattended :(

## Project Future

Im picking this project for a few reasons:

- ~~SSL - gRPC have the ability to use SSL (certificates) to communicate with the client. Since we are talking about data being send back and forth then using SSL is a must!~~
- ~~dependencies update - since the original project was abandoned couple of years ago its dependencies are out of date~~
- features - make sure that the project cover all possibilities of SSE
- ES6 - at the moment the project is written in CommonJS. It has to be re-written to ES6 format
- examples - if possible provide more examples
- documentation - create place for full user documentation aka this site :)
