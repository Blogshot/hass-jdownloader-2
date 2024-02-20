#
# jdownloader-2 Dockerfile
#
# https://github.com/jlesage/docker-jdownloader-2
#
# NOTES:
#   - We are using JRE version 8 because recent versions are much bigger.
#   - JRE for ARM 32-bits on Alpine is very hard to get:
#     - The version in Alpine repo is very, very slow.
#     - The glibc version doesn't work well on Alpine with a compatibility
#       layer (gcompat or libc6-compat).  The `__xstat` symbol is missing and
#       implementing a wrapper is not straight-forward because the `struct stat`
#       is not constant across architectures (32/64 bits) and glibc/musl.
#

ARG BUILD_FROM="alpine:latest"
FROM $BUILD_FROM

# Define software download URLs.
ARG JDOWNLOADER_URL=http://installer.jdownloader.org/JDownloader.jar

# Download JDownloader2
ARG JDOWNLOADER_URL
RUN \
    apk --no-cache add curl && \
    mkdir -p /defaults

RUN curl -L -o /defaults/JDownloader.jar ${JDOWNLOADER_URL}

# Define working directory.
WORKDIR /tmp

# Install dependencies.
RUN \
    apk add \
        java-common \
        openjdk8-jre \
        # Needed by the init script.
        jq \
        # We need a font.
        ttf-dejavu \
        # For ffmpeg and ffprobe tools.
        ffmpeg \
        # For rtmpdump tool.
        rtmpdump \
        # Need for the sponge tool.
        moreutils

# Generate and install favicons.
#RUN \
#    APP_ICON_URL=https://raw.githubusercontent.com/jlesage/docker-templates/master/jlesage/images/jdownloader-2-icon.png && \
#    install_app_icon.sh "$APP_ICON_URL"

# Add files.
COPY rootfs/ /


# Set public environment variables.
ENV \
    MYJDOWNLOADER_EMAIL= \
    MYJDOWNLOADER_PASSWORD= \
    MYJDOWNLOADER_DEVICE_NAME= \
    JDOWNLOADER_HEADLESS=0 \
    JDOWNLOADER_MAX_MEM=

# Expose ports.
#   - 3129: For MyJDownloader in Direct Connection mode.
#EXPOSE 3129

CMD [ "/startapp.sh" ]