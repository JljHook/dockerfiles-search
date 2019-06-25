FROM circleci/android:api-25-alpha

USER root

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -

RUN apt-get install nodejs && sudo npm install -g n && sudo n 8.5.0 && sudo npm install --global npm@6.9.0

RUN apt-get install build-essential make nano

RUN npm install -g react-native-cli

USER circleci

RUN sdkmanager --update && yes | sdkmanager --licenses

RUN sdkmanager "build-tools;23.0.1"

RUN sdkmanager "platforms;android-23"

RUN sdkmanager "platforms;android-26" "build-tools;26.0.2" "build-tools;28.0.3"

USER root

RUN chown -R circleci:circleci /home/circleci/.npm

USER circleci
