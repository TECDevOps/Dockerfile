# Dockerfile
FROM – Options for the base image include Redis, MySQL, Ubuntu, etc.
LABEL – such as EMAIL, AUTHOR, etc.
RUN – This command instructs the container what to perform after it has been created from an image. For instance, rm -fr, apk add — update-redis. Use the cp command in the RUN command to run a file that is inside the container externally if necessary. RUN apk del tzdata && cp /usr/share/zoneinfo/Asia/Colombo /etc/localtime && echo “Asia/Colombo”
COPY – Copy the host system’s files. dest: container destination path src: source path
ADD – This command is similar to COPY in that it downloads tar, zip, or web files, extracts them, and then copies them inside of our image.
WORKDIR – used to provide the directory in which we will work. When adding files from the host’s local machine and saving them in the container, the default directory is the working directory path.
ENTRYPOINT – The command that runs inside the container is called the entrypoint. as in the bash shell in Ubuntu, the server running command in the `httpd` container

## Dockerfile-Java
FROM java:8
COPY   ./var/www/java
WORKDIR   /var/www/
RUN   javac Hello.java  
CMD   ["java",   "Hello"]

## Dockerfile-python

FROM python:3.8-slim-buster
ENV PYTHONUNBUFFERED=1
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD [ "python", "-m" , "django", "run", "--host=0.0.0.0", "--port=5000"]

## Dockerfile-go

# Build Stage
# First pull Golang image
FROM golang:1.17-alpine as build-env
 
# Set environment variable
ENV APP_NAME sample-dockerize-app
ENV CMD_PATH main.go
 
# Copy application data into image
COPY . $GOPATH/src/$APP_NAME
WORKDIR $GOPATH/src/$APP_NAME
 
# Build application
RUN CGO_ENABLED=0 go build -v -o /$APP_NAME $GOPATH/src/$APP_NAME/$CMD_PATH
 
# Run Stage
FROM alpine:3.14
 
# Set environment variable
ENV APP_NAME sample-dockerize-app
 
# Copy only required data into this image
COPY --from=build-env /$APP_NAME .
 
# Expose application port
EXPOSE 8081
 
# Start app



