---
name: gh-pr-flow
description: GitHub PR review workflow using GraphQL. Prefer over raw gh CLI or GitHub MCP tools when working with PR review comments and threads.
---

# GitHub PR Review Flow

Supports standalone operations or full workflow. Ask user what they need. Default to full workflow if they share a PR URL without specifics.

## Standalone Operations

### Fetch PR review threads

```bash
gh api graphql -f query='{
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 100) {
        nodes {
          id
          isResolved
          line
          path
          comments(first: 1) {
            nodes { id body author { login } }
          }
        }
      }
    }
  }
}' --jq '
  ["thread_id","resolved","path","line","body"],
  (.data.repository.pullRequest.reviewThreads.nodes[] | [
    .id,
    .isResolved,
    .path,
    .line,
    (.comments.nodes[0].body | gsub("\n";" ")[0:150])
  ]) | @csv'
```

Add `select(.isResolved == false)` to filter unresolved only.

### Resolve a thread
```bash
gh api graphql -f query='mutation { resolveReviewThread(input: {threadId: "PRRT_xxx"}) { thread { isResolved } } }'
```

### Unresolve a thread
```bash
gh api graphql -f query='mutation { unresolveReviewThread(input: {threadId: "PRRT_xxx"}) { thread { isResolved } } }'
```

### Get full thread details
```bash
gh api graphql -f query='{ node(id: "PRRT_xxx") { ... on PullRequestReviewThread { comments(first: 10) { nodes { body author { login } } } } } }'
```

---

# Full Workflow

## Phase 1: Setup

### 1.1 Identify Repository Context

Extract owner and repo from:
- PR URL provided by user (e.g., `github.com/owner/repo/pull/123`)
- Current git remote: `git remote get-url origin`
- Ask user if ambiguous

Store as variables for all subsequent calls:
```
OWNER=<extracted-owner>
REPO=<extracted-repo>
PR_NUMBER=<extracted-number>
```

### 1.2 Determine User Preference

Infer from context:

**Hands-on** (default): User wants to review changes, says "help me", "walk through", "let's look at". Confirm before executing, show diffs before commit.

**Hands-off**: User says "just fix", "handle it", "address all comments". Process autonomously, commit and resolve when done.

## Phase 2: Retrieve Review Threads

### 2.1 Fetch Threads

```bash
gh api graphql -f query='{
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 100) {
        nodes {
          id
          isResolved
          line
          path
          comments(first: 1) {
            nodes { id body author { login } }
          }
        }
      }
    }
  }
}' --jq '
  ["thread_id","resolved","path","line","body"],
  (.data.repository.pullRequest.reviewThreads.nodes[] | [
    .id,
    .isResolved,
    .path,
    .line,
    (.comments.nodes[0].body | gsub("\n";" ")[0:150])
  ]) | @csv'
```

### 2.2 Filter by Resolution Status

Ask user: "Show only unresolved threads, or include resolved ones too?"

For unresolved only, add to jq: `select(.isResolved == false)`

### 2.3 Present Summary

Show threads in a clear table format:
- Thread ID (for later resolution)
- File path and line number
- Resolution status
- Truncated comment preview

## Phase 3: Analyze Comments

### 3.1 Categorize Each Thread

For each unresolved thread, categorize:
- **Code change required**: Bug fix, refactor, style change
- **Question/clarification**: Needs response, not code
- **Suggestion**: Optional improvement
- **Nitpick**: Minor issue, low priority

### 3.2 Fetch Full Comment When Needed

If truncated preview is insufficient, fetch full body:

```bash
gh api graphql -f query='{
  node(id: "THREAD_ID") {
    ... on PullRequestReviewThread {
      comments(first: 10) {
        nodes { body author { login } createdAt }
      }
    }
  }
}'
```

## Phase 4: Plan Approach

### 4.1 Create Action Plan

For each actionable thread, determine:
- What file to modify
- What change to make
- Whether it affects other code

### 4.2 User Confirmation (Hands-on Mode)

If hands-on: Present the plan and ask for confirmation before proceeding.

Show:
- Files to be modified
- Summary of each change
- Any potential risks or side effects

### 4.3 Create Todo List

Use TodoWrite to track each thread as a task. For example:
```
- [ ] Fix: path/to/file.py:42 - datetime.utcnow() deprecation
- [ ] Fix: path/to/other.py:118 - error message improvement
- [ ] Respond: question about API design
```

## Phase 5: Execute Changes

### 5.1 Process Each Thread

For each code-change thread:
1. Read the relevant file(s)
2. Make the fix
3. Mark todo as complete
4. Move to next thread

For question/clarification threads:
1. Note that a comment response is needed
2. Prepare response text for Phase 8

### 5.2 Hands-on Checkpoints

If hands-on mode: After each significant change, show diff and ask for approval.

## Phase 6: Commit and Push

### 6.1 Stage Changes

```bash
git add -A
git status
git diff --staged
```

### 6.2 Commit

Create descriptive commit message summarizing the PR feedback addressed.

If hands-on: Show commit message and ask for confirmation.

```bash
git commit -m "Address PR review feedback: <summary>"
```

### 6.3 Push

```bash
git push
```

If hands-on: Ask before pushing.

## Phase 7: Resolve Threads

### 7.1 Resolve Fixed Threads

For each thread where code was fixed:

```bash
gh api graphql -f query='mutation {
  resolveReviewThread(input: {threadId: "THREAD_ID"}) {
    thread { id isResolved }
  }
}'
```

### 7.2 Track Resolution

Report which threads were resolved and which remain open.

## Phase 8: Add Comments (Optional)

### 8.1 Respond to Questions

If any threads required clarification responses:

```bash
gh api graphql -f query='mutation {
  addPullRequestReviewComment(input: {
    pullRequestReviewId: "REVIEW_ID",
    body: "Response text here",
    inReplyTo: "COMMENT_ID"
  }) {
    comment { id }
  }
}'
```

Or simpler via REST:
```bash
gh api repos/OWNER/REPO/pulls/PR_NUMBER/comments/COMMENT_ID/replies -f body="Response"
```

### 8.2 Ask User for Response Content

If user wants to add custom responses, ask what they want to say for each thread requiring a response.

