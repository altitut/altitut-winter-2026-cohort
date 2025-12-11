# Altitut Winter 2026 Cohort - Source of Truth Repository

This is the **Source of Truth** repository where all student code for the Winter 2026 cohort will live on separate branches.

## How It Works

### 1. Student Repo Initialization
When a student clicks "Initialize Repo" in the webapp:
- The webapp calls the backend endpoint `POST /github/init-repo`
- The backend authenticates with GitHub
- The backend finds the main branch SHA of this repository
- A new branch is created: `student-winter-2026-{studentID}`
- Students work on their individual branches

### 2. Instructor Dashboard
The Instructor Dashboard's "Live Workspaces" tab:
- Calls the GitHub API to list all branches matching the pattern `student-winter-2026-*`
- Shows real-time progress of all students
- Displays branch activity and commits

## Branch Naming Convention

All student branches follow this pattern:
```
student-winter-2026-{studentID}
```

Example: `student-winter-2026-12345`

## Repository Structure

```
altitut-winter-2026-cohort/
├── README.md           # This file
├── .gitignore         # Git ignore rules
└── starter/           # Starter code/templates for students
```

## For Students

Your instructor will provide you with access to initialize your personal branch. Once initialized:
1. Clone this repository
2. Check out your branch: `git checkout student-winter-2026-{your-student-id}`
3. Start coding!
4. Commit and push your changes regularly

## For Instructors

To view all student branches:
```bash
git branch -r | grep student-winter-2026
```

Or use the Instructor Dashboard's "Live Workspaces" tab for a visual interface.