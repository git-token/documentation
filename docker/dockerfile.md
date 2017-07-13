#### Dockerfile

```dockerfile
# Example Dockerfile

# Use NodeJS Docker image
FROM node:6.11.0

# Install yarn to quickly install cached dependency files
RUN npm i -g yarn

# Run the following commands inside the gittoken-server directory
WORKDIR /gittoken-server

# Clone the git-token/express-server repository
RUN git clone https://github.com/git-token/express-server.git .

# Install dependencies
RUN yarn install

# Build source files
RUN yarn run build-src

# Start the GiToken Server
ENTRYPOINT yarn run start

# Expose port 1324 for the express application;
# Expose port 1325 for the web socket server.
EXPOSE 1324 1325

```
