# AWS Learning Notes

Notes from learning AWS services while building the **HomeScout** real estate application.

## Architecture Overview

| File | Description |
|------|-------------|
| [1. AWS-Architecture.md](1.%20AWS-Architecture.md) | High-level architecture steps for deploying a web app on AWS |
| [2. Problems I solve during the setup.md](2.%20Problems%20I%20solve%20during%20the%20setup.md) | Troubleshooting real issues encountered during setup |

## Networking & Security

| File | Description |
|------|-------------|
| [CIDR-Notation.md](CIDR-Notation.md) | Mental model for understanding IP address ranges and CIDR notation |
| [vpc-architecture.md](vpc-architecture.md) | VPC setup with public/private subnets, IGW, and route tables |
| [aws-network-isolation.md](aws-network-isolation.md) | Why EC2 lives in public subnets and RDS in private subnets |

## Compute & Deployment

| File | Description |
|------|-------------|
| [ec2.md](ec2.md) | EC2 instance setup, Node.js installation, and PM2 production deployment |

## Database

| File | Description |
|------|-------------|
| [aws-rds.md](aws-rds.md) | RDS setup (PostgreSQL), security groups, and Prisma P1001 debugging |

## Storage

| File | Description |
|------|-------------|
| [aws-s3.md](aws-s3.md) | S3 bucket creation, bucket policies, versioning, encryption, and CORS |

## API & Authentication

| File | Description |
|------|-------------|
| [aws-api-gateway.md](aws-api-gateway.md) | REST API Gateway setup with Cognito authorizers and public routes |
| [cognito-nextjs-auth.md](cognito-nextjs-auth.md) | AWS Cognito + Next.js integration using Amplify |
