# If a specific version of the base image is required then replace the latest tag with the version number
FROM docker.io/amazon/aws-cli:latest AS aws-cli-source-image

ARG UID=1001

# The amazon linux source container includes amazon-linux-extras, use this to install the epel repository (Extra Packages for Enterprise Linux)
RUN amazon-linux-extras install epel -y

# Now install all the required packages for the dev container
RUN yum -y install \
      curl \
      bash-completion \
      bash-completion-extras \
      gcc \
      git \
      glibc-devel \
      gzip \
      jq \
      libffi-devel \
      openssl \
      openssl-devel \
      python3 \
      python3-devel \
      python-pip \
      tar \
      unzip \
      util-linux \
      vim \
      wget \
      zip \
      && echo

# Install Python3 dependencies - could be read in from a requirements.txt file
RUN pip3 install --upgrade pip
RUN pip3 install --upgrade boto3

# If there are any files that should exist in the container then copy them in
#COPY files/ /

# Remove the Yum cache if any to reduce size of the image
RUN yum clean all

# Create a user to run the container as - this prevents the user dropping in a the root user.
RUN useradd -u ${UID} -c "Dev Container user" -m developer

# Create a minimal empty base image now
FROM scratch

# Copy the files from the source image into the empty base image - this is the image that will be used it is effectifly a squash of all of the previous layers
COPY --from=aws-cli-source-image / /

# If there are any common mounts that should be exposed then add them here
#VOLUME ["/home/developer"]

# Do not set an entry point for the container - this allows the user to override the entry point when running the container
#ENTRYPOINT ["/usr/bin/tail", "-f", "/dev/null"]

# Do not set any command to execute - this allows the user to override the command when running the container
#CMD []