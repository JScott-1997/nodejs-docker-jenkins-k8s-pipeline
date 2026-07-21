# CI/CD Pipeline: Node.js → Docker → Jenkins → Kubernetes

A small Node.js web service used to demonstrate a full containerised delivery pipeline: build, test, containerise, and ship to a Kubernetes deployment — all automated through Jenkins.

The application itself is intentionally simple; the point of this repo is the pipeline around it.

## What it does

The server responds to HTTP requests on port 8080 with a plain text message identifying the container that served the request — useful for confirming a rollout has actually reached the new version.

## Pipeline overview

Defined in the `Jenkinsfile`, the pipeline runs three stages on every push to `main`:

1. **Build** — checks out the source and builds a Docker image (`jscott1997/coursework2`).
2. **Test** — runs basic checks against the build.
3. **Deploy** — logs in to Docker Hub and pushes the new image, then connects to a production server over SSH and updates the `coursework2` Kubernetes deployment to roll out the new version.

```
push to main → Jenkins build → Docker image → Docker Hub → SSH to server → kubectl rollout
```

## Tech Stack

- **Node.js** — application server
- **Docker** — containerisation
- **Jenkins** — CI/CD orchestration
- **Kubernetes** — deployment target
- **Docker Hub** — image registry

## Running it locally

Requires Docker.

```bash
# Build the image
docker build -t coursework2 .

# Run it
docker run -p 8080:8080 -d coursework2

# Check it's alive
curl http://localhost:8080
```

Expected response:

```
DevOps Coursework 2! | Running on: <container_hostname> | v=3
```

## About

Built for the DevOps module of my BSc (Hons) Computing at Glasgow Caledonian University, as a hands-on exercise in continuous delivery to a real orchestration target rather than a toy example.
