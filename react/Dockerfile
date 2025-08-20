FROM node:22-bullseye AS build-stage

# Set the branch to be used for the application
ENV DL_BRANCH="auth"

RUN mkdir /app
WORKDIR /app

# Download zip from github
RUN curl -L -o datalakes.zip https://github.com/EPFL-ENAC/datalakes-react/archive/refs/heads/${DL_BRANCH}.zip && \    
    # Unzip the downloaded file
    unzip datalakes.zip && rm datalakes.zip && \
    # Move the unzipped directory to the working directory
    mv datalakes-react-${DL_BRANCH}/* . && rm -rf datalakes-react-${DL_BRANCH}

COPY ./src/config.json ./src/config.json

# Install Node.js dependencies
RUN npm install
RUN NODE_OPTIONS=--openssl-legacy-provider npm run build

FROM nginx:stable-alpine AS production-stage
RUN mkdir /app
COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build-stage /app/build  /app
EXPOSE 80

COPY entrypoint.sh /app/entrypoint.sh
ENTRYPOINT ["/app/entrypoint.sh"]
