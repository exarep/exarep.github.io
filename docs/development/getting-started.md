# Getting Started

Set up your local development environment to build and run Exarep services.

## Prerequisites

Install the following tools on your Fedora/RHEL workstation:

```bash
sudo dnf install java-latest-openjdk-devel maven podman git python3
```

Install Node.js (for frontend development):

```bash
sudo dnf install nodejs npm
npm install -g @angular/cli
```

## Clone Repositories

Clone the repositories you need into the organization folder:

```bash
mkdir -p ~/projects/github/exarep
cd ~/projects/github/exarep

git clone https://github.com/exarep/api-customer.git
git clone https://github.com/exarep/api-enrollment.git
git clone https://github.com/exarep/api-billing.git
git clone https://github.com/exarep/api-usage.git
git clone https://github.com/exarep/api-market.git
git clone https://github.com/exarep/api-product.git
git clone https://github.com/exarep/portal-customer.git
git clone https://github.com/exarep/portal-internal.git
```

## Run a Service

Start any API service in Quarkus dev mode:

```bash
cd ~/projects/github/exarep/api-customer
./mvnw quarkus:dev
```

Quarkus dev services will automatically start:

- **PostgreSQL** — local database with Flyway migrations applied
- **Kafka** — local broker for event streaming
- **Keycloak** — local OIDC provider with test realm

The service will be available at [http://localhost:8080](http://localhost:8080).

## Run a Portal

Start a frontend portal in development mode:

```bash
cd ~/projects/github/exarep/portal-customer
npm install
ng serve
```

The portal will be available at [http://localhost:4200](http://localhost:4200).

## Build a Container Image

Build a container image using Podman:

```bash
cd ~/projects/github/exarep/api-customer
./mvnw package -DskipTests
podman build -t api-customer:latest -f Containerfile .
```

## Run the Documentation Site

```bash
cd ~/projects/github/exarep/exarep.github.io
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve --livereload
```

The documentation site will be available at [http://localhost:8000](http://localhost:8000).

## Verify Your Setup

After starting a service in dev mode, verify it is running:

```bash
curl http://localhost:8080/q/health
```

You should receive a response indicating the service is healthy:

```json
{
  "status": "UP",
  "checks": []
}
```
