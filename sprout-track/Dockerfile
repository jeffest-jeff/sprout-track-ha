ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest

# Pull the latest Sprout Track release from Docker Hub
FROM sprouttrack/sprout-track:latest AS app

# HA base image — provides bashio, s6-overlay, and Alpine tooling
FROM ${BUILD_FROM}

RUN apk add --no-cache nodejs npm openssl tzdata

WORKDIR /app

# Copy the full built application from the upstream image
COPY --from=app /app ./

COPY run.sh /run.sh
RUN sed -i 's/\r$//' /run.sh && chmod a+x /run.sh

EXPOSE 3333
CMD ["/run.sh"]
