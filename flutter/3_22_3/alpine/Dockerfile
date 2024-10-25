# Use a lightweight Alpine base image
FROM alpine:3.18

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apk add --no-cache \
    ca-certificates \
    curl \
    wget \
    unzip \
    openjdk17 \
    xz \
    git \
    tzdata

# Set timezone
RUN ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime

# Set JAVA_HOME
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV PATH=$JAVA_HOME/bin:$PATH

# Download and install Flutter
RUN wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.3-stable.tar.xz \
    && tar -xf flutter_linux_3.22.3-stable.tar.xz \
    && mv flutter /usr/local/flutter \
    && rm flutter_linux_3.22.3-stable.tar.xz

# Add Flutter to PATH
ENV PATH="/usr/local/flutter/bin:$PATH"

# Download and install Android SDK command line tools
RUN mkdir -p /usr/local/android-sdk/cmdline-tools/tools \
    && wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip \
    && unzip commandlinetools-linux-9477386_latest.zip -d /usr/local/android-sdk/cmdline-tools/tools \
    && mv /usr/local/android-sdk/cmdline-tools/tools/cmdline-tools/* /usr/local/android-sdk/cmdline-tools/tools/ \
    && rm commandlinetools-linux-9477386_latest.zip \
    && chmod +x /usr/local/android-sdk/cmdline-tools/tools/bin/sdkmanager

# Set Android SDK environment variables
ENV ANDROID_HOME=/usr/local/android-sdk
ENV PATH=$ANDROID_HOME/cmdline-tools/tools/bin:$ANDROID_HOME/platform-tools:$PATH

# Install Android SDK components
RUN yes | sdkmanager --licenses \
    && sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# Final setup
WORKDIR /root