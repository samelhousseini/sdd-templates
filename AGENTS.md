# AGENTS.md

## Project Overview
This repository contains spec-driven templates and research frameworks designed for AI application developers. It provides structured workflows for planning and executing documentation research, particularly focused on Azure services, SDKs, and AI frameworks.


## Repository Structure
- `.github/templates/` - Markdown templates for research planning and collection
- `.github/prompts/` - Prompt instruction files for research workflows
- `.github/chatmodes/` - Custom chat mode definitions for specialized research agents
- `.github/scratchpad/` - Working directory for generated research plans and collection logs (not version controlled)

## Key Components

### Research Templates
1. **research-plan-template.md** - Structured template for creating research plans with scope definition, library resolution, and refined search terms
2. **research-collection-template.md** - Template for executing research collection with findings, code snippets, and consolidated assets

### Prompt Files
1. **research-plan.prompt.md** - Instructions for generating ultra-minimal research plans focused on implementation details
2. **research-collect.prompt.md** - Instructions for systematic collection of code snippets, configuration, and documentation

### Chat Modes
1. **azure-docs-research.agent.md** - Specialized mode for Azure documentation research using Context7 and Azure MCP sources

## Workflow Instructions

### 1. Research Phase
Start with the **azure-docs-research** chat mode agent:

**Step 1: Generate Research Plan**
- Use: `/azure-docs-research-plan` (references `research-plan.prompt.md`)
- Provide your research requirements/user goals
- Output: `.github/scratchpad/research-plan-[TIMESTAMP].md`
- Plan includes:
  - Scope Snapshot (3-5 items, concise descriptions)
  - Topics (accurate and targeted search terms, ≤10 items)
  - Resolved Library Names (using Context7 library resolution)
  - Refined Search Terms (library + specific keyphrase pairs)
  - Open Notes (preparation, not final answers)

**Step 2: Execute Research Collection**
- Use: `/azure-docs-research-collect` (references `research-collect.prompt.md`)
- **IMPORTANT:** Attach the filled planning template from Step 1 to your GitHub Copilot chat session
- Output: `.github/scratchpad/research-collection-[TIMESTAMP].md`
- **NON-NEGOTIABLE RULES:**
  - DO NOT QUIT until EVERY refined search term has a completed Finding
  - DO NOT QUIT until Section 9 Quality Checklist is fully completed
  - Collect MORE EXTENSIVE code snippets than baseline examples
  - MANDATORY tool call sequence for each refined search term:
    - `upstash/context7/resolve-library-id`
    - `upstash/context7/get-library-docs` (tokens=16000)
    - `Azure MCP Server/documentation`
    - Optional: `azure/azure-mcp/search` or `websearch` for gaps

### 2. Implementation Phase
**Before invoking application prompts:**
- Attach the filled research templates (both plan and collection files from `.github/scratchpad/`) to your GitHub Copilot chat session
- These artifacts provide comprehensive context for code generation, configuration, and implementation

### Required Tools for Research Workflows
- **Context7 Tools:** `resolve-library-id`, `get-library-docs`
- **Azure MCP Tools:** `documentation`, `search`
- **Web Search:** `ms-vscode.vscode-websearchforcopilot/websearch`
- **File Operations:** Create, read, edit files in `.github/scratchpad/`

## Code Style & Conventions

### Markdown Formatting
- Use fenced code blocks with language tags (`python`, `bash`, `bicep`, `json`, etc.)
- Replace secrets with `PLACEHOLDER_UPPER_SNAKE_CASE`
- Include inline citations as markdown links `[source](url)`
- Use concise comments in code snippets (not full tutorials)

### Research Quality Standards
- Every factual claim must have a citation
- Code snippets must be runnable or near-runnable
- Include environment variables, configuration, and commands
- Prioritize implementation assets over conceptual documentation
- No fabricated information - only documented facts

### File Naming
- Research plans: `research-plan-[YYYYMMDDTHHMMSSZ].md` (UTC timestamp)
- Collection logs: `research-collection-[YYYYMMDDTHHMMSSZ].md` (UTC timestamp)
- Use `-v2`, `-v3` suffixes for same-day iterations

## Common Libraries for Azure AI Development
When resolving libraries for research, prioritize these commonly used SDKs:

**Core AI & Agent Frameworks:**
- Microsoft Agent Framework (`microsoft.agentframework`)
- Azure OpenAI Service SDK (`azure.ai.openai`)
- Azure AI Foundry SDK (`azure.ai.foundry`)
- Semantic Kernel (`semantic_kernel`)

**Azure AI Services:**
- Azure AI Search (`azure.search.documents`)
- Azure AI Document Intelligence (`azure.ai.documentintelligence`)
- Azure AI Language, Vision, Speech, Translator
- Azure AI Content Safety (`azure.ai.contentsafety`)

**Azure Compute & Deployment:**
- Azure Container Apps CLI Extension
- Azure Kubernetes Service CLI
- Azure Functions Core Tools
- Azure App Service CLI

**Data & Storage:**
- Azure Cosmos DB SDK (`azure.cosmos`)
- Azure Blob Storage SDK (`azure.storage.blob`)
- Azure PostgreSQL SDK, Azure SQL Database SDK

**Observability:**
- Azure Monitor OpenTelemetry Exporter
- OpenTelemetry API & SDK
- OpenTelemetry FastAPI/HTTPX Instrumentation

## Security Considerations
- Never commit actual credentials or secrets
- Use placeholders for sensitive values in templates
- `.github/scratchpad/` should be in `.gitignore`
- Validate all external documentation sources

## Testing & Validation
- Research plans must pass readiness criteria before collection
- Collection must complete quality checklist (Section 9)
- Every refined search term must have a Finding or justified N/A
- Verify snippet depth exceeds baseline examples

## Iteration & Error Handling
- Maximum 5 global iterations for collection
- Retry failed tool calls once before logging error
- Continue with remaining terms if individual tool calls fail
- Only stop when ALL terms are researched or blocked

## Agent-Specific Guidance

### For Documentation Research Agent (azure-docs-research.agent.md)
- Role: Azure documentation research specialist
- Primary goal: Collect relevant code snippets
- Secondary goals: Configuration, how-tos, guidance
- Focus: Demos and proof of concepts (not production architecture)
- Never fabricate information - citation-backed facts only
- Never proceed to compose answer until full research complete

### Citation Rules
- Every factual claim needs inline markdown link
- Never invent links, API versions, or behavioral details
- If exact section not identifiable, state limitation transparently

### Prohibited Actions
- Skipping required tool calls
- Fabricating or paraphrasing unsupported details
- Expanding into architectural advice beyond retrieved material
- Providing samples not supported by sources
- Omitting citations
- Early termination before all refined terms researched

## Exit Criteria for Research Collection
Complete ONLY when:
- All refined search terms have Findings OR explicit N/A with documented attempts
- Quality checklist (Section 9) passes with all boxes checked
- Snippet depth exceeds baseline examples
- No unresolved refined terms remain

External response after completion: "I have completed the collection, please refer to the file `<filename>`."

## Notes for Coding Agents
- When user references prompt files, treat them as executable instructions
- The `.github/scratchpad/` directory is the working area for generated files
- Always validate template structure before populating
- Research workflows are multi-step processes requiring persistence
- Tool call sequences are mandatory and order-dependent
- Quality gates are non-negotiable - do not skip validation steps
