# Using official python runtime base image
FROM python:2.7-alpine

# Copy SSL proxy certificates to python server so it can perform pip update
RUN apk update && apk add ca-certificates && rm -rf /var/cache/apk/*
COPY ssl_proxy_root.crt /usr/local/share/ca-certificates/ssl_proxy_root.crt
RUN update-ca-certificates
COPY ssl_proxy_inter.crt /usr/local/share/ca-certificates/ssl_proxy_inter.crt
RUN update-ca-certificates

# Set the application directory
WORKDIR /app

# Install our requirements.txt
ADD requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt

# Copy our code from the current folder to /app inside the container
ADD . /app

# Make port 80 available for links and/or publish
EXPOSE 80

# Define our command to be run when launching the container
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
