FROM node:20

ENV NODE_VERSION 20

ENV ANDROID_SDK_TOOLS 11076708
ENV ANDROID_SDK_URL https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip
ENV ANDROID_BUILD_TOOLS_VERSION 33.0.0
ENV ANDROID_HOME /home/node/android-sdk-linux
ENV ANDROID_VERSION 33
ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/bin

ENV GRADLE_HOME /home/node/.gradle

# Install OpenJDK
RUN apt-get update && \
    apt-get install -y openjdk-17-jdk

# Install required packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends unzip curl && \
    mkdir "$ANDROID_HOME" .android && \
    cd "$ANDROID_HOME" && \
    curl -o sdk.zip $ANDROID_SDK_URL && \
    unzip sdk.zip && \
    rm sdk.zip && \
# Download Android SDK
    yes | sdkmanager --licenses --sdk_root=$ANDROID_HOME && \
    sdkmanager --update --sdk_root=$ANDROID_HOME && \
    sdkmanager --sdk_root=$ANDROID_HOME \
    "platforms;android-${ANDROID_VERSION}" \
    "platform-tools" && \
# Clean up
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    apt-get autoremove -y && \
    apt-get clean

# EAS CLI
RUN npm install -g eas-cli

# Gradle
RUN mkdir $GRADLE_HOME
COPY ./tooling/gradle/gradle.properties $GRADLE_HOME/gradle.properties

# Permission
RUN chown -R node:node $ANDROID_HOME
RUN chown -R node:node $GRADLE_HOME

USER node

WORKDIR /app