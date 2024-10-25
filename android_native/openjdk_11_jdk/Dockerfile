FROM openjdk:11-jdk

# Set environment variables
ENV ANDROID_COMPILE_SDK=33
ENV ANDROID_SDK_TOOLS=8092744
ENV ANDROID_BUILD_TOOLS=33.0.0
ENV ANDROID_HOME=/opt/android-sdk-linux
ENV PATH="$PATH:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools"

# Update package list and install dependencies
RUN apt-get update -qqy && \
    apt-get install -qqy --no-install-recommends \
      wget \
      unzip \
      lib32stdc++6 \
      lib32z1 \
      rsync \
      openssh-server \
      curl \
      bash && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download and install Android SDK
RUN mkdir "$ANDROID_HOME" && \
    cd "$ANDROID_HOME" && \
    wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip -O android-sdk.zip && \
    unzip -q android-sdk.zip && \
    rm android-sdk.zip

# Install Android SDK components
RUN yes | $ANDROID_HOME/cmdline-tools/bin/sdkmanager --sdk_root=$ANDROID_HOME --licenses && \
    yes | $ANDROID_HOME/cmdline-tools/bin/sdkmanager --sdk_root=$ANDROID_HOME "platforms;android-${ANDROID_COMPILE_SDK}" "build-tools;${ANDROID_BUILD_TOOLS}"

# Set up Gradle user home
RUN mkdir /root/.gradle
ENV GRADLE_USER_HOME=/root/.gradle

# Set working directory to /app
WORKDIR /app
