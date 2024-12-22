# OS & Linux fundamentals

This section is dedicated OS & Linux fundamentals, which are the foundation of computing environments. Whether you want to run pipelines or spin up servers for Kubernetes, it's all powered by Linux. Most Docker containers use Linux images and as a DevOps engineer you will work most of the time with remote servers on cloud which use Linux operating system.

All the commands and concepts mentioned here are based on Ubuntu 20.04 LTS version.

To replicate the commands and examples in this repo, I used a Docker container with the following image configuration (Dockerfile):

```Dockerfile
FROM ubuntu:latest
LABEL authors="alexarlord-boop"


# Install sudo and adduser
RUN apt-get update && apt-get install -y sudo adduser


# entrypoint is used to run a command when the container is started
# top command is used to display the system summary information
# -b option is used to run top in batch mode = non-interactive
ENTRYPOINT ["top", "-b"]

```

