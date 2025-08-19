ADK Sample Agents — Practical Agent Examples and Templates
=========================================================

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/luisn18/adk-samples/releases) [![adk](https://img.shields.io/badge/topic-adk-lightgrey)](https://github.com/topics/adk) [![agent-samples](https://img.shields.io/badge/topic-agent--samples-lightgrey)](https://github.com/topics/agent-samples) [![agents](https://img.shields.io/badge/topic-agents-lightgrey)](https://github.com/topics/agents)

![Agent Illustration](https://upload.wikimedia.org/wikipedia/commons/6/64/Robot_icon.svg)

A curated set of sample agents built with the Agent Development Kit (ADK). Use this repo to learn agent patterns, test integrations, and run real demos. The collection covers agent templates, behavior examples, communication patterns, and deployment helpers.

Table of contents
- Overview
- What you get
- Sample agents
- Architecture and components
- Quick start — download and run release
- Run locally from source
- Examples and usage
- Configuration
- Tests and CI
- Contributing
- License
- Releases and changelog

Overview
--------
This repo targets developers who build autonomous agents with an ADK. It shows common patterns. It shows how to wire perception, decision, and action components. Each sample is small and focused. Each one includes code, tests, and a Dockerfile where relevant.

What you get
------------
- Minimal agent templates (stateless and stateful).
- Multi-agent examples (coordination and messaging).
- Integrations with common services (HTTP, WebSocket, message queues).
- Deployment examples (Docker, Compose, Kubernetes manifests).
- Test harnesses and mock providers.
- Scripts to package releases.

Sample agents
-------------
1. planner-agent  
   - Role: plan tasks and schedule actions.  
   - Tech: Python, ADK-planner module, Redis state backend.  
2. responder-agent  
   - Role: respond to queries and provide facts.  
   - Tech: Node.js, ADK-core, WebSocket API.  
3. monitor-agent  
   - Role: observe metrics and alert.  
   - Tech: Go, ADK-observe, Prometheus exporter.  
4. coordinator-agent  
   - Role: manage workflows across agents.  
   - Tech: Python, message broker (RabbitMQ), ADK-orchestrate.

Each sample has:
- README with design notes.
- Dockerfile and compose for local runs.
- Test suite and integration scripts.

Architecture and components
---------------------------
The samples follow a common layout. Each agent implements these layers:

- Input adapters  
  Connect to sensors, APIs, or message queues. They translate external events into ADK events.

- Perception  
  Validate and normalize incoming events. Add context and metadata.

- Decision core  
  Implement behavior. Use rules, state machines, or planners.

- Action adapters  
  Call external services. Emit messages or update stores.

- Persistence  
  Store minimal state. Use in-memory, Redis, or SQL depending on the sample.

- Health and metrics  
  Expose liveness endpoints and Prometheus metrics.

Diagram (example):
![Agent Flow](https://upload.wikimedia.org/wikipedia/commons/3/3f/Flowchart_example.svg)

Quick start — download and run release
-------------------------------------
Download the packaged release asset from the Releases page and run the included installer. Visit the release page, download the release archive, extract it, and run the provided script.

- Visit and download: https://github.com/luisn18/adk-samples/releases
- Typical steps after download:
  1. Extract the archive: tar xzf adk-samples-vX.Y.Z.tar.gz
  2. Change into folder: cd adk-samples
  3. Make the installer executable: chmod +x install.sh
  4. Run the installer: ./install.sh

The release builds include ready-to-run images, example configs, and a launcher script. The installer sets up local Docker images and creates example configs under ./deploy.

Run locally from source
----------------------
Use Docker Compose for most samples. The repo includes compose files per sample.

Prerequisites:
- Docker
- Docker Compose
- Git
- Python 3.10+ (for Python agents) or Node 16+ (for Node agents)

Clone and run:
- git clone https://github.com/luisn18/adk-samples.git
- cd adk-samples
- docker compose -f samples/planner-agent/docker-compose.yml up --build

Start logs:
- docker compose logs -f planner-agent

Examples and usage
------------------
planner-agent example (shell):
- Export config:
  export PLANNER_CONFIG=./samples/planner-agent/config/local.yaml
- Run locally:
  ./samples/planner-agent/run.sh
- Call API:
  curl -X POST http://localhost:8080/plan -d '{"task":"backup-db"}' -H "Content-Type: application/json"

responder-agent example:
- Start WebSocket server:
  node samples/responder-agent/server.js
- Connect with a client:
  wscat -c ws://localhost:3000
- Send query:
  {"query":"current-status"}

monitor-agent example:
- Build:
  docker build -t monitor-agent samples/monitor-agent
- Run:
  docker run --rm -p 9100:9100 monitor-agent
- Check metrics:
  curl http://localhost:9100/metrics

Configuration
-------------
All agents read configuration from:
- ENV variables (preferred for container runs)
- Local YAML/JSON files (for development)
- Config server (optional; examples provided)

Standard config keys:
- ADK_ENV: runtime environment (dev, staging, prod)
- LOG_LEVEL: debug|info|warning|error
- BROKER_URL: amqp://user:pass@broker:5672
- STATE_BACKEND: redis://redis:6379/0

Overrides:
- Set ENV variables to override file values.
- Use secret management for production credentials.

Tests and CI
------------
Each sample includes unit tests and integration tests.

Run tests:
- Python agents: python -m pytest samples/planner-agent/tests
- Node agents: npm test --prefix samples/responder-agent
- Go agents: go test ./samples/monitor-agent/...

CI pipeline:
- Linting and static checks
- Unit tests in containers
- Build sample Docker images
- Run integration tests against ephemeral services (Docker Compose)

Contributing
------------
Follow the process:
- Fork the repo.
- Create a feature branch.
- Write tests for new behavior.
- Keep commits small and focused.
- Open a pull request with a clear description.
- Use the issue tracker to discuss major changes.

Coding conventions:
- Python: use black and flake8.
- Node: use eslint with the repo config.
- Go: gofmt and vet.

Issue labels:
- bug
- enhancement
- question
- help wanted

License
-------
This repo uses the MIT License. See LICENSE for full text.

Releases and changelog
----------------------
Find packaged releases on the Releases page. Each release contains assets and an installer script. Download the release asset and run the included installer script to set up local demos.

Release page:
[Release downloads and assets](https://github.com/luisn18/adk-samples/releases)

Changelog highlights live in CHANGELOG.md. Each release tag lists notable changes, breaking API changes, and upgrade steps.

Tips and patterns
-----------------
- Keep agents small and single-purpose. One responsibility per agent aids testability.
- Use message brokers to decouple agents. Publish-subscribe models simplify scaling.
- Prefer event sourcing for audit needs.
- Use health checks and metrics for operational visibility.
- Keep configs outside images. Use mounts or secret stores.

Files of interest
-----------------
- samples/* — sample agents and their artifacts.
- scripts/release.sh — build and package release assets.
- scripts/install.sh — example installer used in releases.
- docs/ — design notes and pattern docs.
- .github/workflows/ — CI definitions.

Contact and support
-------------------
Open issues for bugs or feature requests. Use PRs for fixes and additions. Tag maintainers where needed.