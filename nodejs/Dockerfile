# Use a more recent Debian-based image
FROM debian:bullseye-slim

# Set the working directory inside the container
WORKDIR /usr/src/app

# Install required system dependencies
RUN apt-get update && \
    apt-get install -y \
    curl \
    unzip \
    git \
    git-lfs \
    python2.7 \
    awscli \
    gcc \
    gfortran \
    libnetcdf-dev \
    libnetcdff-dev \
    g++ \
    make \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Set Python 2.7 for node-gyp
RUN ln -s /usr/bin/python2.7 /usr/bin/python

# Install n (Node version manager)
RUN curl -L https://raw.githubusercontent.com/tj/n/master/bin/n -o /usr/local/bin/n && \
    chmod +x /usr/local/bin/n

# Use n to install the desired Node.js version (8.10)
RUN n 8.10.0

# Set the correct Node.js and npm binaries
ENV PATH="/usr/local/n/versions/node/8.10.0/bin:${PATH}"

# Verify the installed Node.js version
RUN node -v
RUN npm -v

# Set the branch to be used for the application
ENV DL_BRANCH="auth"

# Download zip from github
RUN curl -L -o datalakes.zip https://github.com/EPFL-ENAC/datalakes-nodejs/archive/refs/heads/${DL_BRANCH}.zip && \    
    # Unzip the downloaded file
    unzip datalakes.zip && rm datalakes.zip && \
    # Move the unzipped directory to the working directory
    mv datalakes-nodejs-${DL_BRANCH}/* . && rm -rf datalakes-nodejs-${DL_BRANCH}

# Install Node.js dependencies
RUN npm install

# Copy the entrypoint script
RUN cp docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Expose the application port
EXPOSE 4000

# Create directories inside the container to store mounted data (optional)
RUN mkdir -p /usr/src/app/data /usr/src/app/logs

# Use the entrypoint script to run migrations + start server
ENTRYPOINT ["sh", "/usr/local/bin/docker-entrypoint.sh"]
