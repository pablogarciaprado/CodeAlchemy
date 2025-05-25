◀️ [Home](../README.md)

# GitHub

## Fork
**Forking a repository** serves as a way to create your own copy of someone else’s project on GitHub (or similar platforms), enabling you to make changes independently of the original project, keeping the original repository untouched.

**Purposes of Forking:**

1.	**Contributing to the Original Project**:

- Forking allows you to experiment, fix bugs, or add features without affecting the original repository.

- Once your changes are ready, you can submit them for inclusion in the original project via a **pull request**.

2.	**Customizing a Project**:

- If you want to use a repository as a base and adapt it to your specific needs, forking provides a personal copy that you can freely modify.

3.	**Backing Up or Archiving**:

- Forking creates a personal, independent snapshot of the original repository, preserving it even if the original project is removed or changed.

4.	**Learning and Experimentation**:

- Forking lets you explore the codebase, learn new concepts, and experiment with changes in a safe environment.

## Contributing to a project

1. Fork the repository
2. Create a new branch

- Option 1: old version
```bash
git checkout -b feature/your-feature-name
```

- Option 2: new version (recommended)
```bash
git switch -c <new_branch_name>
```

3. Make your changes
4. Commit your changes

```bash
git commit -m "Add your commit message"
```

5. Push to the branch

```bash
git push origin feature/your-feature-name
```

6. Create a pull request