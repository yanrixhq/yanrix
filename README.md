# Yanrix

**Continuous STRIDE threat modeling on every pull request.**

Yanrix is an AI-powered GitHub Action that performs structured STRIDE analysis on your pull request diffs and posts a threat model directly in the PR conversation. It brings threat modeling out of the design meeting and into the continuous development workflow, producing a living threat model that accumulates across your codebase over time.

Yanrix operates at the architectural reasoning layer. It applies design-phase thinking continuously throughout development, identifying how changes affect trust boundaries, data flows, and attack surfaces. Where traditional security tools scan for known patterns in code, Yanrix reasons about how your system could be attacked given its architecture.

## What Yanrix does

Every time a pull request is opened, Yanrix analyzes the diff and produces:

- **STRIDE-classified threats** with confidence scores, severity ratings, and CWE mappings
- **Affected component identification** linking each threat to specific files and architectural boundaries
- **Risk assessment** with before/after ratings reflecting the change introduced by the PR
- **A living threat model manifest** stored as a GitHub Artifact, versioned across PRs, so your threat model grows with your codebase

Findings are posted as a structured PR comment. The full threat model manifest and an optional PDF report are available as downloadable workflow artifacts with 400-day retention. When a prior manifest exists from a previous run, Yanrix compares the new analysis against it, identifying new threats, resolved threats, and changed risk ratings. The manifest versions accumulate over time, giving your team a continuous record of how the threat landscape evolves with each change.

## Current capabilities

- **Analysis:** Full STRIDE classification, confidence scoring, CWE mapping, MITRE ATT&CK references, OWASP categorization, and deterministic security checks on any codebase
- **Output:** PR comment with severity-sorted findings, manifest artifact link, and optional PDF report
- **Model:** Bring your own key (BYOK). Yanrix runs on your LLM API key with your preferred provider.
- **Providers:** Anthropic Claude, OpenAI, and Google Gemini via LiteLLM
- **Privacy:** Your source code, repository files, and PR diffs are never transmitted to Yanrix servers. Code is sent from your GitHub runner directly to the LLM provider you configure, using your API key.
- **Cost:** Free and unlimited for public open source repositories. A limited demo is available for private repositories so you can evaluate the product on your own codebase.

## Quick start

### 1. Install the Yanrix GitHub App

Install the [Yanrix App](https://github.com/apps/yanrix) from the GitHub Marketplace. Select the repositories you want to analyze during installation.

The App grants Yanrix the permissions it needs to read repository contents, read PR diffs, and post PR comments. **You need both the App and the Action.** The App provides the permission grant and bot identity (`@yanrix[bot]`). The Action is the analysis engine that runs in your CI workflow.

### 2. Add the Action to your workflow

Create `.github/workflows/yanrix.yml` in your repository:

```yaml
name: Yanrix Threat Model

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write
  actions: read

jobs:
  threat-model:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      ## Comment out before using Gemini model
      - uses: yanrixhq/yanrix@v1
        with:
          model: "anthropic/claude-sonnet-4"
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
    
     # Uncomment to use Gemini instead
     # - uses: yanrixhq/yanrix@v1
     #   with:
     #     model: "gemini/gemini-2.5-pro"
     #     # target_folder: "docs"
     #   env:
     #     GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
    
```

This default configuration uses Anthropic Claude and scans your entire repository.

### 3. Configure your LLM API key

Add your API key as a repository secret. In your repository, go to **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Provider | Secret name | Key format |
|----------|-------------|------------|
| Anthropic | `ANTHROPIC_API_KEY` | Starts with `sk-ant-api03-` |
| OpenAI | `OPENAI_API_KEY` | Starts with `sk-` |
| Google Gemini | `GEMINI_API_KEY` | Alphanumeric string |

Only configure the secret for the provider you are using.

### 4. Open a pull request

Open or update a pull request. Yanrix will run automatically and post a threat model comment within a few minutes.

Yanrix is an AI-powered GitHub Action that performs structured STRIDE analysis on your pull request diffs and posts a threat model directly in the PR conversation. It brings threat modeling out of the design meeting and into the continuous development workflow, producing a living threat model that accumulates across your codebase over time.

Yanrix operates at the architectural reasoning layer. It applies design-phase thinking continuously throughout development, identifying how changes affect trust boundaries, data flows, and attack surfaces. Where traditional security tools scan for known patterns in code, Yanrix reasons about how your system could be attacked given its architecture.

## What Yanrix does

Every time a pull request is opened, Yanrix analyzes the diff and produces:

- **STRIDE-classified threats** with confidence scores, severity ratings, and CWE mappings
- **Affected component identification** linking each threat to specific files and architectural boundaries
- **Risk assessment** with before/after ratings reflecting the change introduced by the PR
- **A living threat model manifest** stored as a GitHub Artifact, versioned across PRs, so your threat model grows with your codebase

Findings are posted as a structured PR comment. The full threat model manifest and an optional PDF report are available as downloadable workflow artifacts with 400-day retention. When a prior manifest exists from a previous run, Yanrix compares the new analysis against it, identifying new threats, resolved threats, and changed risk ratings. The manifest versions accumulate over time, giving your team a continuous record of how the threat landscape evolves with each change.

## Current capabilities

- **Analysis:** Full STRIDE classification, confidence scoring, CWE mapping, MITRE ATT&CK references, OWASP categorization, and deterministic security checks on any codebase
- **Output:** PR comment with severity-sorted findings, manifest artifact link, and optional PDF report
- **Model:** Bring your own key (BYOK). Yanrix runs on your LLM API key with your preferred provider.
- **Providers:** Anthropic Claude, OpenAI, and Google Gemini via LiteLLM
- **Privacy:** Your source code, repository files, and PR diffs are never transmitted to Yanrix servers. Code is sent from your GitHub runner directly to the LLM provider you configure, using your API key.
- **Cost:** Free and unlimited for public open source repositories. A limited demo is available for private repositories so you can evaluate the product on your own codebase.

## Quick start

### 1. Install the Yanrix GitHub App

Install the [Yanrix App](https://github.com/apps/yanrix) from the GitHub Marketplace. Select the repositories you want to analyze during installation.

The App grants Yanrix the permissions it needs to read repository contents, read PR diffs, and post PR comments. **You need both the App and the Action.** The App provides the permission grant and bot identity (`@yanrix[bot]`). The Action is the analysis engine that runs in your CI workflow.

### 2. Add the Action to your workflow

Create `.github/workflows/yanrix.yml` in your repository:

```yaml
name: Yanrix Threat Model

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  threat-model:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: yanrixhq/yanrix@v1
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

This default configuration uses Anthropic Claude and scans your entire repository. See [Configuration](#configuration) for additional options.

### 3. Configure your LLM API key

Add your API key as a repository secret. In your repository, go to **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Provider | Secret name | Key format |
|----------|-------------|------------|
| Anthropic | `ANTHROPIC_API_KEY` | Starts with `sk-ant-api03-` |
| OpenAI | `OPENAI_API_KEY` | Starts with `sk-` |
| Google Gemini | `GEMINI_API_KEY` | Alphanumeric string |

Only configure the secret for the provider you are using. See the [BYOK Configuration Guide](https://yanrix.dev/docs/byok) for step-by-step instructions on obtaining a key, minimum required API key scopes, and spend cap recommendations for each provider.

### 4. Open a pull request

Open or update a pull request. Yanrix will run automatically and post a threat model comment within a few minutes.

---

## Configuration

The Yanrix Action accepts the following inputs:

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `model` | No | `anthropic/claude-sonnet-4-20250514` | LLM model in `provider/model-name` format. See [Supported providers](#supported-providers) for valid values. |
| `target_folder` | No | Repository root | Path to a specific directory to analyze. When set, Yanrix scans only the specified folder instead of the full repository. |
| `use_large_context` | No | `false` | Use the 1M token context window. When enabled, Yanrix automatically selects the 1M-capable model for the chosen provider. When disabled, uses the standard 200K context ceiling. |
| `generate_pdf` | No | `false` | Generate a PDF threat model report. The report is uploaded as a workflow artifact alongside the manifest. |

### Selecting a provider

Set the `model` input and provide the matching API key as an environment variable:

**Anthropic Claude** (default):
```yaml
- uses: yanrixhq/yanrix@v1
  env:
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

**OpenAI:**
```yaml
- uses: yanrixhq/yanrix@v1
  with:
    model: "openai/gpt-4o"
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

**Google Gemini:**
```yaml
- uses: yanrixhq/yanrix@v1
  with:
    model: "gemini/gemini-3.1-pro-preview"
  env:
    GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
```

### Advanced: manual trigger with PDF generation

You can add a `workflow_dispatch` trigger to run Yanrix manually and generate a PDF report:

```yaml
name: Yanrix Threat Model

on:
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:
    inputs:
      use_large_context:
        description: "Use 1M token context window"
        required: false
        default: "false"
        type: choice
        options:
          - "false"
          - "true"
      generate_pdf:
        description: "Generate PDF threat model report"
        required: false
        default: "false"
        type: choice
        options:
          - "false"
          - "true"

permissions:
  contents: read
  pull-requests: write
  actions: read

jobs:
  threat-model:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: yanrixhq/yanrix@v1
        with:
          use_large_context: ${{ github.event.inputs.use_large_context || 'false' }}
          generate_pdf: ${{ github.event.inputs.generate_pdf || 'false' }}
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

When triggered manually from the Actions tab, the PDF report and threat model manifest are uploaded as workflow artifacts.

## How it works

Yanrix runs entirely on your GitHub Actions runner. When triggered by a pull request:

1. The Action checks out your repository and reads the PR diff
2. It assembles context from your codebase, including source code, dependency manifests, configuration files, and framework patterns
3. It sends the assembled context directly to your chosen LLM provider using your API key
4. The LLM performs STRIDE analysis and returns a structured threat model manifest
5. Yanrix validates the manifest, posts a summary comment on the PR, and uploads the full manifest as a GitHub Artifact

Each analysis is a complete evaluation of the codebase in the context of the PR changes. When a prior manifest exists, Yanrix compares the results to identify what is new, what has changed, and what has been resolved. Over successive PRs, the versioned manifests form a continuous threat modeling record for your repository.

## Privacy

Yanrix is designed around a strict privacy invariant: **your source code, repository files, and PR diffs are never transmitted to Yanrix servers.**

- The Action runs on your GitHub runner. Your code is sent directly from the runner to the LLM provider you configure, using your API key. Yanrix infrastructure never receives, processes, or stores your source code.
- Your LLM API key is stored as a GitHub Actions secret in your repository. It is passed directly to your chosen provider at runtime.
- The threat model manifest is uploaded as a GitHub Artifact under your repository.
- The Yanrix backend receives only structured metadata (finding counts, severity distributions, scan duration) for telemetry. No code, diffs, finding descriptions, or manifest content is transmitted to Yanrix.

See the [Yanrix Privacy Policy](https://yanrix.dev/privacy) for the complete data handling disclosure.

## Permissions

The Yanrix GitHub App requests the minimum permissions required for analysis:

| Permission | Level | Purpose |
|------------|-------|---------|
| Contents | Read | Access repository files during context assembly |
| Pull requests | Read & Write | Read PR diffs and post findings as PR comments |
| Metadata | Read | Required by GitHub for all Apps |

Yanrix does not request Actions, Administration, or Secrets permissions.

## Supported providers

Yanrix uses [LiteLLM](https://github.com/BerriAI/litellm) to support multiple LLM providers.

| Provider | Model input example | Required secret |
|----------|-------------------|-----------------|
| Anthropic | `anthropic/claude-sonnet-4-20250514` | `ANTHROPIC_API_KEY` |
| OpenAI | `openai/gpt-4o` | `OPENAI_API_KEY` |
| Google Gemini | `gemini/gemini-3.1-pro-preview` | `GEMINI_API_KEY` |

Azure and Amazon Bedrock are supported by LiteLLM but have not been tested with Yanrix. If you use these providers, community feedback is welcome.

## How Yanrix differs from code scanning tools

Yanrix is not a SAST tool, vulnerability scanner, or linter. Tools like Semgrep, SonarQube, and CodeQL analyze source code for known vulnerability patterns, insecure coding practices, and CVE matches. They are excellent at what they do, and Yanrix is designed to complement them.

Yanrix performs threat modeling. It reasons about architectural risk: how components interact, where trust boundaries exist, what data flows cross those boundaries, and what an attacker could exploit given the system's design. Each finding includes STRIDE classification, CWE mapping, MITRE ATT&CK references, and OWASP categorization to connect the architectural analysis to established security frameworks.

This is the analysis that traditionally happens in a whiteboard session or a design review document. Yanrix applies it continuously, on every PR, as the codebase evolves.

## Documentation

- [Getting Started](https://yanrix.dev/docs/getting-started) — Full setup walkthrough with detailed explanations
- [BYOK Configuration Guide](https://yanrix.dev/docs/byok) — API key setup, minimum scopes, and spend cap guidance per provider
- [Privacy Policy](https://yanrix.dev/privacy)

## Support

- [Open an issue](https://github.com/yanrixhq/yanrix/issues) for bug reports and feature requests
- [Yanrix website](https://yanrix.dev) for product information

## License

This project is licensed under the [Apache License 2.0](LICENSE).

---

Built by [SoluMates LLC](https://yanrix.dev).
