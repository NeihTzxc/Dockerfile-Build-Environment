# Base image: Use OpenJDK 11 from Debian as the base
FROM openjdk:17-slim

# Install dependencies for Flutter and Dart using apt-get
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    wget \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*

# Set environment variables for Flutter and Dart paths
ENV FLUTTER_HOME=/opt/flutter
ENV PATH="$FLUTTER_HOME/bin:$FLUTTER_HOME/bin/cache/dart-sdk/bin:$PATH"

# Install Flutter SDK
RUN curl -L https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.3-stable.tar.xz -o flutter.tar.xz \
    && mkdir -p "$FLUTTER_HOME" \
    && tar -xvf flutter.tar.xz -C "$FLUTTER_HOME" --strip-components=1 \
    && rm flutter.tar.xz

# Pre-download Flutter dependencies (this helps avoid issues in CI)
RUN flutter precache

# Accept Flutter's licenses
RUN yes | flutter doctor --android-licenses

# Verify Flutter and Dart installations
RUN flutter doctor

# Install Dart SDK separately for reliability in CI/CD
#RUN apt-get install -y dart

# Install Sonar Scanner manually
RUN wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip \
    && unzip sonar-scanner-cli-4.8.0.2856-linux.zip -d /opt/ \
    && rm sonar-scanner-cli-4.8.0.2856-linux.zip

# Set environment variables for Sonar Scanner
ENV SONAR_SCANNER_HOME=/opt/sonar-scanner-4.8.0.2856-linux
ENV PATH="$SONAR_SCANNER_HOME/bin:$PATH"

# Verify Sonar Scanner installation
RUN sonar-scanner --version

# Define the working directory
WORKDIR /usr/src/app

# Run sonar-scanner by default
CMD ["sonar-scanner"]