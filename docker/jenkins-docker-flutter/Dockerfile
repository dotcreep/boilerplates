FROM jenkins/jenkins:lts-jdk11

# Set up user as root
USER root

# Prerequisites
RUN apt update && apt install -y tar ca-certificates gnupg
RUN update-ca-certificates -f
RUN apt install -y curl git unzip xz-utils zip clang cmake ninja-build pkg-config libglu1-mesa wget openjdk-17-jdk-headless libjaxb-api-java libjaxb-java sshpass curl wget

# Set up directory
RUN mkdir /home/flutterdev
WORKDIR /home/flutterdev

# Prepare Android Directory & Variable
RUN mkdir -p Android/sdk
ENV ANDROID_SDK_ROOT /home/flutterdev/Android/sdk
RUN mkdir -p .android && touch .android/repositories.cfg

# Set up Android SDK
RUN wget -O sdk-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip
RUN unzip sdk-tools.zip && rm sdk-tools.zip
RUN mv cmdline-tools Android/sdk/cmdline-tools
ENV ANDROID_SDK_ROOT="/home/flutterdev/Android/sdk"
RUN cd Android/sdk/cmdline-tools/bin && yes | ./sdkmanager --sdk_root=$ANDROID_SDK_ROOT --licenses
RUN cd Android/sdk/cmdline-tools/bin && ./sdkmanager --sdk_root=$ANDROID_SDK_ROOT "build-tools;33.0.1" "patcher;v4" "platform-tools" "platforms;android-33" "sources;android-33"
ENV PATH "$PATH:$ANDROID_SDK_ROOT/platform-tools"

# Download Flutter SDK
RUN wget -O flutter.tar.xz https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.7.7-stable.tar.xz
RUN tar -xf flutter.tar.xz && rm flutter.tar.xz
ENV PATH "$PATH:/home/flutterdev/flutter/bin"
RUN git config --global --add safe.directory /home/flutterdev/flutter

# Run basic check to download Dart SDK
RUN flutter doctor
RUN flutter config --no-analytics

# Install Docker
RUN install -m 0755 -d /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN chmod a+r /etc/apt/keyrings/docker.gpg
RUN echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt-get update
RUN apt-get install -y docker-ce-cli docker-compose-plugin