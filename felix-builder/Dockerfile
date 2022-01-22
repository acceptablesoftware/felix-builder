FROM python:3.10.0-alpine3.14

# Create a non-root user.
RUN adduser -D -u 1000 appuser
WORKDIR /home/appuser

# Do not buffer Python output and send it straight to the terminal.
ENV PYTHONUNBUFFERED=1

ENTRYPOINT /bin/sh
