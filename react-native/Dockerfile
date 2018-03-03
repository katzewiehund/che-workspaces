FROM ubuntu:latest
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y openjdk-8-jdk git subversion curl unzip xz-utils && \
    apt-get install -y libc6 libstdc++6 zlib1g libncurses5 --no-install-recommends && \
    apt-get clean

# install nodejs
ENV NODE_VERSION=8.9.4
RUN curl -L https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz -o node.tar.xz && \
    tar -xJf node.tar.xz -C /usr/local --strip-components=1 && \
    rm node.tar.xz && \
    ln -s /usr/local/bin/node /usr/local/bin/nodejs
    
# install react-native
RUN npm install -g yarn && npm install -g react-native-cli

# install android
ENV ANDROID_SDK_URL https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip
RUN curl -L "${ANDROID_SDK_URL}" -o /tmp/android-sdk-linux.zip && \
    unzip /tmp/android-sdk-linux.zip -d /usr/local/ && \
    rm /tmp/android-sdk-linux.zip && \
    mkdir /usr/local/android-sdk-linux && \
    mv /usr/local/tools /usr/local/android-sdk-linux/
ENV ANDROID_HOME=/usr/local/android-sdk-linux ANDROID_SDK=/usr/local/android-sdk-linux
ENV PATH ${ANDROID_HOME}/tools:$ANDROID_HOME/platform-tools:$PATH

# install android build tools
RUN ${ANDROID_SDK}/tools/bin/sdkmanager "platform-tools" "build-tools;27.0.3" "add-ons;addon-google_apis-google-24" "emulator" "extras;google;google_play_services" "ndk-bundle" "platforms;android-27"
RUN yes | ${ANDROID_SDK}/tools/bin/sdkmanager --licenses
RUN ${ANDROID_SDK}/tools/bin/sdkmanager "platform-tools" "build-tools;27.0.3" "add-ons;addon-google_apis-google-24" "emulator" "extras;google;google_play_services" "ndk-bundle" "platforms;android-27"

# Support Gradle
ENV TERM dumb
    
EXPOSE 3000 5000 8000 9000
# non-terminating command for CHE
CMD tail -f /dev/null
# container needs to run as root for CHE