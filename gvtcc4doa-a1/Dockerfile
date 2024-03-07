# Base image
FROM ubuntu:latest

# Install dependencies
RUN apt-get update && \
    apt-get install -y curl unzip

# Install Terraform
RUN curl -LO https://releases.hashicorp.com/terraform/1.5.0/terraform_1.5.0_linux_amd64.zip && \
    unzip terraform_1.5.0_linux_amd64.zip && \
    mv terraform /usr/local/bin && \
    rm terraform_1.5.0_linux_amd64.zip

# Install Terragrunt
RUN curl -LO https://github.com/gruntwork-io/terragrunt/releases/download/v0.34.1/terragrunt_linux_amd64 && \
    chmod +x terragrunt_linux_amd64 && \
    mv terragrunt_linux_amd64 /usr/local/bin/terragrunt

# Install AWS CLI
RUN apt-get install -y python3-pip && \
    pip3 install awscli

# Set the working directory
WORKDIR /app

# Copy your application files if needed
# COPY . /app

# Define the entry point
# ENTRYPOINT ["your-entrypoint-command"]

# Define the default command
# CMD ["your-default-command"]
