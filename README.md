# Task 3: Infrastructure as Code (IaC) with Terraform

## Objective
Provision a local Docker container (Nginx) using Terraform and the Docker provider.

## Tools Used
- Terraform
- Docker
- Kali Linux
- Git and GitHub

## Project Structure
```
Task3-Terraform-Docker/
├── main.tf              # Terraform configuration
├── README.md            # Documentation
├── .gitignore           # Excludes .terraform/ and state files
├── .terraform.lock.hcl  # Provider version lock file
└── logs/                # Execution logs for each step
```

## What main.tf Does
- **Provider:** `kreuzwerker/docker` (version 3.0.2), connected to the local Docker socket.
- **docker_image.nginx:** pulls the `nginx:alpine` image.
- **docker_container.nginx:** runs a container named `terraform-nginx`, mapping host port `8081` to container port `80`.
- **Resource dependency:** the container references `docker_image.nginx.image_id`, so Terraform creates the image first.

## Steps Performed
1. `terraform init`: downloaded the Docker provider (`logs/01-init.log`)
2. `terraform validate`: checked the configuration (`logs/02-validate.log`)
3. `terraform plan`: previewed the changes (`logs/03-plan.log`)
4. `terraform apply`: created the image and container (`logs/04-apply.log`)
5. `docker ps` and `curl -I localhost:8081`: verified the container is running (`logs/05-docker-ps.log`, `logs/06-curl.log`)
6. `terraform state list`: inspected tracked resources (`logs/07-state-list.log`)
7. `terraform destroy`: removed all resources (`logs/08-destroy.log`, `logs/09-after-destroy.log`)

## Troubleshooting Note
`terraform init` initially failed with `504 Gateway Timeout` from GitHub while downloading the provider. Pinning the provider to version `3.0.2` and retrying resolved it.

## Key Learnings
- IaC defines infrastructure in code, so it is repeatable and version controlled.
- `plan` previews changes safely, and `apply` makes them.
- The state file records what Terraform manages.
- Providers let Terraform talk to platforms such as Docker.
- Resource references create implicit dependencies.

## Author
Pratiksha Halloli
