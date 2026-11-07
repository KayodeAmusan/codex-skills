# Codex Skills

**395 skills for building software, researching ideas, running a business, and working with data.**

Pick a skill, give Codex a task, and it follows a workflow written for that job. That might mean reviewing a pull request, planning a product launch, checking a team's injury news, or turning a pile of notes into a useful research brief.

Each folder includes a `SKILL.md` with instructions and setup notes. Some also include scripts, templates, and reference material. You can install one skill or the whole collection.

[Get started](#get-started) · [Installation](#installation) · [Examples](#try-it) · [All 395 skills](#skill-catalog) · [Updates](#update-or-remove-skills) · [Troubleshooting](#troubleshooting)

## Get started

Start with a task you already have. You don't need to install all 395 skills.

| If you want to… | Start here |
| --- | --- |
| Review a pull request | [`pr-review-expert`](skills/pr-review-expert/SKILL.md) |
| Fix a broken feature | [`focused-fix`](skills/focused-fix/SKILL.md) |
| Test a web application | [`pw`](skills/pw/SKILL.md), invoked as `playwright-pro` |
| Improve a website's design | [`design-review`](skills/design-review/SKILL.md) or [`redesign`](skills/redesign/SKILL.md) |
| Research a question with sources | [`research`](skills/research/SKILL.md) or [`deep-research`](skills/deep-research/SKILL.md) |
| Write clearer copy | [`copy-editing`](skills/copy-editing/SKILL.md) or [`content-humanizer`](skills/content-humanizer/SKILL.md) |
| Plan a product | [`product-manager-toolkit`](skills/product-manager-toolkit/SKILL.md) |
| Check sports news and injuries | [`sports-news`](skills/sports-news/SKILL.md) plus the relevant sports data skill |

## Before you install

You need Codex installed and signed in. For the manual instructions below, you also need Git. [OpenAI's skill guide](https://developers.openai.com/codex/skills/) explains how skill discovery and invocation work.

**A skill supplies instructions. Its tools are a separate setup step.** A writing workflow may need nothing else; a browser, cloud, or broker workflow may need a package, account, connector, or API key. Read the selected skill's setup section before using it.

Some skills were written for Claude Code and still refer to its slash commands, hooks, memory files, or Workflow tool. Those features need a compatible host or adaptation for Codex. Copying a folder does not install those integrations.

## Installation

### Option 1: Ask Codex to install a skill

Paste this into Codex:

```text
Use $skill-installer to install this skill:
https://github.com/KayodeAmusan/codex-skills/tree/main/skills/code-reviewer
```

Replace `code-reviewer` with any folder name from the catalog. Use the whole folder, including its references and scripts.

### Option 2: Install selected skills manually

These commands use `~/.agents/skills`, the user-level location in OpenAI's current guide. Older setups may use `~/.codex/skills` or a custom Codex skills directory. If you already have a working location, use that instead. Keep one installed copy of a skill to avoid duplicate entries.

**macOS or Linux**

Run these commands from a folder where you keep downloaded repositories:

```bash
git clone https://github.com/KayodeAmusan/codex-skills.git
mkdir -p "$HOME/.agents/skills"
cp -R -i codex-skills/skills/code-reviewer "$HOME/.agents/skills/"
cp -R -i codex-skills/skills/sports-news "$HOME/.agents/skills/"
```

The `-i` option asks before replacing existing files. Change the skill folder names to the ones you want.

**Windows PowerShell**

```powershell
git clone https://github.com/KayodeAmusan/codex-skills.git
$skillRoot = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
Copy-Item -Path '.\codex-skills\skills\code-reviewer' -Destination $skillRoot -Recurse -Confirm
Copy-Item -Path '.\codex-skills\skills\sports-news' -Destination $skillRoot -Recurse -Confirm
```

`-Confirm` lets you review the copy operation. Back up an installed folder first if you have edited its files.

**Just for one project**

Use `.agents/skills/` inside your project instead of your home directory. For example, from the project's root on macOS or Linux:

```bash
mkdir -p .agents/skills
cp -R -i /path/to/codex-skills/skills/code-reviewer .agents/skills/
```

Replace `/path/to/codex-skills` with your clone's actual location. Open that project in Codex to use the skill there.

### Option 3: Install the full collection

The commands below add missing skill folders and skip folders that already exist. They leave local edits alone. See [Updates](#update-or-remove-skills) when you want to replace an installed version.

**macOS or Linux**

If you haven't cloned the repository yet, run the `git clone` command above first. From the directory containing `codex-skills`:

```bash
mkdir -p "$HOME/.agents/skills"
for skill_dir in codex-skills/skills/*; do
  [ -d "$skill_dir" ] || continue
  skill_name=$(basename "$skill_dir")
  if [ -e "$HOME/.agents/skills/$skill_name" ]; then
    printf 'Already installed: %s\n' "$skill_name"
  else
    cp -R "$skill_dir" "$HOME/.agents/skills/"
  fi
done
```

**Windows PowerShell**

From the directory containing `codex-skills`:

```powershell
$skillRoot = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
Get-ChildItem -Path '.\codex-skills\skills' -Directory | ForEach-Object {
    $destination = Join-Path $skillRoot $_.Name
    if (Test-Path -LiteralPath $destination) {
        Write-Host "Already installed: $($_.Name)"
    } else {
        Copy-Item -LiteralPath $_.FullName -Destination $destination -Recurse
    }
}
```

### Finish setup

Codex should discover the new skills automatically. If one doesn't appear, start a new chat or restart Codex. In the CLI or IDE extension, use `/skills` or type `$` to find an installed skill.

Your installed folder should look like this:

```text
.agents/skills/code-reviewer/
└── SKILL.md
```

Keep any `scripts/`, `references/`, `assets/`, and `agents/` folders beside `SKILL.md`. A folder containing another nested copy of the skill won't match this layout.

## Try it

After installing the relevant skill, give it a concrete task:

```text
Use $pr-review-expert to review this branch against main.
Focus on bugs and regressions, and point to the affected lines.
```

```text
Use $content-humanizer to rewrite this draft for a developer audience.
Keep the facts and make the wording direct.
```

```text
Use $football-data and $sports-news to prepare an Arsenal match preview.
Check recent form, missing players, and dated team news.
Separate confirmed information from reports.
```

```text
Use $playwright-pro to add tests for checkout, including a failed payment.
```

Most folder names match the skill's invocation name. The exception here is [`pw`](skills/pw/SKILL.md), whose `name` field is `playwright-pro`.

## Extra tools and accounts

| Skill family | What to check |
| --- | --- |
| Sports data and news | Install the shared `sports-skills` Python CLI. The public data commands used by these skills need no API key. Coverage varies by league and source. |
| Browser automation and testing | Check browser, Playwright, and any named MCP requirements in the skill. BrowserStack and TestRail need their own service setup. |
| Cloud and workplace administration | Configure the relevant CLI, account, permissions, and connector for AWS, Azure, GCP, Google Workspace, Microsoft 365, or Atlassian. |
| Claude Managed Agents | The launch and scheduling workflows require Anthropic service access and the owner's credentials. |
| Markets and trading | Data-only tasks and broker execution have different requirements. Broker workflows need account setup and explicit order approval; the broker skill starts with paper trading. |
| Design and media | Figma, video, screenshots, and narration workflows may call external tools listed in the skill. |

For the sports CLI, `pipx` provides an isolated installation:

```bash
pipx install sports-skills
sports-skills --help
```

Install [pipx](https://pipx.pypa.io/stable/installation/) first if that command isn't available. Advanced nflverse analysis may need the package's `nfl` extra; check the NFL skill before requesting those datasets. The [`Machina project`](https://github.com/machina-sports/sports-skills) documents its data sources and package options.

No accounts, credentials, installed packages, or paid subscriptions are included in this repository.

## Skill catalog

All **395 skills** are listed below. Each link opens the skill's instructions. The descriptions explain the job; the linked file covers its inputs, tools, and limits. Some entries are routers that help you choose a specialist, and a few preserve older workflow names.

| Browse by topic | Skills |
| --- | ---: |
| [Engineering and code review](#engineering-and-code-review) | 38 |
| [Testing and quality assurance](#testing-and-quality-assurance) | 12 |
| [Cloud, infrastructure, and reliability](#cloud-infrastructure-and-reliability) | 15 |
| [Security and privacy](#security-and-privacy) | 13 |
| [Design, interfaces, and accessibility](#design-interfaces-and-accessibility) | 28 |
| [AI, data, and machine learning](#ai-data-and-machine-learning) | 17 |
| [Agents, skills, and automation](#agents-skills-and-automation) | 50 |
| [Research and evidence](#research-and-evidence) | 16 |
| [Marketing, content, and growth](#marketing-content-and-growth) | 52 |
| [Sales, commercial strategy, and finance](#sales-commercial-strategy-and-finance) | 20 |
| [Leadership, company building, and operations](#leadership-company-building-and-operations) | 46 |
| [Product and project management](#product-and-project-management) | 17 |
| [Compliance, quality, and medical devices](#compliance-quality-and-medical-devices) | 25 |
| [Writing, communication, and personal workflows](#writing-communication-and-personal-workflows) | 17 |
| [Sports data and news](#sports-data-and-news) | 16 |
| [Markets and trading](#markets-and-trading) | 13 |

### Engineering and code review

<details>
<summary>Browse 38 skills</summary>

| Skill | What it does |
| --- | --- |
| [`adversarial-reviewer`](skills/adversarial-reviewer/SKILL.md) | Challenge a code change from deliberately critical reviewer perspectives. |
| [`api-design-reviewer`](skills/api-design-reviewer/SKILL.md) | Review REST APIs for inconsistent conventions, breaking changes, and missing versioning. |
| [`api-test-suite-builder`](skills/api-test-suite-builder/SKILL.md) | Build integration and contract tests for REST endpoints. |
| [`boost-asio-pro`](skills/boost-asio-pro/SKILL.md) | Write and review asynchronous C++ networking code with Boost.Asio or standalone Asio. |
| [`changelog-generator`](skills/changelog-generator/SKILL.md) | Turn Conventional Commits into release notes and version changes. |
| [`code-reviewer`](skills/code-reviewer/SKILL.md) | Review code quality, maintainability, and implementation risks. |
| [`code-to-prd`](skills/code-to-prd/SKILL.md) | Work backwards from an existing codebase to a product requirements document. |
| [`code-tour`](skills/code-tour/SKILL.md) | Create guided CodeTour walkthroughs tied to actual files and lines. |
| [`codebase-onboarding`](skills/codebase-onboarding/SKILL.md) | Explain a codebase's structure and prepare onboarding notes for new developers. |
| [`database-designer`](skills/database-designer/SKILL.md) | Model data, choose a database, plan migrations, and improve queries. |
| [`database-schema-designer`](skills/database-schema-designer/SKILL.md) | Design table relationships, normalize schemas, and draw entity relationship diagrams. |
| [`email-template-builder`](skills/email-template-builder/SKILL.md) | Build transactional email templates, previews, and delivery-provider integrations. |
| [`embedded-iot-mentor`](skills/embedded-iot-mentor/SKILL.md) | Plan embedded hardware projects, from choosing a board to a production PCB. |
| [`engineering-advanced-skills`](skills/engineering-advanced-skills/SKILL.md) | Find the right specialist for advanced engineering work. |
| [`engineering-skills`](skills/engineering-skills/SKILL.md) | Browse the engineering collection and choose a skill for your stack or task. |
| [`feature-flags-architect`](skills/feature-flags-architect/SKILL.md) | Plan feature flags, staged rollouts, kill switches, and flag retirement. |
| [`focused-fix`](skills/focused-fix/SKILL.md) | Repair a broken feature or module across its full workflow. |
| [`git-worktree-manager`](skills/git-worktree-manager/SKILL.md) | Set up isolated Git worktrees for parallel development and clean them up afterwards. |
| [`karpathy-coder`](skills/karpathy-coder/SKILL.md) | Keep coding changes small, state assumptions, and define checks before implementation. |
| [`migration-architect`](skills/migration-architect/SKILL.md) | Plan system and database migrations with compatibility checks and rollback steps. |
| [`minimalist`](skills/minimalist/SKILL.md) | Reduce unnecessary code, dependencies, and abstractions. |
| [`monorepo-navigator`](skills/monorepo-navigator/SKILL.md) | Trace package dependencies and plan selective builds and tests in a monorepo. |
| [`named-persona-adversarial-review`](skills/named-persona-adversarial-review/SKILL.md) | Review code using the documented engineering principles of named practitioners. |
| [`performance-profiler`](skills/performance-profiler/SKILL.md) | Measure CPU, memory, database, and I/O bottlenecks before tuning an application. |
| [`pr-review-expert`](skills/pr-review-expert/SKILL.md) | Review pull request diffs for bugs, security issues, and code quality. |
| [`senior-architect`](skills/senior-architect/SKILL.md) | Compare architecture options and document system boundaries, dependencies, and scaling decisions. |
| [`senior-backend`](skills/senior-backend/SKILL.md) | Design backend APIs, authentication, services, and database access. |
| [`senior-frontend`](skills/senior-frontend/SKILL.md) | Build React and Next.js interfaces with TypeScript and Tailwind CSS. |
| [`senior-fullstack`](skills/senior-fullstack/SKILL.md) | Scaffold full-stack projects and assess their architecture and code quality. |
| [`spec-driven-workflow`](skills/spec-driven-workflow/SKILL.md) | Write specifications and acceptance criteria before implementation. |
| [`spec-to-repo`](skills/spec-to-repo/SKILL.md) | Turn a project brief into a starter codebase and implementation plan. |
| [`sql-database-assistant`](skills/sql-database-assistant/SKILL.md) | Write SQL, inspect schemas, improve queries, and work with common ORMs. |
| [`strict-api`](skills/strict-api/SKILL.md) | Check that functions, imports, and APIs exist in the installed version before using them. |
| [`stripe-integration-expert`](skills/stripe-integration-expert/SKILL.md) | Implement Stripe checkout, subscriptions, invoices, and webhook handling. |
| [`tc-tracker`](skills/tc-tracker/SKILL.md) | Record technical changes and preserve enough context to resume them in another session. |
| [`tech-debt-tracker`](skills/tech-debt-tracker/SKILL.md) | Find technical debt and rank cleanup work by severity and impact. |
| [`tech-stack-evaluator`](skills/tech-stack-evaluator/SKILL.md) | Compare technology choices by cost, security, ecosystem, and migration effort. |
| [`zero-hallucination-coder`](skills/zero-hallucination-coder/SKILL.md) | Ground implementation in verified files and APIs, then check the finished work. |

</details>

### Testing and quality assurance

<details>
<summary>Browse 12 skills</summary>

| Skill | What it does |
| --- | --- |
| [`browserstack`](skills/browserstack/SKILL.md) | Run browser compatibility tests through BrowserStack. |
| [`coverage`](skills/coverage/SKILL.md) | Find gaps in test coverage and identify untested behavior. |
| [`fix`](skills/fix/SKILL.md) | Diagnose and repair failing or flaky Playwright tests. |
| [`generate`](skills/generate/SKILL.md) | Generate Playwright tests for pages, components, and user journeys. |
| [`migrate`](skills/migrate/SKILL.md) | Convert Cypress or Selenium tests to Playwright. |
| [`pw`](skills/pw/SKILL.md) | Coordinate Playwright setup, test generation, debugging, and review; its invocation name is `playwright-pro`. |
| [`pw-init`](skills/pw-init/SKILL.md) | Add Playwright and its initial test configuration to a project. |
| [`pw-review`](skills/pw-review/SKILL.md) | Review Playwright tests for reliability and maintainability. |
| [`report`](skills/report/SKILL.md) | Summarize test results in a readable report. |
| [`senior-qa`](skills/senior-qa/SKILL.md) | Generate unit, integration, and end-to-end tests for React and Next.js applications. |
| [`tdd-guide`](skills/tdd-guide/SKILL.md) | Work through red, green, and refactor with tests, fixtures, and mocks. |
| [`testrail`](skills/testrail/SKILL.md) | Exchange test cases and results with TestRail. |

</details>

### Cloud, infrastructure, and reliability

<details>
<summary>Browse 15 skills</summary>

| Skill | What it does |
| --- | --- |
| [`aws-solution-architect`](skills/aws-solution-architect/SKILL.md) | Plan AWS services, serverless systems, infrastructure templates, and cloud costs. |
| [`azure-cloud-architect`](skills/azure-cloud-architect/SKILL.md) | Design Azure infrastructure, Bicep or ARM templates, and deployment plans. |
| [`chaos-engineering`](skills/chaos-engineering/SKILL.md) | Design fault-injection experiments with clear limits, stop conditions, and recovery checks. |
| [`ci-cd-pipeline-builder`](skills/ci-cd-pipeline-builder/SKILL.md) | Create CI/CD pipelines suited to a project's stack and deployment environments. |
| [`docker-development`](skills/docker-development/SKILL.md) | Improve Dockerfiles, Compose setups, build stages, and container security. |
| [`gcp-cloud-architect`](skills/gcp-cloud-architect/SKILL.md) | Plan Google Cloud infrastructure, data services, deployments, and cost reductions. |
| [`helm-chart-builder`](skills/helm-chart-builder/SKILL.md) | Build, configure, and test Helm charts for Kubernetes applications. |
| [`incident-commander`](skills/incident-commander/SKILL.md) | Coordinate a service incident, maintain its timeline, and guide the follow-up review. |
| [`kubernetes-operator`](skills/kubernetes-operator/SKILL.md) | Design custom Kubernetes controllers, CRDs, and reconciliation loops. |
| [`observability-designer`](skills/observability-designer/SKILL.md) | Plan application metrics, logs, traces, dashboards, and useful alerts. |
| [`runbook-generator`](skills/runbook-generator/SKILL.md) | Draft deployment, maintenance, incident, and rollback runbooks. |
| [`senior-devops`](skills/senior-devops/SKILL.md) | Build deployment automation, infrastructure workflows, and cloud monitoring. |
| [`ship-gate`](skills/ship-gate/SKILL.md) | Check release readiness across security, deployment, data, dependencies, and observability. |
| [`slo-architect`](skills/slo-architect/SKILL.md) | Define service indicators, reliability targets, error budgets, and burn-rate alerts. |
| [`terraform-patterns`](skills/terraform-patterns/SKILL.md) | Design Terraform modules, state management, provider configuration, and deployment checks. |

</details>

### Security and privacy

<details>
<summary>Browse 13 skills</summary>

| Skill | What it does |
| --- | --- |
| [`ai-security`](skills/ai-security/SKILL.md) | Assess AI systems for prompt injection, data poisoning, jailbreaks, and tool abuse. |
| [`cloud-security`](skills/cloud-security/SKILL.md) | Assess cloud infrastructure configuration and security posture. |
| [`dependency-auditor`](skills/dependency-auditor/SKILL.md) | Check third-party packages for vulnerabilities, licensing issues, and upgrade risks. |
| [`env-secrets-manager`](skills/env-secrets-manager/SKILL.md) | Audit environment-variable handling, configuration drift, and credential rotation readiness. |
| [`incident-response`](skills/incident-response/SKILL.md) | Triage security incidents, classify severity, and organize evidence collection. |
| [`red-team`](skills/red-team/SKILL.md) | Plan authorized attack simulations and examine likely attack paths. |
| [`secrets-vault-manager`](skills/secrets-vault-manager/SKILL.md) | Set up secret stores, rotation workflows, and access controls. |
| [`security-guidance`](skills/security-guidance/SKILL.md) | Document Claude Code security hooks that flag risky command and code patterns. |
| [`security-pen-testing`](skills/security-pen-testing/SKILL.md) | Plan and perform authorized vulnerability assessments and penetration testing. |
| [`senior-secops`](skills/senior-secops/SKILL.md) | Review application security, scan vulnerabilities, and plan remediation. |
| [`senior-security`](skills/senior-security/SKILL.md) | Model threats with STRIDE and DREAD, or route security work to a specialist. |
| [`skill-security-auditor`](skills/skill-security-auditor/SKILL.md) | Inspect third-party skills for suspicious instructions, code, and data access. |
| [`threat-detection`](skills/threat-detection/SKILL.md) | Hunt for threats using indicators, telemetry anomalies, and ATT&CK techniques. |

</details>

### Design, interfaces, and accessibility

<details>
<summary>Browse 28 skills</summary>

| Skill | What it does |
| --- | --- |
| [`a11y-audit`](skills/a11y-audit/SKILL.md) | Check interfaces against WCAG and ARIA guidance, with specific fixes for each finding. |
| [`apple-hig-expert`](skills/apple-hig-expert/SKILL.md) | Review Apple-platform interfaces against the Human Interface Guidelines. |
| [`apply-aesthetic`](skills/apply-aesthetic/SKILL.md) | Translate a requested visual style into a consistent set of design tokens. |
| [`brand-guidelines`](skills/brand-guidelines/SKILL.md) | Document brand colors, typography, imagery, logo rules, and tone of voice. |
| [`brandkit`](skills/brandkit/SKILL.md) | Build a brand token system, light and dark themes, and a shared CSS theme. |
| [`design-code`](skills/design-code/SKILL.md) | Turn component specifications and tokens into accessible UI code. |
| [`design-component`](skills/design-component/SKILL.md) | Specify a component's structure, variants, states, tokens, and accessibility behavior. |
| [`design-qa`](skills/design-qa/SKILL.md) | Check tokens, contrast, accessibility, and visual changes across interface states. |
| [`design-review`](skills/design-review/SKILL.md) | Critique a screen or product using usability heuristics and prioritized findings. |
| [`design-system`](skills/design-system/SKILL.md) | Capture brand preferences and configure a reusable visual foundation for generated work. |
| [`design-tokens`](skills/design-tokens/SKILL.md) | Create and audit primitive, semantic, and component design tokens. |
| [`emil-design-eng`](skills/emil-design-eng/SKILL.md) | Apply Emil Kowalski's approach to UI details, animation, and component polish. |
| [`epic-design`](skills/epic-design/SKILL.md) | Build websites with scroll storytelling, parallax, and layered visual effects. |
| [`figma-integration`](skills/figma-integration/SKILL.md) | Map design tokens to Figma Variables and check consistency between designs and code. |
| [`google-workspace-cli`](skills/google-workspace-cli/SKILL.md) | Set up and use the Google Workspace CLI for mail, files, calendars, and administration. |
| [`governance`](skills/governance/SKILL.md) | Define how design tokens and components are versioned, contributed, and retired. |
| [`image-to-code`](skills/image-to-code/SKILL.md) | Rebuild a screenshot or mockup as accessible, token-based interface code. |
| [`landing`](skills/landing/SKILL.md) | Create a single-page HTML landing page with animation and a defined visual direction. |
| [`landing-page-generator`](skills/landing-page-generator/SKILL.md) | Generate a React or Next.js landing page with Tailwind sections and conversion copy. |
| [`migrate-design-system`](skills/migrate-design-system/SKILL.md) | Map tokens and components between design systems or UI libraries. |
| [`ms365-tenant-manager`](skills/ms365-tenant-manager/SKILL.md) | Plan Microsoft 365 tenant administration and generate scripts for common admin tasks. |
| [`performance`](skills/performance/SKILL.md) | Improve Core Web Vitals, loading behavior, layout stability, and animation smoothness. |
| [`prototype`](skills/prototype/SKILL.md) | Move an idea from user journeys and wireframes to tested interface prototypes. |
| [`redesign`](skills/redesign/SKILL.md) | Improve an existing interface's visual design while preserving its working behavior. |
| [`token-build`](skills/token-build/SKILL.md) | Convert design tokens into CSS, Tailwind, JavaScript, iOS, and Android artifacts. |
| [`ui-design-system`](skills/ui-design-system/SKILL.md) | Create UI tokens, component documentation, responsive rules, and developer handoff materials. |
| [`ux-researcher-designer`](skills/ux-researcher-designer/SKILL.md) | Plan user research, develop personas, map journeys, and synthesize usability findings. |
| [`ux-writing`](skills/ux-writing/SKILL.md) | Write clear buttons, errors, labels, empty states, and other interface copy. |

</details>

### AI, data, and machine learning

<details>
<summary>Browse 17 skills</summary>

| Skill | What it does |
| --- | --- |
| [`data-quality-auditor`](skills/data-quality-auditor/SKILL.md) | Profile datasets and identify missing values, inconsistencies, anomalies, and cleanup priorities. |
| [`deep-learning-book`](skills/deep-learning-book/SKILL.md) | Study the Goodfellow, Bengio, and Courville textbook with chapter guides and later developments. |
| [`llm-cost-optimizer`](skills/llm-cost-optimizer/SKILL.md) | Compare models and reduce LLM spending through caching, routing, and token budgeting. |
| [`llm-wiki`](skills/llm-wiki/SKILL.md) | Build an Obsidian knowledge base with linked source notes, concepts, and maintained summaries. |
| [`mcp-server-builder`](skills/mcp-server-builder/SKILL.md) | Build Python or TypeScript MCP servers from API contracts. |
| [`memory-engineering`](skills/memory-engineering/SKILL.md) | Choose and evaluate memory approaches for agents, including retrieval and knowledge graphs. |
| [`prompt-engineer-toolkit`](skills/prompt-engineer-toolkit/SKILL.md) | Test and version marketing prompts against structured examples. |
| [`prompt-governance`](skills/prompt-governance/SKILL.md) | Manage production prompts, evaluation pipelines, registries, and regression checks. |
| [`rag-architect`](skills/rag-architect/SKILL.md) | Design retrieval pipelines and evaluate chunking, embeddings, and search quality. |
| [`senior-computer-vision`](skills/senior-computer-vision/SKILL.md) | Plan object detection, segmentation, and other computer vision systems. |
| [`senior-data-engineer`](skills/senior-data-engineer/SKILL.md) | Build data pipelines, orchestration, modeling, and ETL or ELT workflows. |
| [`senior-data-scientist`](skills/senior-data-scientist/SKILL.md) | Apply statistical modeling, causal inference, experiments, and predictive analysis. |
| [`senior-ml-engineer`](skills/senior-ml-engineer/SKILL.md) | Plan model deployment, MLOps, feature stores, and drift monitoring. |
| [`senior-prompt-engineer`](skills/senior-prompt-engineer/SKILL.md) | Design prompts, output contracts, and evaluation sets for LLM applications. |
| [`snowflake-development`](skills/snowflake-development/SKILL.md) | Work with Snowflake SQL, pipelines, Snowpark, dbt, and Cortex features. |
| [`spinning-up-deep-rl`](skills/spinning-up-deep-rl/SKILL.md) | Apply OpenAI's Spinning Up material to reinforcement learning concepts and algorithms. |
| [`statistical-analyst`](skills/statistical-analyst/SKILL.md) | Choose hypothesis tests, calculate sample sizes, and interpret effects and uncertainty. |

</details>

### Agents, skills, and automation

<details>
<summary>Browse 50 skills</summary>

| Skill | What it does |
| --- | --- |
| [`agent-decision-receipts`](skills/agent-decision-receipts/SKILL.md) | Create and verify signed records of consequential agent actions. |
| [`agent-designer`](skills/agent-designer/SKILL.md) | Design agent architectures, tool schemas, and orchestration patterns. |
| [`agent-harness`](skills/agent-harness/SKILL.md) | Run bounded task loops with checks, retry limits, and escalation rules. |
| [`agent-launcher-orchestrator`](skills/agent-launcher-orchestrator/SKILL.md) | Coordinate the scoping, launch, evaluation, and scheduling of Claude Managed Agents. |
| [`agent-memory`](skills/agent-memory/SKILL.md) | Turn repeated session lessons into durable project memory with reviewed promotion steps. |
| [`agent-protocol`](skills/agent-protocol/SKILL.md) | Define how executive-role agents communicate and hand work to one another. |
| [`agent-workflow-designer`](skills/agent-workflow-designer/SKILL.md) | Plan agent workflows with explicit handoffs, failure handling, and context budgets. |
| [`agenthub`](skills/agenthub/SKILL.md) | Compare competing agent approaches in isolated Git worktrees. |
| [`ar-resume`](skills/ar-resume/SKILL.md) | Resume a paused autoresearch experiment from its branch and saved results. |
| [`ar-status`](skills/ar-status/SKILL.md) | Show the progress, results, and active loops of an autoresearch experiment. |
| [`autoresearch-agent`](skills/autoresearch-agent/SKILL.md) | Repeatedly edit a target, run a fixed evaluation, and keep measured improvements. |
| [`board`](skills/board/SKILL.md) | Read and write the coordination board for an AgentHub session. |
| [`book-to-skill`](skills/book-to-skill/SKILL.md) | Turn books or document collections into skills with reference chapters and working guides. |
| [`browser-automation`](skills/browser-automation/SKILL.md) | Automate browser tasks, forms, screenshots, and web data extraction. |
| [`claude-coach`](skills/claude-coach/SKILL.md) | Teach Claude features and practical workflows through guided coaching. |
| [`collab-proof`](skills/collab-proof/SKILL.md) | Review a session to distinguish the user's decisions from the agent's contributions. |
| [`eval`](skills/eval/SKILL.md) | Score and rank competing AgentHub results using metrics or a judging rubric. |
| [`extract`](skills/extract/SKILL.md) | Package a proven solution or recurring pattern as a standalone skill. |
| [`fable-goal`](skills/fable-goal/SKILL.md) | Turn a loose description of an outcome into a clear autonomous task prompt. |
| [`full-page-screenshot`](skills/full-page-screenshot/SKILL.md) | Capture long web pages, including lazy-loaded content and nested scroll containers. |
| [`grade-iterate`](skills/grade-iterate/SKILL.md) | Evaluate a Claude Managed Agent against a rubric and iterate within a fixed budget. |
| [`handoff`](skills/handoff/SKILL.md) | Write a compact handoff that links to the files, decisions, and work already completed. |
| [`hivemind`](skills/hivemind/SKILL.md) | Delegate work to opencode workers with isolated worktrees and evaluation steps. |
| [`hub-init`](skills/hub-init/SKILL.md) | Define an AgentHub competition's task, agent count, and judging criteria. |
| [`hub-status`](skills/hub-status/SKILL.md) | Report AgentHub progress, task dependencies, and branch status. |
| [`human-gate`](skills/human-gate/SKILL.md) | Collect human review through an HTML review page and record the result. |
| [`interview`](skills/interview/SKILL.md) | Scope a Claude Managed Agent's job and produce its build sheet and evaluation plan. |
| [`loop`](skills/loop/SKILL.md) | Schedule recurring autoresearch experiments where the host supports the required scheduler. |
| [`memory-review`](skills/memory-review/SKILL.md) | Review stored lessons for stale entries, duplication, and promotion candidates. |
| [`memory-status`](skills/memory-status/SKILL.md) | Report memory size, topic coverage, stale entries, and cleanup suggestions. |
| [`merge`](skills/merge/SKILL.md) | Merge the winning AgentHub branch and clean up the other worktrees. |
| [`notebooklm`](skills/notebooklm/SKILL.md) | Operate NotebookLM through browser automation, including sources and generated overviews. |
| [`promote`](skills/promote/SKILL.md) | Move a proven memory pattern into durable project instructions. |
| [`reflect`](skills/reflect/SKILL.md) | Pause a task to reconsider its direction, assumptions, and possible bias. |
| [`remember`](skills/remember/SKILL.md) | Save an important lesson to memory with its context and timestamp. |
| [`run`](skills/run/SKILL.md) | Run an AgentHub competition from initialization through evaluation and merge. |
| [`run-without-you`](skills/run-without-you/SKILL.md) | Set up scheduled, event-driven, or on-demand execution for a Claude Managed Agent. |
| [`self-eval`](skills/self-eval/SKILL.md) | Review the quality of completed agent work and challenge inflated self-assessments. |
| [`self-improving-agent`](skills/self-improving-agent/SKILL.md) | Review session memory and turn recurring lessons into rules or reusable skills. |
| [`setup`](skills/setup/SKILL.md) | Define an autoresearch experiment's target, evaluation command, metric, and direction. |
| [`skill-doctor`](skills/skill-doctor/SKILL.md) | Use real conversation history to assess which skills work and where instructions need revision. |
| [`skill-tester`](skills/skill-tester/SKILL.md) | Validate skill structure, bundled scripts, and overall instruction quality. |
| [`skillopt-sleep`](skills/skillopt-sleep/SKILL.md) | Review past sessions during an offline improvement cycle for memory and skills. |
| [`spawn`](skills/spawn/SKILL.md) | Launch competing AgentHub agents in separate Git worktrees. |
| [`stage-launch`](skills/stage-launch/SKILL.md) | Prepare and launch a Claude Managed Agent using the owner's Anthropic account. |
| [`universal-scraping-architect`](skills/universal-scraping-architect/SKILL.md) | Plan web scraping and document extraction pipelines with validation checks. |
| [`workflow-builder`](skills/workflow-builder/SKILL.md) | Write deterministic multi-agent workflow scripts for Claude Code's Workflow tool. |
| [`wrap-up`](skills/wrap-up/SKILL.md) | Document a launched Claude Managed Agent and its remaining setup or improvement options. |
| [`write-a-skill`](skills/write-a-skill/SKILL.md) | Create a skill with focused instructions, metadata, references, and optional scripts. |
| [`youtube-full`](skills/youtube-full/SKILL.md) | Find YouTube videos, retrieve transcripts, inspect playlists, and monitor channels. |

</details>

### Research and evidence

<details>
<summary>Browse 16 skills</summary>

| Skill | What it does |
| --- | --- |
| [`clinical-research`](skills/clinical-research/SKILL.md) | Plan clinical study endpoints, sample sizes, power calculations, and study design. |
| [`competitive-teardown`](skills/competitive-teardown/SKILL.md) | Compare competitors across product, pricing, customer feedback, and market signals. |
| [`deep-research`](skills/deep-research/SKILL.md) | Investigate a question using saved sources, explicit hypotheses, triangulation, and critical review. |
| [`deepread`](skills/deepread/SKILL.md) | Read books and documents closely, map their claims, and test your understanding. |
| [`dossier`](skills/dossier/SKILL.md) | Investigate a company, person, or organization against a stated hypothesis. |
| [`grants`](skills/grants/SKILL.md) | Research NIH funding opportunities and build a grant search strategy. |
| [`litreview`](skills/litreview/SKILL.md) | Plan an academic literature search and find papers through PubMed and OpenAlex. |
| [`market-research`](skills/market-research/SKILL.md) | Size markets, plan surveys, and assess market evidence and competitive structure. |
| [`patent`](skills/patent/SKILL.md) | Research prior art, patent ownership, competitive activity, and freedom-to-operate questions. |
| [`product-research`](skills/product-research/SKILL.md) | Choose user research methods, estimate sample needs, and organize findings. |
| [`pulse`](skills/pulse/SKILL.md) | Research recent discussion, trends, and sentiment across web and community sources. |
| [`research`](skills/research/SKILL.md) | Route a research question to the appropriate research workflow. |
| [`research-finance`](skills/research-finance/SKILL.md) | Budget an R&D program and track burn, runway, and funding milestones. |
| [`research-ops-skills`](skills/research-ops-skills/SKILL.md) | Find a workflow for clinical research, R&D finance, market research, or user research. |
| [`research-summarizer`](skills/research-summarizer/SKILL.md) | Extract findings and compare academic papers, articles, reports, and documentation. |
| [`syllabus`](skills/syllabus/SKILL.md) | Build a supplementary academic reading list from a course syllabus. |

</details>

### Marketing, content, and growth

<details>
<summary>Browse 52 skills</summary>

| Skill | What it does |
| --- | --- |
| [`ab-test-setup`](skills/ab-test-setup/SKILL.md) | Plan marketing experiments, hypotheses, variants, and measurement. |
| [`ad-creative`](skills/ad-creative/SKILL.md) | Write and iterate ad headlines, copy, and creative variations. |
| [`aeo`](skills/aeo/SKILL.md) | Structure content so answer engines can understand and cite it. |
| [`analytics-tracking`](skills/analytics-tracking/SKILL.md) | Set up and debug GA4, Google Tag Manager, events, and conversion tracking. |
| [`app-store-optimization`](skills/app-store-optimization/SKILL.md) | Research app keywords and improve App Store or Google Play listings. |
| [`campaign-analytics`](skills/campaign-analytics/SKILL.md) | Assess campaign attribution, funnel performance, and return on spend. |
| [`churn-prevention`](skills/churn-prevention/SKILL.md) | Improve cancellation flows, save offers, payment recovery, and exit surveys. |
| [`cmo-advisor`](skills/cmo-advisor/SKILL.md) | Plan marketing strategy, positioning, brand, demand generation, and marketing investment. |
| [`cold-email`](skills/cold-email/SKILL.md) | Write B2B prospecting emails and follow-up sequences. |
| [`competitor-alternatives`](skills/competitor-alternatives/SKILL.md) | Write comparison and alternative pages for search visitors and sales conversations. |
| [`content-creator`](skills/content-creator/SKILL.md) | Redirect older content-creation requests to the appropriate specialist; kept for compatibility. |
| [`content-humanizer`](skills/content-humanizer/SKILL.md) | Rewrite stiff or generic copy with clearer language and a more natural voice. |
| [`content-production`](skills/content-production/SKILL.md) | Research, draft, edit, and prepare articles or guides for publication. |
| [`content-strategy`](skills/content-strategy/SKILL.md) | Choose content topics, editorial priorities, and a publishing plan. |
| [`copy-editing`](skills/copy-editing/SKILL.md) | Review existing marketing copy for clarity, consistency, and persuasive structure. |
| [`copywriting`](skills/copywriting/SKILL.md) | Write website, landing page, pricing page, and product marketing copy. |
| [`email-sequence`](skills/email-sequence/SKILL.md) | Plan and write welcome, nurture, onboarding, and reactivation email sequences. |
| [`form-cro`](skills/form-cro/SKILL.md) | Improve lead, contact, demo, survey, and checkout form completion. |
| [`free-tool-strategy`](skills/free-tool-strategy/SKILL.md) | Plan useful free tools that attract an audience or generate leads. |
| [`launch-strategy`](skills/launch-strategy/SKILL.md) | Plan product launches, announcements, beta access, and launch channels. |
| [`linkedin-analytics`](skills/linkedin-analytics/SKILL.md) | Analyze LinkedIn results and test explanations for changes in reach or engagement. |
| [`linkedin-content`](skills/linkedin-content/SKILL.md) | Write and edit LinkedIn posts, carousel scripts, and repurposed content. |
| [`linkedin-engagement`](skills/linkedin-engagement/SKILL.md) | Plan LinkedIn comments, replies, networking, and outreach. |
| [`linkedin-profile`](skills/linkedin-profile/SKILL.md) | Review and rewrite LinkedIn headlines, About sections, and experience entries. |
| [`linkedin-skills`](skills/linkedin-skills/SKILL.md) | Choose the right LinkedIn workflow for strategy, profiles, posts, or networking. |
| [`linkedin-strategy`](skills/linkedin-strategy/SKILL.md) | Define LinkedIn positioning, content pillars, cadence, and newsletter plans. |
| [`local-seo-manager`](skills/local-seo-manager/SKILL.md) | Improve local search presence, business listings, and service-area pages. |
| [`marketing-context`](skills/marketing-context/SKILL.md) | Document the audience, positioning, brand voice, and context other marketing skills need. |
| [`marketing-demand-acquisition`](skills/marketing-demand-acquisition/SKILL.md) | Plan demand generation across paid ads, search, and partnerships. |
| [`marketing-ideas`](skills/marketing-ideas/SKILL.md) | Generate marketing approaches for a software product or SaaS business. |
| [`marketing-ops`](skills/marketing-ops/SKILL.md) | Coordinate a campaign across content, SEO, conversion, channels, and analytics skills. |
| [`marketing-psychology`](skills/marketing-psychology/SKILL.md) | Apply behavioral science and decision-making principles to marketing. |
| [`marketing-skills`](skills/marketing-skills/SKILL.md) | Browse the marketing collection and find an appropriate specialist. |
| [`marketing-strategy-pmm`](skills/marketing-strategy-pmm/SKILL.md) | Develop product positioning, audience definitions, launch plans, and go-to-market strategy. |
| [`onboarding-cro`](skills/onboarding-cro/SKILL.md) | Improve first-run experiences, activation, and time to value. |
| [`page-cro`](skills/page-cro/SKILL.md) | Find ways to improve conversions on marketing and pricing pages. |
| [`paid-ads`](skills/paid-ads/SKILL.md) | Plan and improve paid campaigns across search and social platforms. |
| [`paywall-upgrade-cro`](skills/paywall-upgrade-cro/SKILL.md) | Improve upgrade screens, in-app paywalls, and free-to-paid conversion. |
| [`popup-cro`](skills/popup-cro/SKILL.md) | Improve banners, popups, slide-ins, and lead-capture overlays. |
| [`pricing-strategy`](skills/pricing-strategy/SKILL.md) | Design SaaS tiers, value metrics, pricing pages, and price-change messaging. |
| [`programmatic-seo`](skills/programmatic-seo/SKILL.md) | Plan search-focused pages built at scale from templates and structured data. |
| [`referral-program`](skills/referral-program/SKILL.md) | Design referral or affiliate incentives, workflows, and measurement. |
| [`schema-markup`](skills/schema-markup/SKILL.md) | Add and validate Schema.org structured data and JSON-LD. |
| [`seo-audit`](skills/seo-audit/SKILL.md) | Diagnose technical and on-page search problems and prioritize fixes. |
| [`signup-flow-cro`](skills/signup-flow-cro/SKILL.md) | Reduce friction in registration, account creation, and trial signup. |
| [`site-architecture`](skills/site-architecture/SKILL.md) | Plan website navigation, URL hierarchy, and internal linking. |
| [`social-content`](skills/social-content/SKILL.md) | Create and adapt social posts, threads, and publishing calendars. |
| [`social-media-analyzer`](skills/social-media-analyzer/SKILL.md) | Compare engagement, campaign returns, and performance across social platforms. |
| [`social-media-manager`](skills/social-media-manager/SKILL.md) | Plan social strategy, content calendars, and community engagement. |
| [`video-content-strategist`](skills/video-content-strategist/SKILL.md) | Plan video channels, scripts, short-form pipelines, and content repurposing. |
| [`webinar-marketing`](skills/webinar-marketing/SKILL.md) | Plan, promote, and follow up on webinars and virtual events. |
| [`x-twitter-growth`](skills/x-twitter-growth/SKILL.md) | Plan X/Twitter content, audience growth, and engagement analysis. |

</details>

### Sales, commercial strategy, and finance

<details>
<summary>Browse 20 skills</summary>

| Skill | What it does |
| --- | --- |
| [`business-growth-skills`](skills/business-growth-skills/SKILL.md) | Route customer success, pre-sales, revenue operations, and proposal requests. |
| [`business-investment-advisor`](skills/business-investment-advisor/SKILL.md) | Compare capital investments using cash flows, payback, NPV, IRR, and alternatives. |
| [`cfo-advisor`](skills/cfo-advisor/SKILL.md) | Plan company finances, cash management, unit economics, and fundraising. |
| [`channel-economics`](skills/channel-economics/SKILL.md) | Compare direct and partner channel costs, returns, and resource allocation. |
| [`commercial-forecaster`](skills/commercial-forecaster/SKILL.md) | Build bookings, pipeline, ARR, and retention forecasts with explicit assumptions. |
| [`commercial-policy`](skills/commercial-policy/SKILL.md) | Define discount rules, approval limits, exceptions, and deal guardrails. |
| [`commercial-skills`](skills/commercial-skills/SKILL.md) | Find help for pricing, deal review, partnerships, proposals, and forecasting. |
| [`contract-and-proposal-writer`](skills/contract-and-proposal-writer/SKILL.md) | Draft proposals, statements of work, NDAs, and other business contracts. |
| [`cro-advisor`](skills/cro-advisor/SKILL.md) | Plan revenue models, quotas, pricing, sales capacity, and retention. |
| [`customer-success-manager`](skills/customer-success-manager/SKILL.md) | Score account health, identify churn risks, and assess expansion opportunities. |
| [`deal-desk`](skills/deal-desk/SKILL.md) | Review unusual discounts, contract changes, deal margins, and approval requirements. |
| [`finance-skills`](skills/finance-skills/SKILL.md) | Choose between financial analysis and SaaS metrics workflows. |
| [`financial-analyst`](skills/financial-analyst/SKILL.md) | Analyze statements, ratios, valuations, budgets, and rolling forecasts. |
| [`ma-playbook`](skills/ma-playbook/SKILL.md) | Plan acquisition strategy, due diligence, deal structure, and integration. |
| [`partnerships-architect`](skills/partnerships-architect/SKILL.md) | Assess potential partners, partner tiers, revenue sharing, and joint go-to-market commitments. |
| [`pricing-strategist`](skills/pricing-strategist/SKILL.md) | Choose pricing models and packages, including willingness-to-pay analysis. |
| [`revenue-operations`](skills/revenue-operations/SKILL.md) | Assess sales pipeline health, forecasting accuracy, and go-to-market efficiency. |
| [`rfp-responder`](skills/rfp-responder/SKILL.md) | Organize and draft responses to RFPs, RFIs, RFQs, and buyer questionnaires. |
| [`saas-metrics-coach`](skills/saas-metrics-coach/SKILL.md) | Interpret recurring revenue, churn, retention, CAC, LTV, and SaaS financial health. |
| [`sales-engineer`](skills/sales-engineer/SKILL.md) | Review RFP coverage, compare product capabilities, and plan proof-of-concept engagements. |

</details>

### Leadership, company building, and operations

<details>
<summary>Browse 46 skills</summary>

| Skill | What it does |
| --- | --- |
| [`andreessen`](skills/andreessen/SKILL.md) | Pressure-test business ideas using market-first frameworks associated with Marc Andreessen. |
| [`arquiteto-de-empresa`](skills/arquiteto-de-empresa/SKILL.md) | Build a company's operating documentation as a linked collection of Markdown files. |
| [`board-deck-builder`](skills/board-deck-builder/SKILL.md) | Prepare board and investor update decks with financial and operational perspectives. |
| [`board-meeting`](skills/board-meeting/SKILL.md) | Structure a strategic decision through independent executive-role reviews and synthesis. |
| [`board-prep`](skills/board-prep/SKILL.md) | Prepare numbers, narratives, and answers for difficult board or investor questions. |
| [`business-name-fit`](skills/business-name-fit/SKILL.md) | Assess business names across cultural origins, languages, and target markets. |
| [`business-operations-skills`](skills/business-operations-skills/SKILL.md) | Find a workflow for processes, vendors, capacity, procurement, or internal operations. |
| [`c-level-skills`](skills/c-level-skills/SKILL.md) | Browse executive advisory roles and choose the right perspective for a company decision. |
| [`capacity-planner`](skills/capacity-planner/SKILL.md) | Model staffing, workload, utilization, and operational capacity. |
| [`ceo-advisor`](skills/ceo-advisor/SKILL.md) | Work through company strategy, leadership, organization, and stakeholder decisions. |
| [`challenge`](skills/challenge/SKILL.md) | Run a pre-mortem to expose assumptions and failure points before committing resources. |
| [`change-management`](skills/change-management/SKILL.md) | Plan organizational changes, communication, adoption, and resistance management. |
| [`chief-ai-officer-advisor`](skills/chief-ai-officer-advisor/SKILL.md) | Assess AI strategy, build-versus-buy choices, costs, risks, and team structure. |
| [`chief-customer-officer-advisor`](skills/chief-customer-officer-advisor/SKILL.md) | Plan customer segmentation, retention strategy, and customer success coverage. |
| [`chief-data-officer-advisor`](skills/chief-data-officer-advisor/SKILL.md) | Assess data strategy, training-data rights, data products, and team structure. |
| [`chief-of-staff`](skills/chief-of-staff/SKILL.md) | Route strategic questions to executive advisors and bring their recommendations together. |
| [`chro-advisor`](skills/chro-advisor/SKILL.md) | Plan hiring, compensation, organizational structure, culture, and retention. |
| [`ciso-advisor`](skills/ciso-advisor/SKILL.md) | Plan security investment, compliance roadmaps, governance, and board reporting. |
| [`company-os`](skills/company-os/SKILL.md) | Set up company scorecards, accountability, meeting rhythms, and operating priorities. |
| [`competitive-intel`](skills/competitive-intel/SKILL.md) | Track competitors and translate changes into positioning, sales, and roadmap decisions. |
| [`context-engine`](skills/context-engine/SKILL.md) | Keep shared company context current for the executive advisory workflows. |
| [`coo-advisor`](skills/coo-advisor/SKILL.md) | Design operating processes, execution rhythms, OKRs, and scaling plans. |
| [`cs-onboard`](skills/cs-onboard/SKILL.md) | Interview a founder and record the company context used by executive advisory skills. |
| [`cto-advisor`](skills/cto-advisor/SKILL.md) | Assess technology strategy, engineering organization, architecture, and technical debt. |
| [`culture-architect`](skills/culture-architect/SKILL.md) | Turn company values into behaviors, rituals, and ways to measure culture. |
| [`decision-logger`](skills/decision-logger/SKILL.md) | Record board discussions, approved decisions, owners, and follow-up actions. |
| [`executive-mentor`](skills/executive-mentor/SKILL.md) | Challenge executive plans, difficult decisions, and leadership assumptions. |
| [`founder-coach`](skills/founder-coach/SKILL.md) | Work on delegation, leadership style, energy, and the demands of being a founder. |
| [`general-counsel-advisor`](skills/general-counsel-advisor/SKILL.md) | Review contract issues, intellectual property strategy, term sheets, and regulatory questions. |
| [`grill-me`](skills/grill-me/SKILL.md) | Question a plan or design until assumptions and decisions are clear. |
| [`grill-with-docs`](skills/grill-with-docs/SKILL.md) | Challenge a plan against existing project context and recorded architecture decisions. |
| [`hard-call`](skills/hard-call/SKILL.md) | Work through difficult decisions using time horizons and regret-based comparisons. |
| [`internal-narrative`](skills/internal-narrative/SKILL.md) | Keep the company's story consistent across employees, investors, customers, and candidates. |
| [`interview-system-designer`](skills/interview-system-designer/SKILL.md) | Build hiring stages, competency rubrics, interview questions, and scoring practices. |
| [`intl-expansion`](skills/intl-expansion/SKILL.md) | Compare new markets and plan localization, entry strategy, and regional operations. |
| [`knowledge-ops`](skills/knowledge-ops/SKILL.md) | Write and maintain company procedures, ownership details, and internal runbooks. |
| [`org-health-diagnostic`](skills/org-health-diagnostic/SKILL.md) | Assess organizational health across functions and identify areas needing attention. |
| [`postmortem`](skills/postmortem/SKILL.md) | Examine a failed launch, missed target, or bad decision and record concrete changes. |
| [`process-mapper`](skills/process-mapper/SKILL.md) | Map business workflows, measure cycle times, and identify handoff bottlenecks. |
| [`procurement-optimizer`](skills/procurement-optimizer/SKILL.md) | Review spending, suppliers, purchasing workflows, and consolidation opportunities. |
| [`roast`](skills/roast/SKILL.md) | Challenge a business idea from several perspectives before investing in it. |
| [`scenario-war-room`](skills/scenario-war-room/SKILL.md) | Model compound business risks and their effects across company functions. |
| [`strategic-alignment`](skills/strategic-alignment/SKILL.md) | Connect company goals to team work and find conflicting or orphaned priorities. |
| [`stress-test`](skills/stress-test/SKILL.md) | Examine the business assumptions that a plan depends on. |
| [`vendor-management`](skills/vendor-management/SKILL.md) | Score vendors, review SLAs, assess third-party risks, and prepare renewals. |
| [`vpe-advisor`](skills/vpe-advisor/SKILL.md) | Assess engineering delivery, hiring, team structure, and management practices. |

</details>

### Product and project management

<details>
<summary>Browse 17 skills</summary>

| Skill | What it does |
| --- | --- |
| [`agile-product-owner`](skills/agile-product-owner/SKILL.md) | Write user stories, define acceptance criteria, and organize sprint backlogs. |
| [`atlassian-admin`](skills/atlassian-admin/SKILL.md) | Manage Atlassian users, permissions, integrations, and organization-wide settings. |
| [`atlassian-templates`](skills/atlassian-templates/SKILL.md) | Create reusable Jira and Confluence templates, layouts, and blueprints. |
| [`confluence-expert`](skills/confluence-expert/SKILL.md) | Organize Confluence spaces, pages, permissions, templates, and knowledge bases. |
| [`cpo-advisor`](skills/cpo-advisor/SKILL.md) | Plan product vision, portfolios, product-market fit, and product organization. |
| [`experiment-designer`](skills/experiment-designer/SKILL.md) | Write testable product hypotheses, size experiments, and interpret results. |
| [`jira-expert`](skills/jira-expert/SKILL.md) | Configure Jira projects, workflows, JQL searches, automation, and reporting. |
| [`pm-skills`](skills/pm-skills/SKILL.md) | Coordinate project delivery across planning, reporting, Jira, Confluence, and team communication. |
| [`product-analytics`](skills/product-analytics/SKILL.md) | Define product metrics and analyze cohorts, retention, and feature adoption. |
| [`product-discovery`](skills/product-discovery/SKILL.md) | Test product opportunities and problem-solution fit before committing to delivery. |
| [`product-manager-toolkit`](skills/product-manager-toolkit/SKILL.md) | Prioritize features, analyze interviews, and write product requirements. |
| [`product-skills`](skills/product-skills/SKILL.md) | Choose a product workflow for discovery, planning, analytics, design, or delivery. |
| [`product-strategist`](skills/product-strategist/SKILL.md) | Plan product goals, quarterly OKRs, positioning, and team growth. |
| [`roadmap-communicator`](skills/roadmap-communicator/SKILL.md) | Explain roadmaps, releases, and product changes to different audiences. |
| [`saas-scaffolder`](skills/saas-scaffolder/SKILL.md) | Generate a SaaS starter with authentication, data models, billing, and dashboard structure. |
| [`scrum-master`](skills/scrum-master/SKILL.md) | Analyze sprint health and improve planning, retrospectives, and backlog practices. |
| [`senior-pm`](skills/senior-pm/SKILL.md) | Plan enterprise projects, portfolio risks, resource allocation, and stakeholder reporting. |

</details>

### Compliance, quality, and medical devices

<details>
<summary>Browse 25 skills</summary>

| Skill | What it does |
| --- | --- |
| [`ai-act-readiness`](skills/ai-act-readiness/SKILL.md) | Assess an AI system's readiness for EU AI Act obligations. |
| [`aims-audit`](skills/aims-audit/SKILL.md) | Prepare internal audits of an ISO/IEC 42001 AI management system. |
| [`capa-officer`](skills/capa-officer/SKILL.md) | Investigate quality problems and manage corrective and preventive actions. |
| [`compliance-os`](skills/compliance-os/SKILL.md) | Map overlapping controls and organize evidence across compliance frameworks. |
| [`compliance-readiness`](skills/compliance-readiness/SKILL.md) | Review a compliance program's audit readiness and remaining gaps. |
| [`eu-ai-act-specialist`](skills/eu-ai-act-specialist/SKILL.md) | Classify AI system risks and map applicable EU AI Act obligations. |
| [`fda-consultant-specialist`](skills/fda-consultant-specialist/SKILL.md) | Plan medical device regulatory pathways, FDA submissions, and quality requirements. |
| [`fda-qsr-audit-prep`](skills/fda-qsr-audit-prep/SKILL.md) | Prepare for FDA quality-system audits, inspections, and findings responses. |
| [`gdpr-audit-prep`](skills/gdpr-audit-prep/SKILL.md) | Prepare for GDPR reviews, investigations, or privacy due diligence. |
| [`gdpr-dsgvo-expert`](skills/gdpr-dsgvo-expert/SKILL.md) | Assess privacy risks, document DPIAs, and track data subject rights requests. |
| [`information-security-manager-iso27001`](skills/information-security-manager-iso27001/SKILL.md) | Design and maintain an ISO 27001 information security management system. |
| [`isms-audit-expert`](skills/isms-audit-expert/SKILL.md) | Assess ISO 27001 controls, audit evidence, and certification gaps. |
| [`iso13485-audit-prep`](skills/iso13485-audit-prep/SKILL.md) | Review medical device quality-system audit readiness, including design controls and CAPA. |
| [`iso27001-audit-prep`](skills/iso27001-audit-prep/SKILL.md) | Prepare for ISO 27001 internal, surveillance, or certification audits. |
| [`iso42001-specialist`](skills/iso42001-specialist/SKILL.md) | Assess AI management system gaps, risks, and controls against ISO/IEC 42001. |
| [`mdr-745-specialist`](skills/mdr-745-specialist/SKILL.md) | Plan EU medical device classification, technical documentation, and post-market work. |
| [`qms-audit-expert`](skills/qms-audit-expert/SKILL.md) | Plan and conduct ISO 13485 audits and verify corrective actions. |
| [`quality-documentation-manager`](skills/quality-documentation-manager/SKILL.md) | Organize controlled documents, versioning, change records, and approval workflows. |
| [`quality-manager-qmr`](skills/quality-manager-qmr/SKILL.md) | Support quality-system governance, management reviews, and quality performance monitoring. |
| [`quality-manager-qms-iso13485`](skills/quality-manager-qms-iso13485/SKILL.md) | Design and maintain an ISO 13485 medical device quality management system. |
| [`ra-qm-skills`](skills/ra-qm-skills/SKILL.md) | Find a regulatory or quality-management workflow for medical device and health technology work. |
| [`regulatory-affairs-head`](skills/regulatory-affairs-head/SKILL.md) | Plan medical device submissions, regulatory responses, and market-access work. |
| [`risk-management-specialist`](skills/risk-management-specialist/SKILL.md) | Apply ISO 14971 risk analysis, controls, and post-production review to medical devices. |
| [`soc2-audit-prep`](skills/soc2-audit-prep/SKILL.md) | Assess readiness before and during a SOC 2 Type II observation period. |
| [`soc2-compliance`](skills/soc2-compliance/SKILL.md) | Map Trust Services Criteria, collect evidence, and prepare SOC 2 control documentation. |

</details>

### Writing, communication, and personal workflows

<details>
<summary>Browse 17 skills</summary>

| Skill | What it does |
| --- | --- |
| [`behuman`](skills/behuman/SKILL.md) | Adjust conversational responses toward a more natural, considerate tone. |
| [`capture`](skills/capture/SKILL.md) | Turn a brain dump into an organized set of ideas, tasks, and commitments. |
| [`caveman`](skills/caveman/SKILL.md) | Shorten responses aggressively while preserving their technical meaning. |
| [`deep-work`](skills/deep-work/SKILL.md) | Plan focus blocks, reduce shallow work, and close the day with a shutdown routine. |
| [`demo-video`](skills/demo-video/SKILL.md) | Assemble product walkthrough videos from screenshots, scenes, narration, and editing tools. |
| [`inbox-setup`](skills/inbox-setup/SKILL.md) | Record email priorities, response preferences, and context for later inbox triage. |
| [`inbox-triage`](skills/inbox-triage/SKILL.md) | Sort and prioritize email using the preferences saved during inbox setup. |
| [`internal-comms`](skills/internal-comms/SKILL.md) | Plan internal announcements for policy changes, reorganizations, and other company transitions. |
| [`markdown-html-orchestrator`](skills/markdown-html-orchestrator/SKILL.md) | Choose an HTML format for a Markdown document, review, or slide deck. |
| [`md-document`](skills/md-document/SKILL.md) | Turn long Markdown documents into HTML with navigation, search, and code-copy controls. |
| [`md-review`](skills/md-review/SKILL.md) | Turn a Markdown code review into an HTML view with diffs and annotations. |
| [`md-slides`](skills/md-slides/SKILL.md) | Turn a Markdown slide deck into an HTML presentation. |
| [`meeting-analyzer`](skills/meeting-analyzer/SKILL.md) | Review meeting transcripts for communication patterns and actionable coaching feedback. |
| [`meetings`](skills/meetings/SKILL.md) | Decide whether to meet, plan an agenda, and turn notes into assigned actions. |
| [`swedish-mentor`](skills/swedish-mentor/SKILL.md) | Build a Swedish learning path using listening and reading material matched to your level. |
| [`team-communications`](skills/team-communications/SKILL.md) | Draft progress updates, internal newsletters, FAQs, and incident communications. |
| [`weekly-review`](skills/weekly-review/SKILL.md) | Review commitments, close open loops, and bring a neglected task system up to date. |

</details>

### Sports data and news

<details>
<summary>Browse 16 skills</summary>

| Skill | What it does |
| --- | --- |
| [`cbb-data`](skills/cbb-data/SKILL.md) | Fetch college basketball scores, rankings, rosters, statistics, news, and NCAA tournament data. |
| [`cfb-data`](skills/cfb-data/SKILL.md) | Fetch college football scores, schedules, injuries, rankings, statistics, and news. |
| [`cricket-data`](skills/cricket-data/SKILL.md) | Fetch cricket scores and news, plus historical Cricsheet ball-by-ball data. |
| [`esports`](skills/esports/SKILL.md) | Fetch Dota 2 pro match data and League of Legends competitive information. |
| [`football-data`](skills/football-data/SKILL.md) | Fetch football fixtures, lineups, form, xG, Elo, forecasts, and Premier League missing-player data. |
| [`gleague-data`](skills/gleague-data/SKILL.md) | Fetch NBA G League scores, schedules, rosters, box scores, and advanced statistics. |
| [`golf-data`](skills/golf-data/SKILL.md) | Fetch PGA, LPGA, and DP World Tour schedules, leaderboards, and player information. |
| [`mlb-data`](skills/mlb-data/SKILL.md) | Fetch MLB scores, injuries, rosters, statistics, and official pitch-level data. |
| [`nba-data`](skills/nba-data/SKILL.md) | Fetch NBA injuries, news, schedules, live game data, and advanced statistics. |
| [`nfl-data`](skills/nfl-data/SKILL.md) | Fetch NFL injuries, news, scores, rosters, play-by-play, and nflverse data. |
| [`nhl-data`](skills/nhl-data/SKILL.md) | Fetch NHL injuries, scores, rosters, statistics, and historical play-by-play data. |
| [`sports-news`](skills/sports-news/SKILL.md) | Search sports headlines and fetch dated articles through Google News and RSS feeds. |
| [`tennis-data`](skills/tennis-data/SKILL.md) | Fetch ATP and WTA scores, rankings, calendars, player profiles, and news. |
| [`volleyball-data`](skills/volleyball-data/SKILL.md) | Fetch Dutch volleyball standings, fixtures, results, clubs, and news through Nevobo. |
| [`wnba-data`](skills/wnba-data/SKILL.md) | Fetch WNBA scores, injuries, schedules, rosters, statistics, and news. |
| [`xctf-data`](skills/xctf-data/SKILL.md) | Fetch NCAA running profiles, personal records, results history, and cross-country or track news. |

</details>

### Markets and trading

<details>
<summary>Browse 13 skills</summary>

| Skill | What it does |
| --- | --- |
| [`pine-script`](skills/pine-script/SKILL.md) | Turn a trading strategy idea into TradingView Pine Script and an on-chart preview. |
| [`stock-analysis`](skills/stock-analysis/SKILL.md) | Analyze a listed company's fundamentals, valuation, sector position, and business quality. |
| [`trader-alerts`](skills/trader-alerts/SKILL.md) | Create price, stop, target, market-open, and planned-exit alerts. |
| [`trader-analyst`](skills/trader-analyst/SKILL.md) | Read SEC filings and XBRL facts to explain changes in company earnings and margins. |
| [`trader-broker`](skills/trader-broker/SKILL.md) | Prepare order tickets and submit approved broker orders, using paper trading by default. |
| [`trader-budget`](skills/trader-budget/SKILL.md) | Size a position against cash, order limits, position caps, and maximum planned loss. |
| [`trader-entry`](skills/trader-entry/SKILL.md) | Watch intraday bars and identify entry signals from a defined setup. |
| [`trader-exit`](skills/trader-exit/SKILL.md) | Set a trade's stop, target, and planned exit conditions before entry. |
| [`trader-news`](skills/trader-news/SKILL.md) | Monitor ticker headlines, SEC 8-K filings, and price changes. |
| [`trader-pelosi`](skills/trader-pelosi/SKILL.md) | Track publicly disclosed House stock transactions and extract rows from filing PDFs. |
| [`trader-risk`](skills/trader-risk/SKILL.md) | Check a proposed trade against risk limits before it reaches a broker. |
| [`trader-scanner`](skills/trader-scanner/SKILL.md) | Scan US-listed stocks for chart setups and intraday opening-range breakouts. |
| [`trader-timemachine`](skills/trader-timemachine/SKILL.md) | Compare a planned trade with historical setups and simulated price paths. |

</details>

## Update or remove skills

Pull the latest collection into your clone:

```bash
git -C codex-skills pull --ff-only
```

This updates the clone, not the copies in your skills directory. To update an installed skill, back up its current folder, then copy the latest folder into place. Replacing the whole folder also removes files retired upstream; merging files can leave old scripts behind. Keep backups outside any skill discovery directory so Codex doesn't load both versions.

For a skill installed through `$skill-installer`, tell Codex which skill to update and whether you want to keep local changes. The full-collection commands above intentionally skip existing folders.

To uninstall, remove that skill's folder from the location where you installed it. This doesn't uninstall its Python packages, browser tools, or connectors. Restart Codex if the old entry remains visible.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| A skill doesn't appear | Confirm that its folder contains `SKILL.md` directly, that it's in a discovery location, and that it isn't disabled. Then restart Codex. |
| The same skill appears twice | Look for copies in both user and project locations, or in the current and older skills directories. Keep the version you intend to use. |
| A command is missing | Open that skill's setup section. Copying instructions doesn't install the CLI or MCP server they call. |
| A Claude slash command or hook doesn't work | It may be specific to Claude Code. Ask Codex to follow the skill's workflow using available tools, or use the required host. |
| A sports query returns no data | Check league coverage, dates, and the provider's response. An empty result isn't proof that nobody is injured or that no match exists. |
| Installation finds an existing folder | Keep it if you have local edits, or back it up outside the skills directory before replacing it. |
| Git says the clone already exists | Update that clone with `git pull --ff-only` rather than cloning over it. |

## How the collection is organized

```text
codex-skills/
├── README.md
└── skills/
    └── skill-name/
        ├── SKILL.md
        ├── scripts/       # Where provided
        ├── references/    # Where provided
        └── assets/        # Where provided
```

This collection brings together user-level skills from Windows and Linux setups. Codex's built-in `.system` skills, plugin caches, and local runtimes are not included.

## Contributing

For a new skill, add a folder under `skills/` with a `SKILL.md` that includes `name` and `description` in its YAML frontmatter. Explain the task it handles, the information it needs, its setup steps, and the checks it runs. Keep supporting files inside the folder.

Add the skill to the catalog and update the total. For changes to an existing skill, describe what behavior changes and how you checked it. Open an issue if a workflow no longer matches the tools or host it was written for.

Keep credentials, `.env` files, private keys, personal records, caches, and virtual environments out of contributions.

## Credits and reuse

This is a collection of workflows from different sources, including [Machina Sports](https://github.com/machina-sports/sports-skills). Original author details, attribution, and license declarations are retained in the skill files where supplied.

There is no single repository-wide license file covering this collection. Check the individual skill and its source before redistributing or adapting it; don't assume one skill's license applies to every folder.

Read a skill before enabling it. Some bundled scripts can change files, call services, or run administrative commands. The catalog describes their intended use, not a guarantee that every workflow has been tested on every system.
