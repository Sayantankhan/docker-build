FROM codercom/code-server:latest

# Switch to root to install system packages
USER root

# Install Python 3, Java 17, Go, Node.js + npm
RUN apt-get update && \
    apt-get install -y python3 python3-pip openjdk-17-jdk golang && \
    curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Switch back to coder user (important!)
USER coder

