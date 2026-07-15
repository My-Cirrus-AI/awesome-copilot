---
title: 'Using Copilot Spaces'
description: 'Learn how to create, manage, and use Copilot Spaces to give GitHub Copilot curated, project-specific context for richer, more accurate conversations.'
authors:
  - GitHub Copilot Learning Hub Team
lastUpdated: 2026-07-15
estimatedReadingTime: '8 minutes'
tags:
  - spaces
  - context
  - knowledge-base
  - collaboration
relatedArticles:
  - ./understanding-copilot-context.md
  - ./understanding-mcp-servers.md
  - ./building-custom-agents.md
  - ./defining-custom-instructions.md
prerequisites:
  - Basic familiarity with GitHub Copilot
  - GitHub Copilot Pro, Pro+, Business, or Enterprise plan
---

Copilot Spaces are shared, curated knowledge bases that you attach to Copilot conversations to ground its responses in your team's actual code, documentation, and internal standards. Instead of pasting context into every chat, you build a Space once and load it whenever you need it.

This article explains what Spaces are, when to use them, and how to create and manage them.

## What Is a Copilot Space?

A Copilot Space is a named collection of resources — repositories, files, GitHub issues, free-text notes, and custom instructions — that you curate for a specific project, team, or workflow. When you load a Space into a Copilot session, all of its context becomes available to the model.

Think of a Space as a persistent briefing document for Copilot:

- A **project Space** might include architecture docs, coding standards, and pointers to key files.
- A **team Space** might package onboarding materials and links to active initiative issues.
- A **workflow Space** might contain step-by-step instructions and templates that turn Copilot into a repeatable process engine.

Spaces auto-update as the underlying repositories and issues change, so the context is always current without any manual maintenance.

## When to Use Spaces

| Scenario | Without a Space | With a Space |
|----------|----------------|--------------|
| "What's our policy on secret scanning?" | Copilot gives a generic answer | Copilot reads your actual internal policy doc |
| "Generate a component following our patterns" | Copilot uses generic React patterns | Copilot follows your team's specific conventions |
| "Write my weekly status update" | You write it from scratch | Copilot follows your template and pulls data from attached issues |
| "Onboard me to this codebase" | Copilot guesses at architecture | Copilot walks you through your own architecture docs |

Spaces are particularly powerful for:

- **Internal documentation**: Policies, runbooks, architecture decision records
- **Team standards**: Coding guidelines, review checklists, naming conventions
- **Repeatable workflows**: Sprint reports, release checklists, incident summaries
- **Onboarding**: Getting new team members or agents up to speed quickly

## Resource Types

A Space can contain any combination of these resource types:

| Type | Description |
|------|-------------|
| **Repository** | Attach one or more GitHub repos — Copilot can read code and docs from them |
| **GitHub file** | Pin a specific file path from a repository |
| **GitHub issue** | Attach an issue for live tracking of requirements or progress |
| **Free text** | Add plain text notes, inline documentation, or instructions directly |
| **Custom instructions** | Behavioral guidance that tells Copilot how to respond when this Space is loaded |

## Creating a Space

### From GitHub.com

1. Navigate to your profile or organization settings
2. Open **Copilot** → **Spaces**
3. Click **New Space**
4. Give it a name, description, and optional instructions
5. Attach resources: repositories, files, issues, or free text

### Via the CLI (`gh api`)

You can create and manage Spaces programmatically:

```bash
# Create a personal space
gh api users/{username}/copilot-spaces \
  -X POST \
  -f name="My Project Space" \
  -f description="Architecture docs and coding standards for Project X" \
  -f general_instructions="Always follow the patterns in docs/architecture.md" \
  -f visibility="private"
```

**Scope requirements**: Your PAT needs `read:user` for reads and `user` for writes. If write operations return a 404, refresh your token:

```bash
gh auth refresh -h github.com -s user
```

### Attaching Resources

Resources are managed as an array. Each update **replaces** the entire list, so always include all existing resources when adding a new one:

```bash
gh api users/{username}/copilot-spaces/{number} \
  -X PUT \
  --input - <<'EOF'
{
  "resources_attributes": [
    {
      "resource_type": "github_file",
      "metadata": {
        "repository_id": 12345,
        "file_path": "docs/architecture.md"
      }
    },
    {
      "resource_type": "github_issue",
      "metadata": {
        "repository_id": 12345,
        "number": 42
      }
    },
    {
      "resource_type": "free_text",
      "metadata": {
        "name": "Coding Standards",
        "text": "Always use TypeScript strict mode. Prefer named exports."
      }
    }
  ]
}
EOF
```

To **remove** a resource, include its `id` with `"_destroy": true` in the array.

## Loading a Space in a Session

### Via the `copilot-spaces` Skill

If you have the `copilot-spaces` skill installed (available from the [Skills directory](../../skills/)), you can ask Copilot to load a Space naturally:

```
Load the Accessibility space
```

```
What Copilot spaces are available for our team?
```

```
Using the security space, what's our policy on secret scanning?
```

The skill uses MCP tools (`mcp__github__list_copilot_spaces` and `mcp__github__get_copilot_space`) to discover and load the Space content into the conversation.

### Via the GitHub MCP Server

If you have the GitHub MCP server configured, these tools are available directly:

| Tool | Purpose |
|------|---------|
| `mcp__github__list_copilot_spaces` | List all Spaces accessible to you |
| `mcp__github__get_copilot_space` | Load a specific Space by owner and name |

> **Note**: Space names are case-sensitive. Use `list_copilot_spaces` to find the exact name before calling `get_copilot_space`.

## Managing Spaces

### Updating a Space

```bash
# Update instructions only
gh api users/{username}/copilot-spaces/{number} \
  -X PUT \
  -f general_instructions="Updated instructions here"

# Update name, description, and instructions together
gh api users/{username}/copilot-spaces/{number} \
  -X PUT \
  -f name="New Name" \
  -f description="Updated description" \
  -f general_instructions="Updated instructions"
```

**Updatable fields**: `name`, `description`, `general_instructions`, `icon_type`, `icon_color`, `visibility` (`"private"` / `"public"`), `base_role` (`"no_access"` / `"reader"`), `resources_attributes`.

### Listing Your Spaces

```bash
# List personal spaces
gh api users/{username}/copilot-spaces

# List organization spaces
gh api orgs/{org}/copilot-spaces
```

### Sharing a Space

- Set `visibility` to `"public"` to allow anyone to read the Space
- Set `base_role` to control what non-collaborator members can do (`"reader"` or `"no_access"`)
- Add collaborators to give specific users or teams access

### Deleting a Space

```bash
gh api users/{username}/copilot-spaces/{number} -X DELETE
```

## Organization Spaces

Spaces can be created at the organization level as well as the user level. Organization Spaces are ideal for company-wide knowledge bases: security policies, engineering standards, or onboarding materials shared across all teams.

```bash
# Create an org space
gh api orgs/{org}/copilot-spaces \
  -X POST \
  -f name="Engineering Standards" \
  -f description="Org-wide coding standards and security policies" \
  -f visibility="private"
```

## Using Spaces as Workflow Engines

One powerful pattern is building a Space that acts as a **workflow template**. Instead of just reference material, the Space contains step-by-step instructions and structured output formats. When you load it, Copilot follows the workflow rather than improvising.

**Example: Weekly PM Update Space**

The Space contains:
- A template for the weekly update format
- Links to active initiative tracking issues
- Instructions to pull data from linked issues and dashboards
- A step-by-step process for drafting each section

When you say "Write my weekly update using the PM Weekly Updates space", Copilot:

1. Loads the Space and reads the workflow instructions
2. Fetches data from the attached issues
3. Drafts each section following the template
4. Shows progress after each step so you can steer

This pattern works well for any repeatable, context-heavy task: incident postmortems, release notes, architecture reviews, and sprint retrospectives.

## Tips

- **Keep instructions actionable**: Frame `general_instructions` as directives ("Always follow the patterns in `docs/api-conventions.md`"), not suggestions.
- **Use issue resources for live data**: Attaching GitHub issues gives Copilot access to current comments and status, not a snapshot.
- **Separate concerns**: Create focused Spaces rather than one monolithic Space. A narrow, well-curated Space is more useful than a broad one.
- **Space content can be large**: Use targeted questions to get the most relevant context rather than asking Copilot to summarize everything.
- **Combine Spaces with instructions**: Use `general_instructions` within a Space for workflow-specific guidance, and `.github/instructions/` files for repository-wide standards.

> **Note**: The Copilot Spaces API is functional but may require the `copilot_spaces_api` feature flag. Availability may vary by plan and organization settings.

## Next Steps

- **Install the Skill**: Add the [copilot-spaces skill](../../skills/) to your Copilot setup to enable natural language Space loading
- **Understand Context**: [Understanding Copilot Context](../understanding-copilot-context/) — How Copilot uses the context you provide
- **Add MCP Servers**: [Understanding MCP Servers](../understanding-mcp-servers/) — Configure the GitHub MCP server to enable Space tools
- **Write Instructions**: [Defining Custom Instructions](../defining-custom-instructions/) — Complement Spaces with file-based instructions for your repository

---
