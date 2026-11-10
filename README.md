# aws-microservices-alb# Microservices on AWS: Path-Based Routing with Auto Scaling

A microservices deployment on AWS (Mumbai, `ap-south-1`) where a single Application Load Balancer routes traffic by URL path to three independent services, each running in its own Auto Scaling group.

## Architecture


```
User -> Internet Gateway -> Application Load Balancer
                                 |-- /Cloth    -> TG-Cloth  -> ASG (Cloth)  -> EC2
                                 |-- /Default  -> TG-Home   -> ASG (Home)   -> EC2
                                 |-- /Laptop   -> TG-Laptop -> ASG (Laptop) -> EC2
```

### Components

| Component | Purpose |
|---|---|
| **VPC** | Isolated network in the Mumbai region (`ap-south-1`) |
| **Internet Gateway** | Lets users reach the load balancer from the internet |
| **Application Load Balancer** | Single entry point; routes requests by path |
| **Target Groups** | `TG-Cloth`, `TG-Home`, `TG-Laptop`: one per microservice |
| **Auto Scaling Groups** | One per service; scales EC2 instances up and down |
| **Launch Templates** | `Cloth`, `Home`, `Laptop`: define the AMI, instance type and setup for each ASG |
| **EC2 instances** | Run each microservice |
| **Availability Zones** | Instances are spread across `ap-south-1a` and `ap-south-1c` for availability |

### Routing rules

| Path | Target group | Service |
|---|---|---|
| `/Cloth` | TG-Cloth | Cloth service |
| `/Default` | TG-Home | Home service (default route) |
| `/Laptop` | TG-Laptop | Laptop service |

## Repository structure

```
.
├── README.md
├── docs/
│   ├── architecture.png
│   └── microserv1.drawio
├── cloth/      # Cloth microservice code
├── home/       # Home microservice code
└── laptop/     # Laptop microservice code
```

Adjust the folders above to match your actual project layout.

## Setup

1. Create a VPC with subnets in at least two Availability Zones and attach an Internet Gateway.
2. Create a launch template for each service (`Cloth`, `Home`, `Laptop`).
3. Create a target group for each service and register health checks.
4. Create an Auto Scaling group per service from its launch template, attached to its target group.
5. Create an Application Load Balancer and add listener rules:
   - `/Cloth*` -> TG-Cloth
   - `/Laptop*` -> TG-Laptop
   - default (`/Default`) -> TG-Home
6. Open the ALB DNS name in a browser and test each path.

## Tech stack

- AWS: VPC, EC2, Application Load Balancer, Auto Scaling, Launch Templates, Internet Gateway
- Add your application stack here (e.g. Node.js, Python/Flask, Java/Spring Boot, Docker)

## Author

Your name: https://github.com/shraddha-24-dev/shraddha-24-dev.git
