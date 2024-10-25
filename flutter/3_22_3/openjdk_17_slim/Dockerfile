# Use an OpenJDK image with Java 17 pre-installed
FROM openjdk:17-slim

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    wget \
    unzip \
    xz-utils \
    git \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Download and install Flutter
RUN wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.3-stable.tar.xz \
    && tar -xf flutter_linux_3.22.3-stable.tar.xz \
    && mv flutter /usr/local/flutter \
    && rm flutter_linux_3.22.3-stable.tar.xz

# Add Flutter to PATH
ENV PATH="/usr/local/flutter/bin:$PATH"

# Run Flutter doctor to pre-cache binaries
RUN flutter doctor

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
    && sdkmanager "platform-tools" \
    "platforms;android-29" "platforms;android-31" "platforms;android-33" "platforms;android-34" \
    "build-tools;30.0.3" "build-tools;34.0.0"

# Final setup
WORKDIR /root