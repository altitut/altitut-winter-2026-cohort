# API Integration Guide

This document describes how external systems (backend, webapp, instructor dashboard) interact with this repository.

## Backend Integration

### Endpoint: POST /github/init-repo

The backend should implement the following workflow:

1. **Authenticate with GitHub**: Use a GitHub App or Personal Access Token with appropriate permissions
   - Required permissions: `repo` (full control of private repositories)
   - Recommended: Use a GitHub App with `contents: write` permission

2. **Get the main branch SHA**:
   ```
   GET /repos/altitut/altitut-winter-2026-cohort/git/refs/heads/main
   ```
   Response contains the SHA of the latest commit on main

3. **Create a new student branch**:
   ```
   POST /repos/altitut/altitut-winter-2026-cohort/git/refs
   {
     "ref": "refs/heads/student-winter-2026-{studentID}",
     "sha": "{main-branch-sha}"
   }
   ```

### Example Implementation (Node.js)

```javascript
const { Octokit } = require("@octokit/rest");

async function initStudentRepo(studentID) {
  const octokit = new Octokit({
    auth: process.env.GITHUB_TOKEN
  });

  // Get main branch SHA
  const { data: mainRef } = await octokit.git.getRef({
    owner: 'altitut',
    repo: 'altitut-winter-2026-cohort',
    ref: 'heads/main'
  });

  const mainSha = mainRef.object.sha;

  // Create student branch
  const { data: newBranch } = await octokit.git.createRef({
    owner: 'altitut',
    repo: 'altitut-winter-2026-cohort',
    ref: `refs/heads/student-winter-2026-${studentID}`,
    sha: mainSha
  });

  return newBranch;
}
```

## Instructor Dashboard Integration

### Live Workspaces Tab

The Instructor Dashboard should list all student branches:

1. **List all branches**:
   ```
   GET /repos/altitut/altitut-winter-2026-cohort/branches
   ```

2. **Filter branches by pattern**:
   Filter the response to only include branches matching `student-winter-2026-*`

3. **Get branch details** (optional):
   ```
   GET /repos/altitut/altitut-winter-2026-cohort/branches/student-winter-2026-{studentID}
   ```

4. **Get recent commits** (optional):
   ```
   GET /repos/altitut/altitut-winter-2026-cohort/commits?sha=student-winter-2026-{studentID}
   ```

### Example Implementation (React/TypeScript)

```typescript
import { Octokit } from "@octokit/rest";

interface StudentWorkspace {
  studentID: string;
  branchName: string;
  lastCommit: string;
  lastUpdated: Date;
}

async function listStudentWorkspaces(): Promise<StudentWorkspace[]> {
  const octokit = new Octokit({
    auth: process.env.GITHUB_TOKEN
  });

  const { data: branches } = await octokit.repos.listBranches({
    owner: 'altitut',
    repo: 'altitut-winter-2026-cohort',
    per_page: 100
  });

  const studentBranches = branches.filter(branch => 
    branch.name.startsWith('student-winter-2026-')
  );

  const workspaces: StudentWorkspace[] = await Promise.all(
    studentBranches.map(async (branch) => {
      const studentID = branch.name.replace('student-winter-2026-', '');
      
      const { data: commits } = await octokit.repos.listCommits({
        owner: 'altitut',
        repo: 'altitut-winter-2026-cohort',
        sha: branch.name,
        per_page: 1
      });

      return {
        studentID,
        branchName: branch.name,
        lastCommit: commits[0]?.commit.message || 'No commits',
        lastUpdated: new Date(commits[0]?.commit.author?.date || '')
      };
    })
  );

  return workspaces;
}
```

## Security Considerations

1. **Authentication**: Use GitHub Apps or fine-grained personal access tokens
2. **Branch Protection**: Consider protecting the `main` branch to prevent accidental changes
3. **Permissions**: Students should only have write access to their own branches
4. **Rate Limiting**: Implement caching and respect GitHub API rate limits

## GitHub API Resources

- [GitHub REST API Documentation](https://docs.github.com/en/rest)
- [Create a reference](https://docs.github.com/en/rest/git/refs#create-a-reference)
- [List branches](https://docs.github.com/en/rest/branches/branches#list-branches)
- [Get a commit](https://docs.github.com/en/rest/commits/commits#get-a-commit)
