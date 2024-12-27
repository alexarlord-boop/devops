FROM ubuntu:latest
LABEL authors="alexarlord-boop"


# Install sudo and adduser
RUN apt-get update && apt-get install -y sudo adduser less


# entrypoint is used to run a command when the container is started
# top command is used to display the system summary information
# -b option is used to run top in batch mode = non-interactive
ENTRYPOINT ["top", "-b"]
