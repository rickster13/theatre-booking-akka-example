# AWS EC2 Container Service (ECS) deployment #

## Pre-requisites ##
- An [ECS cluster](https://github.com/LoyaltyOne/ecs-cluster-akka) 
- [Docker-Mirror](https://github.com/LoyaltyOne/docker-mirror) running on every single EC2 container instance belonging 
to the cluster
- [Application Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
- [Access](https://github.com/LoyaltyOne/bazooka) to a [ZooKeeper ensemble](https://zookeeper.apache.org)
- [ecs-service](https://github.com/ukayani/ecs-service)

### A note on ECS Service ###
`ecs-service` is used to isolate the environment from the application. We extract the environment variables into `.env` files so we can have versatile deployments (deploy to many different environments like dev, staging, prod).

## Deployment ##
In the `dev.params.json` file, we have specified the target ECS cluster. The `service.json` describes how to deploy
this application as an ECS Service and also route traffic to the service via the Application Load Balancer. 

To deploy [version 0.8](https://hub.docker.com/r/loyaltyone/theatre-example/tags) of the application, we use ecs-service:
```bash
ecs-service deploy dev-theatre-example 0.8 env/service.json env/dev.params.json -e env/dev.env 
```

A CloudFormation stack will be created named `dev-theatre-example` and that can be used to monitor the creation process.
Once the stack has been created successfully, you can make sure the service works by accessing:
```bash
curl -k https://<ALB URL>/my-theatre/health
```

### Further Details ###
If you want to know more about the deployment process, see [here](https://github.com/LoyaltyOne/theatre-booking-akka-example/wiki/AWS-ECS-Deployments)
