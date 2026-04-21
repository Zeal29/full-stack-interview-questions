# CI/CD & DevOps Basics Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) · Intermediate (Q21–Q40) · Advanced (Q41–Q50)

---

## Basic (Q1–Q20)

### Q1: What is CI/CD?

**Answer:**
- **CI (Continuous Integration)** — developers merge code frequently (daily), triggering automated builds and tests
- **CD (Continuous Delivery/Deployment)** — automated release process. Delivery = one-click deploy. Deployment = fully automatic deploy to production

**💡 Tip:** CI = integrate often, test automatically. CD = deliver/deploy automatically. CI catches bugs early, CD ships faster.

---

### Q2: What is the difference between Continuous Delivery and Continuous Deployment?

**Answer:**
- **Continuous Delivery** — every change is automatically built, tested, and staged for release. A human approves the final deployment to production
- **Continuous Deployment** — every change that passes all tests is automatically deployed to production without human approval

**💡 Tip:** Delivery = "ready to ship, press button to deploy." Deployment = "ship automatically, no button needed." Deployment requires more confidence in tests.

---

### Q3: What is a CI/CD pipeline?

**Answer:** An automated sequence of steps that code goes through from commit to deployment: build → test → lint → security scan → staging → deploy. Defined in configuration files (YAML) and triggered by events (push, PR, tag).

**💡 Tip:** Pipeline = assembly line for code. Each stage is a quality gate. Fail fast — catch issues in the earliest possible stage.

---

### Q4: What is Jenkins?

**Answer:** An open-source automation server for building, testing, and deploying software. It uses pipelines (Declarative or Scripted) defined in Jenkinsfiles. Highly extensible via plugins (1800+).

**💡 Tip:** Jenkins = the grandparent of CI/CD. Powerful but maintenance-heavy. Newer alternatives: GitHub Actions, GitLab CI, CircleCI. Still widely used in enterprise.

---

### Q5: What is GitHub Actions?

**Answer:** GitHub's CI/CD platform integrated directly into repositories. Workflows are defined in YAML files (`.github/workflows/`). Triggered by events (push, PR, schedule). Free for public repos, generous free tier for private.

**💡 Tip:** GitHub Actions = CI/CD built into GitHub. No external server needed. Marketplace has thousands of reusable actions.

---

### Q6: What is GitLab CI/CD?

**Answer:** GitLab's built-in CI/CD system configured via `.gitlab-ci.yml` at the repo root. Uses runners (shared or specific) to execute jobs. Integrated with GitLab's merge request and review workflow.

**💡 Tip:** GitLab CI = all-in-one platform. Repo + CI/CD + registry + monitoring. `.gitlab-ci.yml` defines stages, jobs, and runners.

---

### Q7: What is a build stage in CI?

**Answer:** The stage where source code is compiled, dependencies are installed, and artifacts (binaries, Docker images, bundles) are created. It ensures the code can actually be built successfully.

**💡 Tip:** Build = "can this code compile?" Fail here = syntax errors, missing dependencies, or incompatible changes. Fast feedback.

---

### Q8: What is a deployment stage?

**Answer:** The stage where the built artifact is deployed to a target environment (staging, production). Methods: rolling update, blue-green, canary. Should be automated and repeatable.

**💡 Tip:** Deployment = putting the built product on the shelf. Automate everything. Never SSH into a server and manually deploy.

---

### Q9: What is a staging environment?

**Answer:** A production-like environment used for final testing before deploying to actual production. It mirrors production configuration to catch environment-specific issues.

**💡 Tip:** Staging = the dress rehearsal. As close to production as possible. Test here first, then promote to production with confidence.

---

### Q10: What is DevOps?

**Answer:** A culture, set of practices, and tools that combine software development (Dev) and IT operations (Ops) to shorten the development lifecycle and deliver high-quality software continuously.

**💡 Tip:** DevOps = Dev + Ops working together, not in silos. Culture > Tools. It's about collaboration, automation, and shared responsibility.

---

### Q11: What is infrastructure as code (IaC)?

**Answer:** Managing and provisioning infrastructure (servers, networks, databases) through machine-readable configuration files rather than manual processes. Tools: Terraform, Pulumi, AWS CloudFormation, Ansible.

**💡 Tip:** IaC = version control for infrastructure. Treat servers as code — reproducible, reviewable, testable, disposable.

---

### Q12: What is Terraform?

**Answer:** An open-source IaC tool by HashiCorp that lets you define cloud resources in declarative HCL files. It plans and applies changes to reach the desired state. Supports all major cloud providers.

**💡 Tip:** Terraform = "describe what you want, not how to build it." `terraform plan` (preview changes), `terraform apply` (execute changes). State file tracks real resources.

---

### Q13: What is Ansible?

**Answer:** An open-source configuration management tool that automates server setup, application deployment, and task execution. Agentless (uses SSH). Playbooks define tasks in YAML.

**💡 Tip:** Ansible = remote control via SSH. No agent needed on target machines. Idempotent — run the same playbook multiple times safely.

---

### Q14: What is a artifact in CI/CD?

**Answer:** A file or collection of files produced during the build process: compiled binaries, Docker images, JAR/WAR files, npm packages, zip archives. Artifacts are stored and used in later pipeline stages.

**💡 Tip:** Artifact = the finished product from the build stage. Build once, deploy everywhere. Never rebuild for each environment.

---

### Q15: What is a rollback?

**Answer:** Reverting a deployment to the previous working version when a new deployment fails or causes issues. Strategies: redeploy previous artifact, switch traffic back (blue-green), or restore database snapshot.

**💡 Tip:** Rollback = the undo button for deployments. Always have a rollback plan. Blue-green deployments make rollbacks instant (switch traffic back).

---

### Q16: What is blue-green deployment?

**Answer:** A deployment strategy with two identical environments: blue (current production) and green (new version). Deploy to green, test it, then switch traffic from blue to green. Rollback = switch back to blue.

**💡 Tip:** Blue-green = two identical stages. Swap the spotlight. Zero-downtime deployment and instant rollback. Requires double infrastructure.

---

### Q17: What is canary deployment?

**Answer:** Gradually rolling out a new version to a small percentage of users first (e.g., 5%), monitoring for errors, then increasing to 100% if healthy. Named after canary in a coal mine — early warning system.

**💡 Tip:** Canary = "let a few users try it first." If metrics look bad, stop the rollout. If good, gradually increase traffic. Lower risk than full rollout.

---

### Q18: What is a rolling deployment?

**Answer:** Gradually replacing instances of the old version with the new version, one or a few at a time. The service remains available throughout. If issues arise, stop the rollout and keep the remaining old instances.

**💡 Tip:** Rolling = "slowly swap old for new." No extra infrastructure needed. Slower than blue-green but resource-efficient.

---

### Q19: What is a pipeline trigger?

**Answer:** An event that starts a CI/CD pipeline execution. Common triggers: git push, pull request created, tag created, scheduled (cron), manual trigger, or webhook from external systems.

**💡 Tip:** Triggers = the starting gun. Push to main → full pipeline. PR created → build + test only. Tag → build + test + deploy to production.

---

### Q20: What is a YAML pipeline file?

**Answer:** A configuration file that defines the CI/CD pipeline stages, jobs, scripts, and conditions. Location varies by platform: `.github/workflows/*.yml` (GitHub), `.gitlab-ci.yml` (GitLab), `Jenkinsfile` (Jenkins).

**💡 Tip:** YAML pipeline = the recipe for your automation. Version-controlled alongside code. Changes to pipeline = PR + review.

---

## Intermediate (Q21–Q40)

### Q21: What is the difference between a CI/CD pipeline and a deployment pipeline?

**Answer:**
- **CI/CD pipeline** — the entire automation from code commit to production: build, test, security, deploy
- **Deployment pipeline** — specifically the stages that take a build artifact through environments (dev → staging → production)

**💡 Tip:** CI/CD = the whole factory. Deployment pipeline = the shipping department. CI/CD includes deployment, but deployment is just one part.

---

### Q22: What are GitHub Actions runners?

**Answer:** Servers that execute GitHub Actions workflows. GitHub-hosted runners (Ubuntu, Windows, macOS) are free with limits. Self-hosted runners run on your own infrastructure for custom environments or higher specs.

**💡 Tip:** GitHub-hosted = convenient, isolated, ephemeral. Self-hosted = full control, persistent, no minute limits. Use self-hosted for special hardware or security requirements.

---

### Q23: What is a matrix build?

**Answer:** Running the same job across multiple configurations simultaneously (e.g., Node 18 + 20, Ubuntu + macOS, different browsers). Defined in the pipeline YAML with a matrix strategy.

**💡 Tip:** Matrix = "test everywhere at once." `{ node: [18, 20], os: [ubuntu, macos] }` = 4 parallel jobs. Catches compatibility issues early.

---

### Q24: What is a Docker registry in CI/CD?

**Answer:** A repository for storing and distributing Docker images. During CI, built images are pushed to a registry (Docker Hub, ECR, GCR). During CD, the deployment pulls the image from the registry.

**💡 Tip:** Registry = the artifact store for Docker. Build once → push to registry → pull from any environment. ECR for AWS, GCR for GCP.

---

### Q25: What is configuration drift?

**Answer:** When the actual state of infrastructure diverges from the defined state in IaC. Caused by manual changes, ad-hoc fixes, or inconsistent updates. IaC tools detect and correct drift.

**💡 Tip:** Drift = servers becoming "special snowflakes." IaC prevents drift. Terraform's `plan` shows drift. Ansible converges back to desired state.

---

### Q26: What is GitOps?

**Answer:** A practice where Git is the single source of truth for infrastructure and application configuration. Changes are made via Git commits, and automated tools (ArgoCD, Flux) sync the desired state to the cluster.

**💡 Tip:** GitOps = IaC + Git + automation. Git commit → ArgoCD detects change → applies to Kubernetes. If something drifts, Git wins.

---

### Q27: What is ArgoCD?

**Answer:** A declarative, GitOps continuous delivery tool for Kubernetes. It watches Git repositories and automatically syncs Kubernetes manifests to the cluster when changes are detected.

**💡 Tip:** ArgoCD = GitOps for Kubernetes. Git is the source of truth, ArgoCD is the enforcer. Visual dashboard shows sync status and differences.

---

### Q28: What is a pipeline secret?

**Answer:** Sensitive data (API keys, passwords, tokens) used in CI/CD pipelines. Stored securely in the CI platform's secret store (GitHub Secrets, GitLab Variables) and injected at runtime. Never hardcode in YAML or code.

**💡 Tip:** Secrets = injected, not committed. GitHub Secrets → `${{ secrets.API_KEY }}`. Rotate regularly. Audit who has access.

---

### Q29: What is pipeline caching?

**Answer:** Storing and reusing dependencies/build outputs between pipeline runs to speed up builds. Common caches: `node_modules`, `.cache`, Docker layers, pip packages. Reduces build time from minutes to seconds.

**💡 Tip:** Cache = "don't download `node_modules` every time." Cache key = `package-lock.json` hash. Invalidate cache when dependencies change.

---

### Q30: What is a smoke test in CI/CD?

**Answer:** A quick set of tests run after deployment to verify the most critical functionality works. If smoke tests fail, the deployment is automatically rolled back. They are the first quality gate in production.

**💡 Tip:** Smoke test in CD = "is the app alive?" Health check endpoint, login works, homepage loads. Fast (< 1 min). Fail = instant rollback.

---

### Q31: What is a feature flag in CI/CD?

**Answer:** A toggle that enables or disables features without deploying new code. Used to: deploy features turned off, enable for specific users (canary), or kill a feature instantly without rollback.

**💡 Tip:** Feature flag = a light switch for features. Deploy code safely → enable gradually. "Kill switch" for emergencies. Tools: LaunchDarkly, Unleash.

---

### Q32: What is SonarQube / SonarCloud?

**Answer:** A static code analysis platform that checks code quality: bugs, vulnerabilities, code smells, test coverage, and technical debt. Integrated into CI pipelines for continuous quality gates.

**💡 Tip:** SonarQube = automated code reviewer. Fails the pipeline if quality drops below threshold. Catches issues humans miss.

---

### Q33: What is a pipeline stage vs job vs step?

**Answer:**
- **Stage** — a logical group (build, test, deploy). Stages run sequentially.
- **Job** — a specific task within a stage. Jobs in the same stage run in parallel.
- **Step** — an individual command within a job (e.g., `npm install`, `npm test`).

**💡 Tip:** Stage > Job > Step. Stage = "Test", Job = "unit-tests", Step = "run npm test". Stages are sequential, jobs are parallel.

---

### Q34: What is environment parity?

**Answer:** Making development, staging, and production environments as similar as possible. Reduces "works on my machine" issues. Docker and IaC help achieve parity by ensuring consistent environments.

**💡 Tip:** Parity = dev ≈ staging ≈ production. Docker containers guarantee parity. The more different environments are, the more bugs escape to production.

---

### Q35: What is a webhook in CI/CD?

**Answer:** An HTTP callback triggered by an event (git push, PR merge). The CI/CD platform registers webhooks with the Git hosting service to start pipelines automatically when events occur.

**💡 Tip:** Webhook = "call me when something happens." GitHub sends a POST to Jenkins/GitHub Actions when code is pushed. No polling needed.

---

### Q36: What is the difference between declarative and scripted CI/CD pipelines?

**Answer:**
- **Declarative** — defines WHAT to do using structured YAML syntax. Simpler, more readable, limited flexibility. (GitHub Actions, GitLab CI)
- **Scripted** — defines HOW to do it using a programming language (Groovy in Jenkins). More flexible, harder to read.

**💡 Tip:** Declarative = fill in a form. Scripted = write a program. Prefer declarative — it's what modern CI/CD platforms use.

---

### Q37: What is artifact versioning?

**Answer:** Assigning unique version numbers to build artifacts (Docker images, packages, binaries). Common strategies: semantic versioning (1.2.3), git hash, build number. Essential for traceability and rollback.

**💡 Tip:** Always tag Docker images with git SHA + version number. `myapp:1.2.3-abc1234`. Never deploy `latest` in production — it's not reproducible.

---

### Q38: What is database migration in CI/CD?

**Answer:** Automated schema changes applied during deployment. Migration scripts are version-controlled and executed as part of the pipeline. Forward-only migrations (additive) are preferred; destructive changes require careful planning.

**💡 Tip:** Migrations = database schema in code. Run before deploying new code (backward-compatible migration first, then deploy). Tools: Flyway, Liquibase, Prisma Migrate, Drizzle Kit.

---

### Q39: What is container orchestration in CI/CD?

**Answer:** Automating the deployment, scaling, and management of containerized applications. Kubernetes is the standard orchestrator. CI/CD pipelines build images → push to registry → update Kubernetes deployment.

**💡 Tip:** Orchestration = conducting the container orchestra. Kubernetes handles: deployment, scaling, self-healing, service discovery, load balancing.

---

### Q40: What is an SLA, SLO, and SLI?

**Answer:**
- **SLA (Service Level Agreement)** — contractual commitment to reliability (e.g., "99.9% uptime or refund")
- **SLO (Service Level Objective)** — internal reliability target (e.g., "99.9% uptime goal")
- **SLI (Service Level Indicator)** — the metric measuring reliability (e.g., "actual uptime measured: 99.95%")

**💡 Tip:** SLI = the measurement. SLO = the goal. SLA = the contract. SLI feeds SLO which feeds SLA. "Error budget" = 100% - SLO.

---

## Advanced (Q41–Q50)

### Q41: What is zero-downtime deployment?

**Answer:** Deploying new versions without any service interruption. Techniques: blue-green (switch traffic), rolling (gradual replacement), canary (incremental rollout). Requires health checks and automatic rollback.

**💡 Tip:** Zero-downtime = users never notice a deployment. Key: keep old version running while new version starts up, then switch traffic.

---

### Q42: What is observability in DevOps?

**Answer:** The ability to understand the internal state of a system from its external outputs. Three pillars:
- **Logs** — discrete event records (structured JSON logs)
- **Metrics** — numeric measurements over time (CPU, latency, error rate)
- **Traces** — request flow across services (distributed tracing)

**💡 Tip:** Logs = "what happened?" Metrics = "how much?" Traces = "where did it go?" Together = full picture. Tools: Prometheus, Grafana, Jaeger, ELK.

---

### Q43: What is a deployment pipeline with approval gates?

**Answer:** A pipeline that requires manual approval at specific stages before proceeding (e.g., approve before deploying to production). Provides human oversight for critical environments while automating everything else.

**💡 Tip:** Approval gates = "eyes on this before it goes live." Auto-deploy to staging, manual approve for production. Balance automation speed with human judgment.

---

### Q44: What is immutable infrastructure?

**Answer:** Servers/containers are never modified after deployment. Instead of updating in place, you build a new version and replace the old one entirely. If something breaks, deploy a fresh instance rather than fixing the broken one.

**💡 Tip:** Immutable = "build new, don't patch old." Docker containers are immutable by design. Terraform replaces resources instead of modifying them.

---

### Q45: What is chaos engineering in DevOps?

**Answer:** Deliberately introducing failures into production-like environments to build confidence in system resilience. Practice: define steady state → hypothesize resilience → inject failure → observe → improve.

**💡 Tip:** Chaos engineering = "break things on purpose to build confidence." Start small in staging. Tools: Chaos Monkey, Gremlin, Litmus. Goal: find weaknesses before users do.

---

### Q46: What is a canary analysis (automated canary)?

**Answer:** Automated comparison of metrics between the canary (new version) and baseline (current version). If the canary's error rate, latency, or other metrics are significantly worse, the rollout is automatically stopped and rolled back.

**💡 Tip:** Automated canary = "let the robots decide." Compare canary vs baseline metrics for 5-15 minutes. Statistical significance > gut feeling. Tools: Flagger, Argo Rollouts.

---

### Q47: What is the difference between Terraform and Ansible?

**Answer:**
- **Terraform** — provisioning tool. Creates infrastructure (VMs, networks, databases). Declarative. Cloud-level.
- **Ansible** — configuration tool. Configures already-provisioned machines (install software, manage files). Imperative/procedural. OS-level.

**💡 Tip:** Terraform = builds the house. Ansible = furnishes the house. Often used together: Terraform provisions, Ansible configures.

---

### Q48: What is a supply chain attack in CI/CD?

**Answer:** An attack that targets the build pipeline or dependencies rather than the application code. Examples: compromising a dependency package, injecting malicious code into the build process, stealing pipeline secrets.

**💡 Tip:** Defense: pin dependency versions, verify checksums, scan for vulnerabilities (Snyk, Dependabot), use private registries, lock down pipeline permissions.

---

### Q49: What is platform engineering?

**Answer:** Building internal developer platforms (IDPs) that provide self-service tools, templates, and guardrails for developers. It abstracts infrastructure complexity so developers can deploy without knowing Kubernetes, Terraform, or cloud specifics.

**💡 Tip:** Platform engineering = "make DevOps invisible to developers." Developer: `deploy my app` → platform handles everything. Backstage is a popular IDP framework.

---

### Q50: How do you design a production-ready CI/CD pipeline?

**Answer:**
1. **Build stage** — compile, install deps, build artifact (Docker image)
2. **Test stage** — unit tests, integration tests, linting, type checking
3. **Security stage** — dependency scanning (Snyk), SAST (SonarQube), container scanning (Trivy)
4. **Artifact stage** — push tagged image to registry
5. **Deploy to staging** — automated, run smoke/E2E tests
6. **Approval gate** — manual or automated (canary analysis)
7. **Deploy to production** — blue-green/canary/rolling
8. **Post-deploy** — smoke tests, monitoring, alerts
9. **Rollback plan** — automatic on failure

**💡 Tip:** Production-ready pipeline = build → test → secure → stage → verify → approve → deploy → verify → monitor. Every stage is a quality gate. Fail fast, fail early.
