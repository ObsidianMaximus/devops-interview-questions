# CI/CD & GitHub Actions: Key Interview Questions and Answers

---

## 1. What is CI/CD?

**CI/CD** stands for Continuous Integration and Continuous Delivery/Deployment:

- **Continuous Integration (CI):**  
  The practice of automatically building and testing code every time changes are pushed to a shared repository. This helps catch bugs early and ensures that code merges are smooth.

- **Continuous Delivery/Deployment (CD):**  
  The automation of delivering code to production or staging environments. Continuous Delivery means code is always in a deployable state; Continuous Deployment goes a step further by automatically pushing changes to production once they pass all checks.

---

## 2. How do GitHub Actions work?

**GitHub Actions** is a CI/CD platform built into GitHub that allows you to automate workflows for your repositories.

- Workflows are defined in YAML files located in `.github/workflows/` in your repository.
- Each workflow can respond to various GitHub events (such as `push`, `pull_request`, `issue`, etc.).
- Workflows consist of jobs, and jobs consist of steps.
- GitHub hosts runners (machines) that execute your workflows.
- You can use pre-built actions from the marketplace or write custom ones.

---

## 3. What are runners?

A **runner** is a server that runs your GitHub Actions jobs.

- **GitHub-hosted runners:** Provided and managed by GitHub; support popular OSes (Linux, Windows, macOS).
- **Self-hosted runners:** You can run your own machines (on-premises or in the cloud) as runners.
- Runners pick up jobs from the GitHub Actions service and execute the steps in your workflow.

---

## 4. Difference between jobs and steps

**Job:**
- A collection of steps that run in the same runner environment.
- Jobs can run in parallel or sequentially (with dependencies).
- Each job gets a fresh runner instance.

**Step:**
- An individual task within a job (e.g., running a script, checking out code, executing a command).
- Steps within a job run sequentially and share the same environment.

**Example:**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code   # Step 1
      - name: Run tests       # Step 2
```

---

## 5. How to secure secrets in GitHub Actions?

- Store sensitive values (API keys, passwords, tokens) in the **GitHub Secrets** section (`Settings > Secrets and variables > Actions`) for your repository or organization.
- Access secrets in workflows via `${{ secrets.SECRET_NAME }}`.
- Secrets are masked in logs and cannot be echoed or printed.
- Never hardcode secrets in your workflow YAML or source code.

---

## 6. How to handle deployment errors?

- Use `continue-on-error: false` (default) to stop workflow on errors.
- Add error handling in your scripts (e.g., try-catch in shell or Node.js).
- Use job or step `if:` conditionals to handle failures gracefully.
- Set up notifications (Slack, email, etc.) for failed deployments.
- Use environment protection rules in GitHub to require manual approval before deploying to production.

---

## 7. Explain the Docker build-push workflow.

**Build:**
- Create a Docker image from your application source using a Dockerfile  
  (`docker build -t myimage:tag .`).

**Tag:**
- Optionally give the image a version/tag  
  (`docker tag myimage:tag myrepo/myimage:tag`).

**Login:**
- Authenticate to your Docker registry  
  (`docker login`).

**Push:**
- Upload the image to a Docker registry  
  (`docker push myrepo/myimage:tag`).

**In GitHub Actions:**
```yaml
- name: Build Image
  run: docker build -t myrepo/myimage:tag .
- name: Login
  run: echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
- name: Push Image
  run: docker push myrepo/myimage:tag
```

---

## 8. How can you test a CI/CD pipeline locally?

- **act:** Use the [`act`](https://github.com/nektos/act) tool to run GitHub Actions workflows locally.
- **Docker Compose:** For Docker-based workflows, use Docker Compose to mimic build and test steps.
- **Shell scripts:** Execute pipeline scripts manually on your local machine.
- **Mock environments:** Use tools like [LocalStack](https://github.com/localstack/localstack) to mock cloud services.
- **Linting/Validation:** Use [`actionlint`](https://github.com/rhysd/actionlint) or GitHub Actions workflow syntax checker to validate your YAML before pushing.

---