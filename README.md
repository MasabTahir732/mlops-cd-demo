MLOps Continuous Delivery (CD) Pipeline Demo

Author: Masab Tahir

## Project Overview
This repository contains a complete Continuous Delivery pipeline for a machine learning inference API. The project demonstrates the progression from Continuous Integration (CI) into controlled, repeatable delivery of tested artifacts using Docker, GitHub Container Registry (GHCR), and GitHub Actions[cite: 1].

## Tutorial Steps Completed
* **Flask ML API:** Built a lightweight inference service with `/predict` and `/health` endpoints[cite: 1].
* **Automated Testing:** Implemented validation checks using `pytest` to ensure API reliability before allowing artifacts into the delivery pipeline[cite: 1].
* **Containerization:** Packaged the application into an immutable Docker artifact to guarantee the "build-once, deploy-many" principle[cite: 1].
* **Semantic Versioning:** Linked Git tags (e.g., `v1.3.0`) directly to Docker image tags for exact deployment tracking[cite: 1].
* **Continuous Delivery Pipeline:** Configured `.github/workflows/cd.yml` to automate testing, building, and publishing to GHCR[cite: 1].
* **Environment Gates:** Established distinct GitHub Environments for `staging` and `production` deployments[cite: 1].
* **Manual Approval:** Configured a required reviewer gate before the artifact can be promoted to the production environment[cite: 1].
* **Bonus Exercise (Production Traceability):** Modified the `/health` endpoint to expose the `application_version`, `model_version`, and the exact `git_commit` hash[cite: 1].

## Custom Modifications (The "New Things")
* **Serverless Staging & Production Deployment:** Instead of provisioning external Ubuntu cloud servers and configuring SSH keys as outlined in the original tutorial, the `cd.yml` workflow was modified to pull, run, and test the Docker containers directly on the local GitHub Actions runner.
* **Dynamic Variable Injection:** Upgraded the `Dockerfile` to use `ARG` and `ENV` instructions, allowing the GitHub Actions pipeline to inject the short Git commit hash into the Python application dynamically at build time.
* **Python Module Path Resolution:** Updated the CI test command from `pytest` to `python -m pytest` to explicitly add the workspace directory to the Python path, resolving a `ModuleNotFoundError` during the automated pipeline run.
* **Lowercase Repository Enforcement:** Overrode the default `${{ github.repository }}` variable in the YAML file with a custom `IMAGE_NAME` environment variable (`ghcr.io/masabtahir732/mlops-cd-demo`). This successfully bypassed Docker's strict rejection of uppercase letters in image repository names.