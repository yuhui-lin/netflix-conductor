#
# conductor:ui - Netflix conductor UI
#
FROM node:alpine
# MAINTAINER Netflix OSS <conductor@netflix.com>

# Install the required packages for the node build
# to run on alpine
RUN apk update && apk add \
  autoconf \
  automake \
  libtool \
  build-base \
  libstdc++ \
  gcc \
  abuild \
  binutils \
  nasm \
  libpng \
  libpng-dev \
  libjpeg-turbo \
  libjpeg-turbo-dev \
  python

# Make app folders
# RUN mkdir -p /app

ENV CONDUCTOR_VERSION 1.10.4

# Get and install conductor
RUN wget -O /conductor.tar.gz https://github.com/Netflix/conductor/archive/v${CONDUCTOR_VERSION}.tar.gz && \
    tar -xf /conductor.tar.gz -C / && \
    mv /conductor-${CONDUCTOR_VERSION}/docker/ui/bin /app && \
    mv /conductor-${CONDUCTOR_VERSION}/ui /app/ui && \
    rm -rf /conductor*

# Copy the ui files onto the image
# COPY ./docker/ui/bin /app
# COPY ./ui /app/ui

# Copy the files for the server into the app folders
RUN chmod +x /app/startup.sh

# Get and install conductor UI
RUN cd /app/ui \
  && npm install \
  && npm run build --server

EXPOSE 5000

CMD [ "/app/startup.sh" ]
ENTRYPOINT ["/bin/sh"]
