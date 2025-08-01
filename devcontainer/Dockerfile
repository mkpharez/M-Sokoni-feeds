# Start from a Java image with JDK 17
FROM mcr.microsoft.com/devcontainers/java:17

# Install essential tools
RUN apt-get update && \
    apt-get install -y unzip curl git && \
    rm -rf /var/lib/apt/lists/*

# Install Gradle
ENV GRADLE_VERSION=8.7
RUN curl -fsSL https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip && \
    unzip gradle.zip -d /opt && \
    mv /opt/gradle-${GRADLE_VERSION} /opt/gradle && \
    ln -s /opt/gradle/bin/gradle /usr/bin/gradle && \
    rm gradle.zip

# Install Kotlin
ENV KOTLIN_VERSION=1.9.21
RUN curl -fsSL https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip -o kotlin.zip && \
    unzip kotlin.zip -d /opt && \
    mv /opt/kotlinc /opt/kotlinc-${KOTLIN_VERSION} || true && \
    ln -s /opt/kotlinc-${KOTLIN_VERSION}/bin/kotlinc /usr/bin/kotlinc && \
    ln -s /opt/kotlinc-${KOTLIN_VERSION}/bin/kotlin /usr/bin/kotlin && \
    rm kotlin.zip

# Install Android SDK Command-line tools
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    curl -fsSL https://dl.google.com/android/repository/commandlinetools-linux-10406996_latest.zip -o cmdline-tools.zip && \
    unzip cmdline-tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm cmdline-tools.zip

# Install Android SDK packages
RUN yes | ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager --licenses && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager \
        "platform-tools" \
        "platforms;android-34" \
        "build-tools;34.0.0"

# Set environment variables for Android SDK
ENV ANDROID_HOME=${ANDROID_SDK_ROOT}
ENV PATH=${PATH}:${ANDROID_SDK_ROOT}/platform-tools
