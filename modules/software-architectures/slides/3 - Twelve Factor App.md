# The Twelve Factor App

One of the early pioneers laying claim to territory in the public cloud market was [Heroku](https://www.heroku.com/). It offered to host your application for you, and all you had to do was build your application and push it via git, and then the cloud took over, and your application magically worked online.

The problem was that most people simply had no idea how to build applications in a way that was *cloud friendly*.

To solve this problem (and to increase their own platform adoption), a group of people within Heroku developed the [12 Factors](https://12factor.net/) in 2012. This is essentially a manifesto describing the rules and guide‐lines that needed to be followed to build a cloud-native applications.

## Codebase
One codebase tracked in revision control, many deploys

## Dependencies
Explicitly declare and isolate dependencies

## Configuration
Store configuration in the environment

## Backing Services
Treat backing services as attached resources

## Build, release, run
Strictly separate build and run stages

## Processes
Execute the app as one or more stateless processes

## Port binding
Export services via port binding

## Concurrency
Scale out via the process model

## Disposability
Maximize robustness with fast startup and graceful shutdown

## Dev/prod parity
Keep development, staging, and production as similar as possible

## Logs
Treat logs as event streams

## Admin processes
Run admin/management tasks as one-off processes

## Resources
- [What is 12-Factor App?](https://www.youtube.com/watch?v=1OhmRmMsGdQ)
- [Beyond the Twelve-Factor App](../../../books/beyond-the-twelve-factor-app.pdf)

