FROM swind/android-emulator-base:latest

# ================================================
#
# Install Android AVD and Emulator
#
# ================================================
ENV ABI=${ABI:-"x86_64"} \
    TARGET=${TARGET:-"android-29"} \
    TAG=${TAG:-"google_apis"} \
    NAME="Docker" \
    ANDROID_LOG_TAGS="e" \
    # Argument `-qemu -vnc :0` is required for VNC and noVNC to work
    # You can still override this variable: just add this argument to the end
    ANDROID_EMULATOR_EXTRA_ARGS="-skin 1080x1920 -memory 2048 -gpu swiftshader_indirect -no-boot-anim -qemu" \
    noVNC="false"

# Install android emulator
RUN mkdir -p $HOME/.android \
 && touch $HOME/.android/repositories.cfg \
 && yes yes | $ANDROID_HOME/tools/bin/sdkmanager --verbose \
        "tools" \
        "platform-tools" \
        "platforms;${TARGET}" \
        "system-images;${TARGET};${TAG};${ABI}"
