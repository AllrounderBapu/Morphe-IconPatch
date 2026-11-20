## Termux Sdk Install Morphe Patch


pkg update && pkg upgrade

pkg install x11-repo wget unzip kotlin gradle git aapt2 openjdk-21


cd ~
wget https://dl.google.com/android/repository/commandlinetools-linux-15859902_latest.zip
`Use Lastest CommandLineTool for Linux Version`


mkdir AndroidSdk
mkdir latest


unzip commandlinetools-linux-15859902_latest.zip* -d AndroidSdk/

ls AndroidSdk/cmdline-tools

mv AndroidSdk/cmdline-tools/* latest/

mv latest AndroidSdk/cmdline-tools/

ls AndroidSdk/cmdline-tools/latest/bin

export ANDROID_HOME=$(pwd)/AndroidSdk

export JAVA_HOME=/data/data/com.termux/files/usr/lib/jvm/java-21-openjdk

ls $ANDROID_HOME

//use sdkmanager from export path

export PATH=$PATH:$(pwd)/AndroidSdk/cmdline-tools/latest/bin:$(pwd)/AndroidSdk/platform-tools

sdkmanager  || cd ~/AndroidSdk/cmdline-tools/latest/bin/ bash sdkmanager

sdkmanager --update

sdkmanager --list

yes | sdkmanager --licenses
yes | sdkmanager --licenses

sdkmanager "platform-tools" "platforms;android-35"
sdkmanager "build-tools;36.0.0"

rm -rf commandlinetools-linux*

###Termux Storage
termux-setup-storage

##Run This Command Get Github Access Token(Morphe Patch)

mkdir -p ~/.gradle
cat << 'EOF' >> ~/.gradle/gradle.properties
gpr.user=YOUR_GITHUB_USERNAME
gpr.key=ghp_YOUR_TOKEN
EOF


##Create Patch Morphe
cd /storage/emulated/0/Download/BKPS/morphed-pl
bash gradlew buildAndroid --no-daemon


## create local.properties file in project module of your project and add below line:
sdk.dir=/data/data/com.termux/files/home/AndroidSdk

## add below line in gradle.properties file of your app project module
android.aapt2FromMavenOverride=/data/data/com.termux/files/usr/bin/aapt2

### generate unsigned bundle .aab fike
bash gradlew bundleRelease

### generate signed debug apk .apk
 bash gradlew assembleDebug

### generate unsigned release .apk
bash gradlew assembleRelease

### For signing unsigned apk file
apksigner sign --ks your_release_key.jks your_app.apk
OR
apksigner sign --ks your_release_key.keystore your_app.apk
