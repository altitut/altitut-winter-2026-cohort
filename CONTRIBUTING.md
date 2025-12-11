# Contributing to Your Student Branch

## Workflow

### Initial Setup

1. **Wait for your branch to be created**: Your instructor will initialize your personal branch when you click "Initialize Repo" in the webapp.

2. **Clone the repository**:
   ```bash
   git clone https://github.com/altitut/altitut-winter-2026-cohort.git
   cd altitut-winter-2026-cohort
   ```

3. **Switch to your branch**:
   ```bash
   git checkout student-winter-2026-{your-student-id}
   ```

### Making Changes

1. **Make your changes**: Edit files, create new files, work on assignments.

2. **Check your changes**:
   ```bash
   git status
   git diff
   ```

3. **Stage your changes**:
   ```bash
   git add .
   ```

4. **Commit your changes**:
   ```bash
   git commit -m "Describe your changes here"
   ```

5. **Push to your branch**:
   ```bash
   git push origin student-winter-2026-{your-student-id}
   ```

### Best Practices

- **Commit often**: Small, frequent commits are better than large, infrequent ones
- **Write clear commit messages**: Describe what you changed and why
- **Test your code**: Make sure it works before pushing
- **Keep your branch up to date**: Pull changes regularly if the main branch is updated

## Getting Help

If you encounter any issues:
1. Check with your classmates
2. Consult the documentation
3. Ask your instructor

## Branch Protection

- **DO NOT** push to the `main` branch
- **DO NOT** create branches with names other than your assigned student branch
- **DO NOT** delete your student branch

Your student branch is your workspace - feel free to experiment!
