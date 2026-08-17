## Session startup

At the beginning of every new session, before working on the assigned task:

1. Check the parent repository and all submodules for remote updates.
2. Fast-forward the parent repository, then synchronize, initialize, and update all
   submodules recursively to their configured remote branches.
3. Only proceed with the assigned task after the update check completes.

Use non-destructive Git operations and preserve local changes. If local changes,
authentication, network access, or a non-fast-forward update prevents completion,
report the blocker before proceeding. Do not commit updated submodule pointers
unless the user explicitly requests a commit.

## Workspace boundaries

The `.agents` directory is reserved for agent instructions, skills, specialist
agent definitions, and related configuration. Do not create application projects
or user deliverables inside `.agents` unless the user explicitly requests that
location.

Before resolving a user-supplied relative path or creating files, identify the
current workspace or repository root from the available environment and project
context. IDE context supplied by the user takes precedence: when an open
`*.code-workspace` file or an explicit workspace path is shown, resolve relative
project paths against that VS Code workspace rather than the agent process's
current directory or the repository containing `.agents`.

Treat the repository containing `.agents` as the instructions repository, not
automatically as the user's active application workspace. Before the first write
for a task, state the resolved absolute destination and verify it against the IDE
context. If the intended workspace remains ambiguous and writing to the wrong
location would be costly, clarify the destination before proceeding.

## Skill and specialist-agent routing

Treat the paths below as a routing index, not as instructions to load everything.
For each task:

1. Identify the user's actual deliverable and repository before selecting a skill.
2. Select at most one primary workflow skill and the smallest number of domain
   skills needed to complete the work.
3. Read every selected `SKILL.md` completely before acting. Resolve its relative
   references from the skill directory and reuse its scripts, assets, and templates.
4. Use a specialist-agent definition only when it adds expertise or provides a
   bounded independent review. Read that agent's Markdown file before assigning or
   adopting the role.
5. Instructions available in the current runtime take precedence over local skill
   copies. User instructions and nested repository `AGENTS.md` files take precedence
   over this routing guide.
6. State which skill or specialist definition is being used and why. If no route
   matches, continue with normal repository conventions without loading unrelated
   material.

Do not spawn subagents merely because an agent definition exists. Spawn only when
the user, active runtime instructions, or the selected skill explicitly permits or
requires delegation. Keep each delegated task concrete, bounded, and independently
verifiable.

### Primary engineering workflow

Use `.agents/agent-skills/skills/<name>/SKILL.md` as the default source for general
software-engineering discipline:

| Use case | Primary skill | Optional specialist |
| --- | --- | --- |
| Clarify an idea or requirements | `idea-refine`, `interview-me` | `awesome-claude-code-subagents/categories/08-business-product/product-manager.md` |
| Plan or decompose work | `planning-and-task-breakdown`, `spec-driven-development` | `awesome-claude-code-subagents/categories/09-meta-orchestration/task-distributor.md` |
| Implement safely in increments | `incremental-implementation` | relevant language specialist below |
| Design an API or interface | `api-and-interface-design` | `awesome-claude-code-subagents/categories/01-core-development/api-designer.md` |
| Debug a failure | `debugging-and-error-recovery` | `awesome-claude-code-subagents/categories/04-quality-security/debugger.md` |
| Write or improve tests | `test-driven-development` | `agent-skills/agents/test-engineer.md` |
| Review code | `code-review-and-quality` | `agent-skills/agents/code-reviewer.md` |
| Simplify or refactor | `code-simplification` | `awesome-claude-code-subagents/categories/06-developer-experience/refactoring-specialist.md` |
| Migrate or deprecate code | `deprecation-and-migration` | `awesome-claude-code-subagents/categories/06-developer-experience/legacy-modernizer.md` |
| CI/CD or automation | `ci-cd-and-automation` | `awesome-claude-code-subagents/categories/03-infrastructure/devops-engineer.md` |
| Git workflow | `git-workflow-and-versioning` | `awesome-claude-code-subagents/categories/06-developer-experience/git-workflow-manager.md` |
| Performance | `performance-optimization` | `agent-skills/agents/web-performance-auditor.md` for web work |
| Observability | `observability-and-instrumentation` | `awesome-claude-code-subagents/categories/03-infrastructure/sre-engineer.md` |
| Security hardening | `security-and-hardening` | `agent-skills/agents/security-auditor.md` |
| Documentation or ADRs | `documentation-and-adrs` | `awesome-claude-code-subagents/categories/06-developer-experience/documentation-engineer.md` |
| Release or launch | `shipping-and-launch` | `awesome-claude-code-subagents/categories/03-infrastructure/deployment-engineer.md` |
| Work from authoritative sources | `source-driven-development` | `awesome-claude-code-subagents/categories/10-research-analysis/search-specialist.md` |

Skill paths in this table are relative to `.agents/agent-skills/skills/`.

### TypeScript, architecture, and TDD

For TypeScript-heavy work and rigorous implementation loops, use
`.agents/mattpocock-skills/skills/engineering/<name>/SKILL.md`:

- `tdd` for red-green-refactor work and test-first fixes.
- `diagnosing-bugs` and `triage` for isolating failures.
- `domain-modeling` and `codebase-design` for type-safe system design.
- `improve-codebase-architecture` for structural refactors.
- `implement` for executing an accepted specification.
- `to-spec` and `to-tickets` for converting requirements into executable work.
- `research` or `wayfinder` for unfamiliar codebases.
- `resolving-merge-conflicts` for conflict resolution.
- `code-review` for a TypeScript-focused review.

Use `.agents/awesome-claude-code-subagents/categories/02-language-specialists/typescript-pro.md`
for a bounded TypeScript specialist task, or another file in that directory for
Python, Swift, Kotlin, Go, Rust, Java, C#, PHP, Ruby, SQL, and framework-specific
work.

### React, Next.js, React Native, and Vercel

Use the following exact Vercel skills:

- React or Next.js correctness/performance:
  `.agents/vercel-agent-skills/skills/react-best-practices/SKILL.md`
- Component API and composition design:
  `.agents/vercel-agent-skills/skills/composition-patterns/SKILL.md`
- React Native or Expo:
  `.agents/vercel-agent-skills/skills/react-native-skills/SKILL.md`
- View Transitions:
  `.agents/vercel-agent-skills/skills/react-view-transitions/SKILL.md`
- Web design guideline review:
  `.agents/vercel-agent-skills/skills/web-design-guidelines/SKILL.md`
- Vercel optimization:
  `.agents/vercel-agent-skills/skills/vercel-optimize/SKILL.md`
- Vercel deployment:
  `.agents/vercel-agent-skills/skills/deploy-to-vercel/SKILL.md`

Use `nextjs-developer.md`, `react-specialist.md`, or
`expo-react-native-expert.md` from
`.agents/awesome-claude-code-subagents/categories/02-language-specialists/` only
when a separate specialist review or bounded implementation is useful.

### UI and design engineering

For creating or substantially redesigning interfaces, use one UI Craft mode:

- General product UI: `.agents/ui-craft/skills/ui-craft/SKILL.md`
- Restrained/minimal UI: `.agents/ui-craft/skills/ui-craft-minimal/SKILL.md`
- Dense dashboards: `.agents/ui-craft/skills/ui-craft-dense-dashboard/SKILL.md`
- Editorial/content-led pages: `.agents/ui-craft/skills/ui-craft-editorial/SKILL.md`

For focused review or refinement, use exactly the relevant Jakub Krehel skill:

- Typography: `.agents/jakubkrehel-skills/skills/better-typography/SKILL.md`
- Colour systems and contrast: `.agents/jakubkrehel-skills/skills/better-colors/SKILL.md`
- Layout, grouping, spacing, and responsiveness:
  `.agents/jakubkrehel-skills/skills/better-layout/SKILL.md`
- Accessibility: `.agents/jakubkrehel-skills/skills/better-accessibility/SKILL.md`
- Interaction details and animation: `.agents/jakubkrehel-skills/skills/better-ui/SKILL.md`
- Interface structure: `.agents/jakubkrehel-skills/skills/better-interface/SKILL.md`
- Holistic UI review: `.agents/jakubkrehel-skills/skills/interface-review/SKILL.md`
- Product copy: `.agents/jakubkrehel-skills/skills/better-writing/SKILL.md`

For implementation from a screenshot or reference image, use
`.agents/codex-skills/skills/img-to-frontend/SKILL.md`. For a bounded design role,
use `ui-designer.md` or `design-bridge.md` under
`.agents/awesome-claude-code-subagents/categories/01-core-development/`.

### Apple platforms

Use `.agents/dimillian-skills/<name>/SKILL.md`:

- Swift concurrency: `swift-concurrency-expert`
- SwiftUI architecture and patterns: `swiftui-ui-patterns`
- SwiftUI view cleanup: `swiftui-view-refactor`
- SwiftUI performance: `swiftui-performance-audit`
- Liquid Glass adoption: `swiftui-liquid-glass`
- iOS debugging: `ios-debugger-agent`
- macOS SwiftPM packaging: `macos-spm-app-packaging`
- macOS menu-bar apps with Tuist: `macos-menubar-tuist-app`
- App Store release notes: `app-store-changelog`
- Multi-angle bug or review work, only when delegation is allowed:
  `bug-hunt-swarm`, `review-swarm`, or `orchestrate-batch-refactor`

For a bounded language specialist, use
`.agents/awesome-claude-code-subagents/categories/02-language-specialists/swift-expert.md`.

### OpenAI, artifacts, browser work, security, and deployment

Prefer the OpenAI-maintained skills under `.agents/openai-skills/skills/`:

- OpenAI products/API: `.system/openai-docs/SKILL.md`
- Create or install skills/plugins: `.system/skill-creator/SKILL.md`,
  `.system/skill-installer/SKILL.md`, or `.system/plugin-creator/SKILL.md`
- Image generation/editing: `.system/imagegen/SKILL.md`
- Browser automation: `.curated/playwright/SKILL.md`
- PDFs and notebooks: `.curated/pdf/SKILL.md` and
  `.curated/jupyter-notebook/SKILL.md`
- Security review: `.curated/security-best-practices/SKILL.md`
- Threat modelling: `.curated/security-threat-model/SKILL.md`
- Security ownership analysis: `.curated/security-ownership-map/SKILL.md`
- GitHub CI failures or review comments: `.curated/gh-fix-ci/SKILL.md` or
  `.curated/gh-address-comments/SKILL.md`
- Deployments: `.curated/cloudflare-deploy/SKILL.md`,
  `.curated/netlify-deploy/SKILL.md`, `.curated/render-deploy/SKILL.md`, or
  `.curated/vercel-deploy/SKILL.md`
- Figma tasks: choose the narrow matching `.curated/figma*/SKILL.md`
- Notion tasks: choose the narrow matching `.curated/notion-*/SKILL.md`

### Specialist-agent catalog

The broad specialist library is under
`.agents/awesome-claude-code-subagents/categories/`. Choose one exact Markdown
definition by domain; do not load a whole category:

- `01-core-development`: frontend, backend, full-stack, API, GraphQL, UI design,
  microservices, WebSockets, Electron.
- `02-language-specialists`: language and framework experts.
- `03-infrastructure`: cloud, DevOps, SRE, containers, Kubernetes, Terraform,
  networking, databases, incidents.
- `04-quality-security`: testing, debugging, performance, accessibility,
  architecture review, security, compliance.
- `05-data-ai`: databases, data engineering/analysis, ML, LLMs, MLOps, NLP,
  prompt engineering.
- `06-developer-experience`: refactoring, modernization, builds, dependencies,
  CLIs, MCP, documentation, Git workflows.
- `07-specialized-domains`: payments, healthcare, fintech, mobile, games, IoT,
  blockchain, SEO, email deliverability.
- `08-business-product`: product/project management, UX research, business
  analysis, content, sales, customer success, legal.
- `09-meta-orchestration`: multi-agent coordination, task distribution, context,
  workflow orchestration. Use only when delegation is explicitly allowed.
- `10-research-analysis`: literature, market, competitive, trend, cohort, and
  first-principles research.

For routine code review, security, test, or web-performance work, prefer the four
smaller definitions in `.agents/agent-skills/agents/` before the broader catalog.

### Discovery fallback

If the route is not listed above, search locally before improvising:

```sh
rg --files .agents -g 'SKILL.md' -g '*.md'
rg -n "relevant keyword" .agents/*/skills .agents/*/agents \
  .agents/awesome-claude-code-subagents/categories
```

The large `.agents/awesome-codex-skills` catalog is a secondary fallback for
specialized automation and integrations. Search it by product or use-case name and
read only the matching `SKILL.md`; do not scan or load the full catalog.
