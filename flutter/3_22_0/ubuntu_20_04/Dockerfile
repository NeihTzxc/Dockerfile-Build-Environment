# Use a lightweight base image
FROM ubuntu:20.04

# Preconfigure the timezone
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y tzdata

RUN ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    unzip \
    openjdk-17-jdk \
    xz-utils \
    git \
    && rm -rf /var/lib/apt/lists/*

# Set JAVA_HOME
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH

# Download and install Flutter
RUN wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.3-stable.tar.xz \
    && tar -xf flutter_linux_3.22.3-stable.tar.xz \
    && mv flutter /usr/local/flutter \
    && rm flutter_linux_3.22.3-stable.tar.xz
# Add Flutter to PATH
ENV PATH="/usr/local/flutter/bin:$PATH"

# Run Flutter doctor to pre-cache binaries
RUN flutter doctor

# Download Android SDK command line tools
RUN mkdir -p /usr/local/android-sdk/cmdline-tools/tools \
    && wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip \
    && unzip commandlinetools-linux-9477386_latest.zip -d /usr/local/android-sdk/cmdline-tools/tools && mv /usr/local/android-sdk/cmdline-tools/tools/cmdline-tools/* /usr/local/android-sdk/cmdline-tools/tools/ \
    && rm commandlinetools-linux-9477386_latest.zip

# Correct directory structure for sdkmanager
ENV ANDROID_HOME=/usr/local/android-sdk
ENV PATH=$ANDROID_HOME/cmdline-tools/tools/bin:$PATH
ENV PATH=$ANDROID_HOME/platform-tools:$PATH

# Make sure sdkmanager is executable
RUN chmod +x /usr/local/android-sdk/cmdline-tools/tools/bin/sdkmanager

# Install Android SDK components
RUN yes | sdkmanager --licenses \
    && sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# Final setup
WORKDIR /root
RUN flutter doctor