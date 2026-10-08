# AI-Infra Gateway + Car Assistant

> A cost-optimizing LLM gateway written in Go, with a car diagnostics and mod assistant (Python, FastAPI, Claude tool-calling, RAG) running on top of it. Deployed on AWS with Terraform.


## Why This Exists

Most LLM demos call a model API directly and never look at what each request costs. This project treats AI as an infrastructure problem. It has two layers:

1. **The Gateway:** a Go service that sits in front of multiple LLM providers (Claude, plus models reachable through OpenRouter), routes each request to the cheapest model that can handle it, and records the tokens and dollars spent on every call.
2. **The Car Assistant:** a real application built on the gateway. You ask it about OBD codes, mods, or diagnostics on a 2021 BMW M2 Competition, and it answers using my actual mod history and reference material instead of guessing.

The gateway has a real consumer, and the assistant runs on infrastructure I built and can explain end to end.

## Architecture

```
User → Car Assistant (FastAPI) → Gateway (Go) → LLM providers
                │                      │
                ▼                      ▼
          RDS (Postgres)         Token/cost logs
```


**AWS infrastructure:** VPC with public and private subnets, Application Load Balancer, EC2/container compute, RDS PostgreSQL, NAT Gateway. All provisioned with Terraform.

## Tech Stack

| Layer | Technology |
|---|---|
| Gateway | Go |
| Assistant | Python, FastAPI, Claude API (tool use), RAG |
| Database | PostgreSQL on AWS RDS |
| Infrastructure | Terraform, AWS |
| Containers | Docker |
| CI/CD | GitHub Actions |
| Observability | Logging, monitoring, token/cost dashboard |

## Repository Layout

```
terraform/   Infrastructure as code
gateway/     Go gateway service
assistant/   Python/FastAPI car assistant
docs/        Architecture notes and diagrams
```

## Running It Locally

<!-- Fill in once there's something to run -->

## Cost Notes

Infrastructure is provisioned and destroyed with Terraform between work sessions to keep AWS spend low. NAT Gateways and load balancers can't be paused, so they're torn down and recreated instead of left running. The target is roughly $20-40 total across the project.

## License

MIT. See [LICENSE](LICENSE).