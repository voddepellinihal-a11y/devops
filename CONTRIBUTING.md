# Contributing Guidelines

Thank you for contributing to this project! Please follow these guidelines to ensure a smooth collaboration process.

## Branch Naming Convention

- Feature branches: `feature/<github-username>` (e.g., `feature/amogh`)
- Bug fixes: `fix/<short-description>`
- Documentation: `docs/<short-description>`

## Commit Message Format

Follow conventional commits:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

Examples:
```
feat(form): add date of birth field with validation
fix(validation): correct mobile number pattern
docs(readme): update Jenkins configuration steps
```

## Pull Request Process

1. **Create a feature branch** from `main`
2. **Make meaningful changes** - each PR should address a single concern
3. **Write clear commit messages** following the format above
4. **Push your branch** and create a PR to `main`
5. **PR title** should match the commit format
6. **PR description** should explain:
   - What changes were made
   - Why they were needed
   - How to test the changes

## Code Review Requirements

- **All PRs require at least 1 approval** before merging
- **Admin PRs require review from a team member** (not self-approved)
- **CI checks must pass** (Jenkins validation)
- **No direct pushes to `main`** - only via PR merge

## HTML Standards

- Use semantic HTML5 elements
- Include proper `DOCTYPE` declaration
- All form inputs must have associated `<label>` elements
- Use `aria-*` attributes for accessibility
- Validate HTML structure before committing

## Testing

Before submitting PR:
1. Open `registartion.html` in browser
2. Test form submission with valid/invalid data
3. Verify accessibility with keyboard navigation
4. Run Jenkins validation (automatic on PR)

## Merging

- Use **Squash and merge** for feature branches
- Delete branch after merge
- Update `main` locally after merge: `git checkout main && git pull`