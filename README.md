# DevOps CI/CD Pipeline

A beginner friendly DevOps project: a small Flask app, packaged with Docker, with GitHub Actions running tests and building the container on every push and pull request.

The workflow is the project's continuous integration step. It does not publish the image or deploy to a cloud host; those can be added as a later milestone.

## Requirements

- Python 3.12
- Git
- Docker Desktop (optional for running locally, required for the local container steps)
- A GitHub account to run the Actions workflow

## Run on Windows

Open PowerShell in this folder and create a virtual environment:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

If PowerShell blocks activation, allow it for the current terminal session and activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

Run the app and open <http://localhost:5000/>:

```powershell
python app.py
```

The health endpoint is at <http://localhost:5000/health> and returns `{"status":"healthy"}`. Stop the app with **Ctrl+C**.

## Run the checks

```powershell
python -m pytest -v
```

The tests cover the home page and health endpoint.

## Build and run with Docker

Start Docker Desktop, then run:

```powershell
docker build -t devops-demo .
docker run --rm -p 5000:5000 devops-demo
```

Visit <http://localhost:5000/health>. Stop the container with **Ctrl+C**.

## GitHub Actions

The workflow at `.github/workflows/ci.yml` runs for pushes and pull requests. It installs Python dependencies, runs the tests, then builds the Docker image. It needs no cloud credentials or repository secrets.

To publish this already initialized local repository to GitHub:

1. Create a repository named `devops-cicd-pipeline`. For the simplest Actions setup, make it public and leave the README initialization unchecked. Do not commit passwords, access tokens, or other secrets.
2. In this folder, add the new repository as `origin` and push the existing `main` branch, replacing `YOUR_USERNAME` with your GitHub username:

   ```powershell
   git remote add origin https://github.com/YOUR_USERNAME/devops-cicd-pipeline.git
   git push -u origin main
   ```

3. Open the repository's **Actions** tab to see the workflow run.

If Git has not been configured on this computer, set your commit name and email first:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

## Project files

```text
app.py                     Flask app and health endpoint
test_app.py                Endpoint tests
requirements.txt           Python dependencies
Dockerfile                 Container build instructions
.dockerignore              Files excluded from Docker build context
.github/workflows/ci.yml   GitHub Actions pipeline
```
