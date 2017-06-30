# MysteryNumber
Signed APK �����
Google Play Store�� �����غ���.

���� signed release APK�� ������ �Ѵ�.

sining key �����
keytool�� private signing key�� �����.
keytool.exe�� JDK�� ��ġ�� bin ������ ���� (C:\Program Files\Java\jdk1.8.0_121\bin)
�ڹٴ� 'Ű �����'�� ���� ���� �ȿ��� �����ϰ� �ִ�.
���� Ű ����ҿ� ����ִ� Ű�� Ȯ���Ϸ��� keytool -list�� Ȯ���Ѵ�.
keytool ����: java.lang.Exception: Ű ����� ������ �������� ����: ...\.keystore
�ѹ��� Ű�� �������� ������ Ű ����ҵ� ���� Ű�� ����.
�׷� Ű ����Ҵ� ��� ���峪? Ű ����Ҹ� ����� ����� ���� ���ʷ� Ű�� ����� ����ҵ� �ڵ����� �����.
�� �׷� keytool -genkey�� Ű�� ����� ����.

$ keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000

��й�ȣ�� �Է��ϰ� ��� ���͸� �ļ� skip

������ ���� ��ȿ �Ⱓ�� 10,000���� 2,048��Ʈ RSA Ű �� �� ��ü ����� ������(SHA256withRSA)�� �����ϴ� ��
        : CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown
<my-key-alias>�� ���� Ű ��й�ȣ�� �Է��Ͻʽÿ�.
        (Ű ����� ��й�ȣ�� ������ ��� Enter Ű�� ����):

������ ���͸� ġ�� my-release-key.keystore ������ �����ȴ�.
(���� �������� keystore�� ���� 1ȸ������ ���������Ƿ� �ٽ� keytool -list �غ��� ����Ҵ� ����.)

�� keystore ������ 10,000�� ���� ��밡���� single key�� ������ �ִ�.
ailas�� ���߿� ���� signing �Ҷ� ����� �̸��̴� �� ����ص���.

keystore ������ private�ϰ� �����ؾ� ��. version control�� �ø��� ����!

gradle ���� ����
- �� keystore������ android/app ������ ����
- android/gradle.properties ���ϾƷ� ���� �߰��ϰ� ����

MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****

- ��ǥ�� �Ʊ� �Է��� ��й�ȣ
- �� ������ ���߿� �츮 app signing�� ����� global gradle ����
- Play Store�� �ѹ� ������ ���¿��� signing key�� �ٲٰ� �ʹٸ� �ٸ� ��Ű�� �̸����� ������ؾ� ��. (�ٸ� ��Ű������ �Է��Ѵٴ� ���� �ٿ�ε� Ƚ���� ���� ������ ���ư��ٴ� ��!) �׷��� �� keystore ������ �� ������ ��!!

app�� gradle config���� signing config ����
- android/app/build.gradle ���� ����
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...

release APK ����
- android/app ������ react.gradle ������ ���ٸ�
$ mkdir -p android/app/src/main/assets
$ react-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
$ cd android
$ ./gradlew assembleRelease

> ������ assembleRelease���� �ϸ� ���� ������ ������.. android/app/build/outputs/apk/app-release.apk ��� apk ������ �����.

����~!

release APK �׽�Ʈ
��� ���� ���� �׽�Ʈ�� ���ð� �غ��� �� ���̴�.
���� �׽�Ʈ�� apk�� ����� �Ʒ� ��ɾ ��������.

cd android
./gradlew installRelease

���� �� �򸰴�.

����~!

Proguard�� APK ũ�� ���̱� (�ɼ�)
�ɼ��� �������� ���� ��� apk�� 7,479KB�̴�.
Proguard�� APK�� ũ�⸦ ��¦ �ٿ��ش�. (������� �ʴ� �ڵ带 �����Ѵ�.)
Proguard�� ���� library ���� ������ �ʿ��ϴٰ� �Ѵ�. ? ���� �� �𸣰���.
android/app/build.gradle���� �Ʒ� �ɼ��� true�� �����ϸ� �ȴ�.
    buildTypes {
        release {
            ...
            minifyEnabled true
        }
    }


------------FIRE BASE ����

https://medium.com/react-native-development/build-a-chat-app-with-firebase-and-redux-part-1-8a2197fb0f88
https://medium.com/@jamesmarino/getting-started-with-react-native-and-firebase-ab1f396db549