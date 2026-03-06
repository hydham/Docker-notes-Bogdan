# Chapter 15 — (Optional) Push Your Docker Examples to GitHub

This is an optional workflow step: keep your Docker examples as a repository so you can reuse them later.

## Steps
From your project root folder (example: `docker-examples/`):

1) Initialize git:
```bash
git init
```

2) Stage files:
```bash
git add .
```

3) Commit:
```bash
git commit -m "docker crash course project files"
```

4) Create a GitHub repository (public or private).

5) Add remote origin (example URL):
```bash
git remote add origin <YOUR_REPO_URL>
```

6) Rename branch to main:
```bash
git branch -M main
```

7) Push:
```bash
git push -u origin main
```

Now your folder structure (nginx example, python examples, docker-compose examples) is stored in GitHub.
