# Development Container

## Pre-request

1. Install Rancher Desktop
2. Start Rancher Desktop
3. Configure Rancher Desktop to use the docker backend

## Build The Container manually

```bash
# Uncomment the export line for your OS Architecture
# Windows WSL1/WSL2
#export arch=linux/amd64
# Linux x86_64 instances machines
#export arch=linux/amd64
# Mac M1/M2 CPUs
export arch=linux/arm64
cd /path/to/dockerfile/
docker build \
    --build-arg UID=$(id -u) \
    --platform 'linux/arm64' \
    `pwd` \
    -f Dockerfile \
    -t "test-dev-container:1.0.0"
```

## Run The dev container manually

```bash
docker run --rm -it \
    -v "${HOME}/.ssh:/home/developer/.ssh" \
    -e "GIT_USERNAME=$(git config --get user.name)" \
    -e "GIT_USEREMAIL=$(git config --get user.email)" \
    "test-dev-container:1.0.0" \
    /bin/bash
```

## Use the container as a dev container in a project

 - The .devcontainer folder is populated with a devcontainer.json and a Dockerfile
 - The Dockerfile creates a custom container with terraform v1.4.0 and the latest AWS CLI
 - The devcontainer.json creates the environment that is used by vscode with the appropriate vscode extensions


## Notes

The contents of the .devcontainer folder should be added as a subrepo to repositories that would like to use devcontainers, then updates are made to a single devcontainer repository and the consumers will need to run a submodule update. This could be incorporated into a git hook of some form if using a tool like pre-commit etc.