# Use the official Docker image as the base
FROM docker:latest

# Install the Go CLI 
RUN apk add --no-cache go
RUN apk add --no-cache nodejs npm
ENV GOROOT /usr/lib/go
ENV GOPATH /go
ENV PATH /go/bin:$PATH

RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin

ARG PAT

# Install necessary packages and configure Git
RUN apk add --no-cache git
RUN git config --global url."https://${PAT}:x-oauth-basic@github.com/".insteadOf "https://github.com/"
RUN git config --global http.sslVerify false

# Copy the Go module file and install the necessary Go package
COPY go.mod ./
RUN go get gofr.dev
RUN rm go.mod

# Install Node.js using NVM
ENV NODE_VERSION=16.13.0
RUN apk add --no-cache curl
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | sh
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"

# Verify Node.js and install Newman
RUN node --version
RUN npm --version
RUN npm install -g newman

