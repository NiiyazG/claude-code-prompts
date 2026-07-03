# Claude Code Prompt Library Skill

Source: https://code.claude.com/docs/en/prompt-library
Saved: 2026-07-03

## When to Use
When coding, debugging, refactoring, reviewing, or planning — pick the right prompt template, fill in the slots, and use it.

## How to Use This Skill
1. Identify the task category (Discover → Design → Build → Ship → Operate)
2. Pick the matching prompt from the library below
3. Fill in the `{slots}` with concrete values from the current project
4. Deliver the filled prompt to the agent/coder

---

## Prompt Library

### 🔍 Discover / Understand

**Codebase overview:**
```
give me an overview of this codebase: architecture, key directories, and how the pieces connect
```

**Explain a file:**
```
explain what {path} does and how data flows through it. write it up as {format}
```
Example: `src/scheduler/queue.ts` → `an HTML page with a diagram`

**Search by behavior (not filename):**
```
where do we {behavior}?
```
Example: `validate uploaded file types`

**Impact analysis:**
```
what would break if I deleted {target}?
```
Example: `the retryWithBackoff helper`

**Git history:**
```
look through the commit history of {path} and summarize how it evolved and why
```

**Walk through as non-dev role:**
```
I am a {role}. walk me through what happens when a user {action}, from the UI down to the result
```
Example: `PM` → `clicks Export to PDF`

### 🏗️ Design / Plan

**Scope assessment:**
```
which files would I need to touch to {change}?
```
Example: `add a dark mode toggle to settings`

**Refactoring plan (no edits yet):**
```
plan how to refactor the {target} to {goal}. list the files you would change, but don't edit anything yet
```
Example: `payment module` → `support multiple currencies`

**Spec via interview:**
```
I want to build {feature}. interview me about implementation, UX, edge cases, and tradeoffs until we have covered everything, then write the spec to SPEC.md
```

**Edge case map:**
```
list the error states, empty states, and edge cases for {feature} that the design needs to cover
```
Example: `the file upload flow`

### 💻 Build / Implement

**Follow existing pattern:**
```
look at how {example} is implemented to understand the pattern, then build {new} the same way
```
Example: `the GitHub webhook handler` → `a Stripe webhook handler`

**Generate code docs:**
```
find {scope} without {format} comments and add them, matching the style already used in the file
```
Example: `the public functions in src/auth/` → `JSDoc`

**Small feature:**
```
add a {endpoint} endpoint that returns {payload}
```
Example: `/health` → `the app version and uptime`

**Build internal tool from scratch:**
```
create a {tool} using HTML, CSS, and vanilla JavaScript, then open it in my browser
```
Example: `drag-and-drop Kanban board with three columns`

**Implement from issue:**
```
read issue #{issue}, implement the fix, and run the tests
```

**Find and replace across codebase:**
```
find every place we say "{copy}" or a close variant, show me each one in context, then update them all to "{new}". leave tests and the changelog alone
```

**Draft document from examples:**
```
read the {examples} in {folder} to learn the structure and voice, then draft a new one for {topic}
```
Example: `privacy impact assessments` → `legal/pia/` → `the new analytics integration`

### 🧪 Build / Test

**Write → Run → Fix:**
```
write tests for {path}, run them, and fix any failures
```

**TDD approach:**
```
write tests for {feature} first, then implement it until they pass
```
Example: `the password reset flow`

**Coverage gap fill:**
```
read {report} and add tests for the lowest-covered files until each is above {target}%
```
Example: `coverage/coverage-summary.json` → `80`

### 🔧 Build / Refactor

**Pattern migration:**
```
migrate everything from {from} to {to}: identify every place that needs to change, then make the changes
```
Example: `the old logging API` → `the structured logger`

**Cross-language port:**
```
port {source} to {target}, keeping the same {keep}
```
Example: `this Python module` → `Rust` → `public API and test behavior`

**Performance optimization:**
```
optimize {target} to bring {metric} from {current} down to under {goal}
```
Example: `the search query` → `p95 latency` → `2s` → `500ms`

**Visual bug fix:**
```
the {element} extends {amount} beyond the {container} on {viewport}. fix it.
```
Example: `login button` → `20px` → `card border` → `mobile`

### 👀 Build / Review

**Pre-commit self-review:**
```
review my uncommitted changes and flag anything that looks risky before I commit
```

**PR review:**
```
review PR #{pr} and summarize what changed, then list any concerns
```

**Infra review:**
```
here is my Terraform plan output. what is this going to do, and is anything here going to cause problems?
```

**Security scan via sub-agent:**
```
use a subagent to review {path} for security issues and report what it finds
```

**Content review before sending:**
```
review {file} for {concerns} and list anything I should fix before it goes to {reviewer}
```
Example: `launch-post.md` → `unsupported claims, missing attributions` → `legal`

### 🎯 Build / Steer (Course Correct)

**Fix wrong approach:**
```
that is not right: {feedback}. try a different approach
```
Example: `the function signature needs to stay backward-compatible`

**Narrow scope:**
```
that is too much. keep only the changes to {scope} and undo your other edits
```
Example: `the validation logic in src/forms/`

**Turn correction into rule:**
```
you keep {mistake}. add a rule to CLAUDE.md so this stops happening
```
Example: `using default exports when this project uses named exports`

### 🐛 Operate / Debug

**Failing test:**
```
the {test} test is failing, find out why and fix it
```

**User-reported error:**
```
users are seeing {symptom} on {where}. investigate and tell me what is going on
```
Example: `500 errors` → `/api/settings`

**Build error:**
```
here is a build error. fix the root cause and verify the build succeeds
```

**Production incident:**
```
{symptom}. check the logs, recent deploys, and config changes, then tell me the most likely cause
```
Example: `the checkout endpoint started returning 500s an hour ago`

**Log query in plain language:**
```
show me all {events} for {scope} over {timeframe}. write the query, run it, and tell me what stands out
```
Example: `failed logins` → `the auth service` → `the past 24 hours`

### 📊 Data

**CSV analysis:**
```
read {file}, summarize the key patterns, and write the results to {output}
```
Example: `@reports/q1-signups.csv` → `an HTML page with charts, then open it in my browser`

**Generate variations from performance data:**
```
read {file}, find the underperforming {items}, and generate {n} new variations that stay under {limit} characters
```
Example: `@ads-performance.csv` → `headlines` → `20` → `90`

### 🤖 Automate

**Create a reusable skill:**
```
create a /{name} skill for this project that {steps}
```
Example: `ship` → `runs the linter and tests, then drafts a commit message`

**Add a hook:**
```
write a hook that {action} after every {event}
```
Example: `runs prettier` → `edit to a .ts or .tsx file`

**Session summary:**
```
summarize what we did this session and suggest what to add to CLAUDE.md
```

---

## Quick Reference by Task Type

| When you need to... | Use this keyword |
|---|---|
| Understand something | `explain`, `where do we`, `what would break` |
| Plan before building | `which files`, `plan how to`, `list the error states` |
| Build following a pattern | `look at how {x} is implemented` |
| Write tests | `write tests for`, `read {report} and add tests` |
| Refactor safely | `migrate everything from`, `port {x} to {y}` |
| Review changes | `review my uncommitted`, `review PR #` |
| Fix wrong direction | `that is not right`, `that is too much` |
| Debug | `find out why`, `investigate and tell me` |
| Analyze data | `summarize the key patterns` |
| Create tools/skills | `create a /{name} skill`, `write a hook` |
