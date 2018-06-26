#
# conductor:server - Netflix conductor server
#
FROM java:8-jdk AS builder

# MAINTAINER Netflix OSS <conductor@netflix.com>

ENV CONDUCTOR_VERSION 1.10.4

# Get and install conductor
RUN wget -O conductor.tar.gz https://github.com/Netflix/conductor/archive/v${CONDUCTOR_VERSION}.tar.gz && \
    tar -xf conductor.tar.gz -C / && \
    mv /conductor-${CONDUCTOR_VERSION} /conductor

# Copy the project directly onto the image
# COPY . /conductor
WORKDIR /conductor

# Build the server
RUN ./gradlew build -x test




FROM java:8-jre-alpine AS prod

# MAINTAINER Netflix OSS <conductor@netflix.com>

# Make app folders
RUN mkdir -p /app/config /app/logs /app/libs

# Copy the project directly onto the image
COPY --from=builder /conductor/docker/server/bin /app
COPY --from=builder /conductor/docker/server/config /app/config
COPY --from=builder /conductor/server/build/libs/conductor-server-*-all.jar /app/libs

# Copy the files for the server into the app folders
RUN chmod +x /app/startup.sh

EXPOSE 8080

CMD [ "/app/startup.sh" ]
ENTRYPOINT [ "/bin/sh"]
