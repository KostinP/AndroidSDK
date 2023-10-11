ADD file:ff6963f777661fb16cc12fb04a97c558bd94768a6e4ab5bd90e01f3086818853 in /
CMD ["bash"]
LABEL maintainer=thyrlian@gmail.com
RUN /bin/sh -c dpkg --add-architecture i386 && apt-get update -y && apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc1 lib32ncurses6 lib32z1 zlib1g:i386 && apt-get install -y --no-install-recommends openjdk-11-jdk && apt-get install -y --no-install-recommends git wget curl unzip && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends build-essential && apt install -y libncurses5 # buildkit
ARG GRADLE_VERSION=7.2
ARG GRADLE_DIST=bin
RUN |2 GRADLE_VERSION=7.2 GRADLE_DIST=bin /bin/sh -c cd /opt && wget -q https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-${GRADLE_DIST}.zip && unzip gradle*.zip && ls -d */ | sed 's/\/*$//g' | xargs -I{} mv {} gradle && rm gradle*.zip # buildkit
ARG KOTLIN_VERSION=1.3.72
RUN |3 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 /bin/sh -c cd /opt && wget -q https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip && unzip *kotlin*.zip && rm *kotlin*.zip # buildkit
ARG ANDROID_SDK_VERSION=6514223
ENV ANDROID_SDK_ROOT=/opt/android-sdk
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_VERSION}_latest.zip && unzip *tools*linux*.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && rm *tools*linux*.zip # buildkit
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV GRADLE_HOME=/opt/gradle
ENV KOTLIN_HOME=/opt/kotlinc
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/gradle/bin:/opt/kotlinc/bin:/opt/android-sdk/cmdline-tools/tools/bin:/opt/android-sdk/platform-tools:/opt/android-sdk/emulator
ENV _JAVA_OPTIONS=-XX:+UnlockExperimentalVMOptions
ADD license_accepter.sh /opt/ # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c /bin/bash /opt/license_accepter.sh $ANDROID_SDK_ROOT # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c curl -fsSL https://deb.nodesource.com/setup_15.x | bash - && apt-get install -y nodejs && npm i -g yarn # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c yarn --version # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c sdkmanager "emulator" "build-tools;29.0.3" # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c sdkmanager "platforms;android-29" # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c sdkmanager "platforms;android-28" # buildkit
RUN |4 GRADLE_VERSION=7.2 GRADLE_DIST=bin KOTLIN_VERSION=1.3.72 ANDROID_SDK_VERSION=6514223 /bin/sh -c sysctl net.ipv6.conf.all.disable_ipv6=1 && sysctl net.ipv6.conf.default.disable_ipv6=1 # buildkit
ADD entrypoint.sh / # buildkit
ENTRYPOINT ["/entrypoint.sh"]