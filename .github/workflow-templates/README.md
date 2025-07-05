# GitHub Actions Workflow Templates

This directory contains ready-to-use GitHub Actions workflows for various CI/CD processes and deployment strategies.

## Available Templates

- [Node.js CI](./nodejs-ci.yml) - Continuous integration for Node.js applications
- [Python CI](./python-ci.yml) - Continuous integration for Python applications
- [Docker Build and Push](./docker-build-push.yml) - Build and push Docker images with optional vulnerability scanning and Kubernetes deployment

## How to Use

1. Copy the workflow YAML files into your repository's `.github/workflows` directory.
    ```
    mkdir -p /path/to/your/repo/.github/workflows
    cp /path/to/this/repo/.github/workflow-templates/nodejs-ci.yml /path/to/your/repo/.github/workflows/
    cp /path/to/this/repo/.github/workflow-templates/python-ci.yml /path/to/your/repo/.github/workflows/
    cp /path/to/this/repo/.github/workflow-templates/docker-build-push.yml /path/to/your/repo/.github/workflows/
    ```

2. Adjust the files to fit your project specifics:
    - Update branch names to match your development flow
    - Uncomment and configure deployment steps where needed
    - Add any additional steps or modifications for your specific needs

## What's Included

### Node.js CI

- Builds your Node.js application
- Runs linting and tests across different Node.js versions
- Caches npm modules to speed up builds
- Optional deployment steps to platforms like GitHub Pages, AWS S3, Netlify, or Vercel

### Python CI

- Installs dependencies and runs tests for Python applications
- Lints code using flake8 and formats code with black
- Performs type checking with mypy
- Caches pip to speed up builds
- Optional deployment steps to PyPI, Heroku, AWS Elastic Beanstalk, or Docker on AWS

### Docker Build and Push

- Builds and pushes Docker images using Buildx for multi-platform builds
- Tags images based on semver, branches, or commits
- Optional vulnerability scanning automatically uploaded to GitHub Security tab
- Optional deployment to Kubernetes using GKE, EKS, or AKS

## Best Practices

- Check the version compatibility of the action or tool you're using
- Keep action versions locked to ensure consistent builds
- Use secrets to protect sensitive information like API keys or credentials
- Regularly review and update your CI/CD configurations as your project evolves

These templates serve as a starting point and can be expanded or customized to fit the exact requirements of your project and team workflows.
