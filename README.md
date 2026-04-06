# Claude PR Review Agent 🤖

AI-powered code review agent that analyzes GitHub pull requests using Claude AI and provides structured, actionable feedback.

## Features

- ✅ **CLI Interface** — Simple command-line usage
- ✅ **Structured Reviews** — Organized by Code Quality, Security, Performance, Testing
- ✅ **Smart Error Handling** — Validates URLs, handles API failures gracefully
- ✅ **Diff Size Management** — Warns on large diffs, chunks oversized content
- ✅ **High Signal-to-Noise** — Concise, actionable insights (no fluff)
- ✅ **Severity Ratings** — 🔴 Critical, 🟡 Moderate, 🟢 Minor

---

## Setup

### 1. Download the Script

```bash
curl -O https://raw.githubusercontent.com/neo-sonicseeds/claude-builders-bounty/feat/pr-review-agent/claude-review-pr
chmod +x claude-review-pr
```

### 2. Set API Keys

**Required:**
```bash
export ANTHROPIC_API_KEY="sk-ant-..."  # Get from https://console.anthropic.com/
```

**Optional (to avoid GitHub rate limits):**
```bash
export GITHUB_TOKEN="ghp_..."  # Get from https://github.com/settings/tokens
```

### 3. Run

```bash
./claude-review-pr <pr-url>
```

---

## Usage

### Basic Review

```bash
./claude-review-pr https://github.com/owner/repo/pull/123
```

### Example Output

```
🔍 Parsing PR URL...
📦 Repository: facebook/react
🔢 PR Number: #12345

📥 Fetching PR diff...
✅ Diff fetched (45230 chars)

🤖 Analyzing with Claude AI...

────────────────────────────────────────────────────────────────────────────────
# PR Review: facebook/react#12345
🔗 https://github.com/facebook/react/pull/12345

## 🔍 Code Quality

🟡 **Moderate**: Function `processUpdates` has high cyclomatic complexity
- Consider extracting the switch statement into a strategy pattern
- Lines 156-234

🟢 **Minor**: Inconsistent error message formatting
- Use consistent casing for user-facing messages
- Lines 89, 102, 145

## 🔒 Security

🔴 **Critical**: Potential XSS vulnerability
- User input not sanitized before rendering
- Line 67: `innerHTML = userInput`
- Use `textContent` or sanitize with DOMPurify

## ⚡ Performance

🟡 **Moderate**: Redundant array iteration
- Array is mapped twice when single pass would suffice
- Lines 201-215

## 🧪 Testing

🟢 **Minor**: Missing edge case tests
- Add tests for empty array input
- Add tests for undefined props

────────────────────────────────────────────────────────────────────────────────

✨ Review complete!
```

---

## Error Handling

The tool handles various failure scenarios gracefully:

### Invalid PR URL
```
❌ Error: Invalid GitHub PR URL: invalid-url
Expected format: https://github.com/owner/repo/pull/123
```

### PR Not Found
```
❌ Error: PR not found: owner/repo#999
Check that the repository and PR number are correct.
```

### Rate Limit
```
❌ Error: GitHub API rate limit exceeded. Set GITHUB_TOKEN environment variable.
```

### Missing API Key
```
❌ Error: ANTHROPIC_API_KEY environment variable not set.
Get your API key from https://console.anthropic.com/
```

### Oversized Diff
```
⚠️  Warning: Diff is very large (250.3 KB). Review may be incomplete.
```

---

## Requirements

- **Python 3.7+** (uses standard library only, no external dependencies)
- **Anthropic API Key** (Claude access)
- **GitHub Token** (optional, recommended for private repos or high usage)

---

## How It Works

1. **Parses** the GitHub PR URL to extract owner, repo, and PR number
2. **Fetches** the PR diff via GitHub REST API
3. **Validates** diff size (warns if >100KB, chunks if >200KB)
4. **Analyzes** the diff using Claude AI with a structured prompt
5. **Outputs** a clean Markdown review with severity ratings

---

## Review Categories

The agent evaluates PRs across four dimensions:

| Category | Focus Areas |
|----------|-------------|
| **Code Quality** | Design patterns, readability, maintainability, complexity |
| **Security** | Vulnerabilities, input validation, authentication, data exposure |
| **Performance** | Algorithm efficiency, resource usage, scalability issues |
| **Testing** | Test coverage, edge cases, test quality |

---

## Severity Ratings

- 🔴 **Critical** — Must be fixed before merge (security, breaking changes)
- 🟡 **Moderate** — Should be addressed (performance, maintainability)
- 🟢 **Minor** — Nice to have (style, conventions, documentation)

---

## Limitations

- **Diff Size**: Truncates diffs >200KB to stay within Claude API limits
- **Context**: Analyzes only the diff, not the full codebase
- **Language Support**: Works best with common languages (JS, Python, Go, etc.)
- **Rate Limits**: Subject to GitHub and Anthropic API rate limits

---

## License

MIT

---

**Built with ❤️ by [Neo](https://github.com/neo-sonicseeds) @ SonicSeeds**