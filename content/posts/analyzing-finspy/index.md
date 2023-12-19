---
draft: true
---

# FinSpy



Sample: https://bazaar.abuse.ch/sample/854774a198db490a1ae9f06d5da5fe6a1f683bf3d7186e56776516f982d41ad3/

Download: https://bazaar.abuse.ch/download/9d82149e3d0647433bbe/

SHA-256: 854774a198db490a1ae9f06d5da5fe6a1f683bf3d7186e56776516f982d41ad3

Filescan: https://www.filescan.io/uploads/644b94da92903068e34f46ec/reports/85d56d44-d60c-479f-bbcf-7a161f8b7e31/overview

VirusTotal: https://www.virustotal.com/gui/file/854774a198db490a1ae9f06d5da5fe6a1f683bf3d7186e56776516f982d41ad3

## AndroidManifest.xml



As always, let's analyze the `AndroidManifest.xml` file.

The full contents of the file is included for reference. Some parts will be further explained below.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" android:compileSdkVersion="23" android:compileSdkVersionCodename="6.0-2438415" package="org.xmlpush.v3" platformBuildVersionCode="23" platformBuildVersionName="6.0-2438415">
    <uses-sdk android:minSdkVersion="19" android:targetSdkVersion="22"/>
    <uses-feature android:name="android.hardware.camera"/>
    <uses-feature android:name="android.hardware.camera.autofocus"/>
    <uses-permission android:name="android.permission.PACKAGE_USAGE_STATS"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
    <uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS"/>
    <uses-permission android:name="android.permission.READ_CALENDAR"/>
    <uses-permission android:name="android.permission.READ_CALL_LOG"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.READ_SMS"/>
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
    <uses-permission android:name="android.permission.RECEIVE_SMS"/>
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>
    <uses-permission android:name="android.permission.SEND_SMS"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
    <uses-permission android:name="android.permission.WRITE_CONTACTS"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_SETTINGS"/>
    <uses-permission android:name="android.permission.WRITE_SMS"/>
    <meta-data android:name="android.support.VERSION" android:value="25.3.1"/>
    <application android:label="@string/app_name" android:icon="@drawable/ic_launcher" android:allowBackup="false" android:hardwareAccelerated="false">
        <activity android:theme="@style/Theme.Transparent" android:name="org.xmlpush.v3.StartVersion">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        <service android:name="org.xmlpush.v3.Services" android:exported="true"/>
        <service android:name="org.xmlpush.v3.schedule.SchedulerServices"/>
        <service android:name="org.xmlpush.v3.eventbased.ReceiverService"/>
        <service android:name="org.xmlpush.v3.EventBasedService"/>
        <service android:name="org.xmlpush.v3.AlarmManager"/>
        <receiver android:name="org.xmlpush.v3.ReceiverMain">
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
                <action android:name="android.net.wifi.STATE_CHANGE"/>
                <action android:name="android.intent.action.AIRPLANE_MODE"/>
                <action android:name="android.intent.action.BATTERY_LOW"/>
                <action android:name="android.intent.action.BATTERY_OKAY"/>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="android.intent.action.DEVICE_STORAGE_LOW"/>
                <action android:name="android.intent.action.DEVICE_STORAGE_OK"/>
                <action android:name="android.intent.action.NEW_OUTGOING_CALL"/>
                <action android:name="android.intent.action.PHONE_STATE"/>
                <action android:name="android.intent.action.USER_PRESENT"/>
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_ADDED"/>
                <action android:name="android.intent.action.PACKAGE_REPLACED"/>
                <data android:scheme="package"/>
            </intent-filter>
            <intent-filter android:priority="100">
                <action android:name="android.intent.action.DATA_SMS_RECEIVED"/>
                <data android:scheme="sms" android:host="*" android:port="8095"/>
            </intent-filter>
        </receiver>
    </application>
</manifest>
```



### Application

```xml
<application android:label="@string/app_name" android:icon="@drawable/ic_launcher" android:allowBackup="false" android:hardwareAccelerated="false">
	... snip ...
</application>
```

First things first, we start from `<application>` tag.

In the above XML file references to `@string` refer to `res/values/strings.xml`. Therefore the application's name is: `@string/app_name` => `wifi`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">wifi</string>
</resources>
```

The `android:name` attribute defines the fully qualified name of an `Application` subclass implemented for the application.  When the application process is started, this class is instantiated before any of the application's components.

The icon of the application is:

![image-20230426225044751](/home/ishtar/github/xmpf.github.io/content/posts/analyzing-finspy/assets/image-20230426225044751.png)



**Main activity**

```xml

<activity android:theme="@style/Theme.Transparent" android:name="org.xmlpush.v3.StartVersion">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
```

The class `org.xmlpush.v3.StartVersion` is defined as the "main" activity for this Android application.

Looking at the decompiled output from JADX-GUI, it is evident that the application is heavily obfuscated:

![image-20230426220307648](/home/ishtar/github/xmpf.github.io/content/posts/analyzing-finspy/assets/image-20230426220307648.png)



**Services and Broadcast Receivers**

```xml
<service android:name="org.xmlpush.v3.Services" android:exported="true"/>
<service android:name="org.xmlpush.v3.schedule.SchedulerServices"/>
<service android:name="org.xmlpush.v3.eventbased.ReceiverService"/>
<service android:name="org.xmlpush.v3.EventBasedService"/>
<service android:name="org.xmlpush.v3.AlarmManager"/>
```

The `android:exported="true"` attribute means that this service can be accessed by other applications on the device.

`BroadcastReceiver` components are used to listen for and respond to system-wide or application specific events:

```xml
<receiver android:name="org.xmlpush.v3.ReceiverMain">
<intent-filter>
    <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
    <action android:name="android.net.wifi.STATE_CHANGE"/>
    <action android:name="android.intent.action.AIRPLANE_MODE"/>
    <action android:name="android.intent.action.BATTERY_LOW"/>
    <action android:name="android.intent.action.BATTERY_OKAY"/>
    <action android:name="android.intent.action.BOOT_COMPLETED"/>
    <action android:name="android.intent.action.DEVICE_STORAGE_LOW"/>
    <action android:name="android.intent.action.DEVICE_STORAGE_OK"/>
    <action android:name="android.intent.action.NEW_OUTGOING_CALL"/>
    <action android:name="android.intent.action.PHONE_STATE"/>
    <action android:name="android.intent.action.USER_PRESENT"/>
</intent-filter>
```

In this case, `org.xmlpush.v3.ReceiverMain` will be triggered after the system receives any of the following events:

1. `android.net.conn.CONNECTIVITY_CHANGE`: Triggered when the device's network connectivity changes, such as when switching between Wi-Fi and mobile data.
2. `android.net.wifi.STATE_CHANGE`: Triggered when the Wi-Fi state changes, such as when it is enabled, disabled, or connected to a network.
3. `android.intent.action.AIRPLANE_MODE`: Triggered when the device's airplane mode setting is changed.
4. `android.intent.action.BATTERY_LOW`: Triggered when the device's battery level is low.
5. `android.intent.action.BATTERY_OKAY`: Triggered when the device's battery level returns to an acceptable level after being low.
6. `android.intent.action.BOOT_COMPLETED`: Triggered when the device has finished booting up.
7. `android.intent.action.DEVICE_STORAGE_LOW`: Triggered when the device's internal storage is running low.
8. `android.intent.action.DEVICE_STORAGE_OK`: Triggered when the device's internal storage has returned to an acceptable level after being low.
9. `android.intent.action.NEW_OUTGOING_CALL`: Triggered when a new outgoing call is initiated.
10. `android.intent.action.PHONE_STATE`: Triggered when the phone's state changes, such as when a call is incoming, outgoing, or ended.
11. `android.intent.action.USER_PRESENT`: Triggered when the user is present, such as when the device is unlocked.



It is crucial for a Spyware to observe the above system generated events for tracking purposes. For example, `android.intent.action.PHONE_STATE` will allow a malicious application to collect information regarding user's communication while `android.intent.action.USER_PRESENT` might be used either for surveillance (take pictures while the user holds the phone) or for "evasion" purposes (perform actions when the user is less likely to notice).



For further information, consult [^1] or [^2].



**Command and Control**

Finally, it looks like SMS is used for communication with the C2 server:

```xml
<intent-filter android:priority="100">
    <action android:name="android.intent.action.DATA_SMS_RECEIVED"/>
    <data android:scheme="sms" android:host="*" android:port="8095"/>
</intent-filter>
```

Using a specific SMS port allows the application to filter out other regular SMS messages and focus on messages that are intended for (most probably) its command and  control purposes.



The `android:priority="100"` attribute specifies the priority of the `BroadcastReceiver` for this intent filter. The application wants to make sure that the `org.xmlpush.v3.ReceiverMain` receiver will be the first to be notified when an `android.intent.action.DATA_SMS_RECEIVED` event is triggered.



## org.xmlpush.p011v3.ReceiverMain



```java
public void onReceive(Context context, Intent intent) {
        boolean z;
        boolean z2 = false;
        NOP();
        Intent intent2 = new Intent(context, ReceiverService.class);
        NOP();
        intent2.putExtras(intent);
        NOP();
        String action = intent.getAction();
        NOP();
        intent2.setAction(action);
        NOP();
        context.startService(intent2);
        String PHONE_STATE = PHONE_STATE();
        NOP();
        String action2 = intent.getAction();
        NOP();
        if (!PHONE_STATE.equalsIgnoreCase(action2)) {
            String NEW_OUTGOING_CALL = NEW_OUTGOING_CALL();
            NOP();
            String action3 = intent.getAction();
            NOP();
            if (!NEW_OUTGOING_CALL.equalsIgnoreCase(action3)) {
                z = false;
                this.f1019a = z;
                if (!C0558k.f1501cg || C0558k.f1437bU) {
                    z2 = true;
                }
                this.f1020b = z2;
                if (this.f1019a || !this.f1020b) {
                }
                NOP();
                C0485a m482a = C0485a.m482a(context);
                TelephonyManager telephonyManager = C0558k.telephonyManager;
                NOP();
                telephonyManager.listen(m482a, 32);
                return;
            }
        }
        z = true;
        this.f1019a = z;
        if (!C0558k.f1501cg) {
        }
        z2 = true;
        this.f1020b = z2;
        if (this.f1019a) {
        }
}
```



## org.xmlpush.p011v3.eventbased.ReceiverService



This is the most important class in this sample. It implements the Broadcast Receiver.

An `IntentService` [^10] is a subclass of `Service` in Android, designed to handle asynchronous tasks in the background without affecting the main thread.

```java
/* renamed from: org.xmlpush.v3.eventbased.ReceiverService */
/* loaded from: classes.dex */
public class ReceiverService extends IntentService {

    /* renamed from: a */
    Context context;

    /* renamed from: b */
    WifiManager wifiManager;

    /* renamed from: c */
    WifiManager.WifiLock wifiLock;

    /* renamed from: d */
    PowerManager powerManager;

    /* renamed from: e */
    PowerManager.WakeLock wakeLock;
    
    ... snip ...
}
```



> **This class was deprecated in API level 30.**
>     IntentService is subject to all the   [background execution limits](https://developer.android.com/about/versions/oreo/background)   imposed with Android 8.0 (API level 26). Consider using `WorkManager`   instead.



```java
public void onCreate() {
    NOP();
    super.onCreate();
    NOP();
    Context applicationContext = getApplicationContext();
    String str_wifi = str_wifi();
    NOP();
    this.wifiManager = (WifiManager) applicationContext.getSystemService(str_wifi);
    WifiManager wifiManager = this.wifiManager;
    String MyWifiLock = MyWifiLock();
    NOP();
    this.wifiLock = wifiManager.createWifiLock(1, MyWifiLock); // WifiManager.WIFI_MODE_FULL
    String str_power = str_power();
    NOP();
    this.powerManager = (PowerManager) getSystemService(str_power);
    PowerManager powerManager = this.powerManager;
    String str_My_Lock = str_My_Lock();
    NOP();
    this.wakeLock = powerManager.newWakeLock(1, str_My_Lock); // PowerManager.PARTIAL_WAKE_LOCK
}
```



A `WifiLock` in Android is a mechanism that allows an application to keep the Wi-Fi radio awake and maintain a Wi-Fi connection even when the device is idle or the screen is off. This can be useful in situations where the application needs a persistent Wi-Fi connection to perform tasks, such as downloading large files, streaming media, or maintaining a connection to a remote server.



https://cs.android.com/android/platform/superproject/+/master:packages/modules/Wifi/framework/java/android/net/wifi/WifiManager.java

```java
/**
 * In this Wi-Fi lock mode, Wi-Fi will be kept active,
 * and will behave normally, i.e., it will attempt to automatically
 * establish a connection to a remembered access point that is
 * within range, and will do periodic scans if there are remembered
 * access points but none are in range.
 */
public static final int WIFI_MODE_FULL = 1;
```





## Decrypted Strings

Using the code snippet below we can retrieve the decrypted strings:

```java
import java.util.ArrayList;

public class MyClass {
    
    private static byte[] xorDecrypt(byte[] bArr, byte[] bArr2) {
            byte[] bArr3 = new byte[bArr2.length];
            for (int i = 0; i < bArr2.length; i++) {
                bArr3[i] = (byte) (bArr2[i] ^ bArr[i % bArr.length]);
            }
            return bArr3;
        }
    
    private static String decryptWith(int i) {
            byte[] bArr = {48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 97, 98, 99, 100, 101, 102}; // 0 .. F
            byte[] bArr2 = {102, 101, 100, 99, 98, 97, 57, 56, 55, 54, 53, 52, 51, 50, 49, 48}; // F .. 0
            ArrayList arrayList = new ArrayList();
            arrayList.add(new byte[]{83, 94, 95, 29, 93, 91, 66, 82, 86, 77, 79, 46, 44, 39, 36, 50, 121, 126, 124, 108, 119, 125, 119, 121, Byte.MAX_VALUE, 124});
            arrayList.add(new byte[]{1, 21, 23});
            arrayList.add(new byte[]{94, 84, 70, 68, 91, 71, 93});
            arrayList.add(new byte[]{52, 32, 41, 44, 52, 32, 117});
            arrayList.add(new byte[]{81, 93, 83, 65, 89});
            arrayList.add(new byte[]{17, 12, 2, 10});
            arrayList.add(new byte[]{10});
            return new String(xorDecrypt(i % 2 == 0 ? bArr : bArr2, (byte[]) arrayList.get(i)));
    }
    
    public static void main(String[] args) {
        for (int i = 0; i <= 6; i++) {
            System.out.println(i + ": " + decryptWith(i));
        }
    }
}
```

- 0: `com.intent.LOCATION_CHANGE`
- 1: `gps`
- 2: `network`
- 3: `REMOVAL`
- 4: `alarm`
- 5: `wifi`
- 6: `:`



And 

```java
import java.util.ArrayList;

public class MyClass {
    
    private static byte[] xorDecrypt2(byte[] bArr, byte[] bArr2) {
        byte[] bArr3 = new byte[bArr2.length];
        for (int i = 0; i < bArr2.length; i++) {
            bArr3[i] = (byte) (bArr2[i] ^ bArr[i % bArr.length]);
        }
        return bArr3;
    }
    
    private static String decryptWith2(int i) {
        byte[] bArr = {48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 97, 98, 99, 100, 101, 102};
        byte[] bArr2 = {102, 101, 100, 99, 98, 97, 57, 56, 55, 54, 53, 52, 51, 50, 49, 48};
        ArrayList arrayList = new ArrayList();
        arrayList.add(new byte[]{31, 66, 75, 64, 64, 80, 91, 24, 93, 77, 2, 77, 27, 22, 0, 4, 69, 88, 94, 87, 26, 70, 94});
        arrayList.add(new byte[]{73, 22, 29, 16, 22, 4, 84, 23, 85, 95, 91, 27, 68, 83, 69, 83, 14, 1, 11, 4, 6});
        arrayList.add(new byte[]{90, 65, 28, 93, 85, 67, 83, 69, 22, 85, 8, 12, 6, 74, 4, 8, 84, 67, 93, 90, 80});
        arrayList.add(new byte[]{5, 10, 9, 77, 20, 8, 91, 93, 69, 24, 67, 91, 90, 66});
        arrayList.add(new byte[]{83, 94, 95, 29, 82, 84, 85, 82, 90, 86, 14, 9, 77, 11, 23, 5, 81});
        arrayList.add(new byte[]{5, 10, 9, 77, 17, 10, 64, 72, 82, 24, 71, 85, 90, 86, 84, 66});
        arrayList.add(new byte[]{83, 94, 95, 29, 71, 94, 79, 71, 93, 23, 19, 3, 10, 0, 0, 20});
        arrayList.add(new byte[]{5, 10, 9, 77, 4, 20, 77, 77, 69, 83, 87, 93, 71, 65, 31, 89, 8, 22, 16, 2, 15, 4, 74, 75, 86, 81, 80, 26, 85, 64, 84, 85});
        arrayList.add(new byte[]{83, 94, 95, 29, 86, 87, 91});
        arrayList.add(new byte[]{9, 23, 3, 77, 22, 4, 85, 93, 80, 68, 84, 89, 29, 95, 84, 67, 21, 0, 10, 4, 7, 19});
        arrayList.add(new byte[]{83, 89, 28, 71, 92, 71, 83, 82, 85, 88, 79, 3, 19, 20});
        arrayList.add(new byte[]{5, 10, 9, 77, 21, 9, 88, 76, 68, 87, 69, 68});
        arrayList.add(new byte[]{21, 1, 10, 107});
        arrayList.add(new byte[]{5, 4, 8, 37, 11, 13, 92});
        arrayList.add(new byte[]{83, 94, 92, 71, 114, 92, 90, 82});
        arrayList.add(new byte[]{5, 10, 10, 23, 7, 15, 77, 2, 24, 25, 92, 87, 80, 29, 80, 84, 8});
        arrayList.add(new byte[]{67, 92, 116, 90, 88, 80});
        arrayList.add(new byte[]{5, 10, 10, 23, 7, 15, 77, 2, 24, 25, 70, 89, 64});
        arrayList.add(new byte[]{83, 80, 94, 85, 93, 89, 83});
        arrayList.add(new byte[]{5, 4, 8, 37, 11, 13, 92});
        arrayList.add(new byte[]{83, 94, 92, 71, 114, 92, 90, 82});
        arrayList.add(new byte[]{21, 8, 34, 10, 14, 4});
        arrayList.add(new byte[]{83, 80, 94, 85, 93, 89, 83});
        arrayList.add(new byte[]{31, 28, 29, 26, 79, 44, 116, 21, 83, 82, 21, 124, 123, 8, 92, 93, 92, 22, 23});
        arrayList.add(new byte[]{81, 93, 83, 65, 89});
        arrayList.add(new byte[]{2, 16, 22, 2, 22, 8, 86, 86});
        arrayList.add(new byte[]{93, 94, 86, 70, 88, 80});
        arrayList.add(new byte[]{21, 17, 11, 19, 54, 8, 84, 93});
        arrayList.add(new byte[]{66, 92, 91, 95, 1, 80});
        arrayList.add(new byte[]{14, 0, 8, 19, 7, 19});
        arrayList.add(new byte[]{81, 95, 86, 65, 91, 92, 82, 25, 81, 87, 21, 7, 13, 16, 75, 7, 83, 69, 91, 92, 90, 27, 101, 116, 106, 124, 36, 44, 60, 43, 43});
        arrayList.add(new byte[]{7, 11, 0, 17, 13, 8, 93, 22, 94, 88, 65, 81, 93, 70, 31, 81, 5, 17, 13, 12, 12, 79, 106, 123, 101, 115, 112, 122, 108, 125, 119, 118});
        arrayList.add(new byte[]{66, 92, 91, 95, 1, 80});
        arrayList.add(new byte[]{43, 32, 55, 48, 39, 47, 126, 125, 101, 105, 124, 122, 96, 102, 112, 124, 42, 32, 32});
        arrayList.add(new byte[]{64, 80, 81, 88, 85, 82, 83, 121, 89, 84, 4});
        return new String(xorDecrypt2(i % 2 == 0 ? bArr : bArr2, (byte[]) arrayList.get(i)));
    }
    
    public static void main(String[] args) {
        for (int i = 0; i < 35; i++) {
            System.out.println(i + ": " + decryptWith2(i));
        }
    }
}
```



- 0: `/system/etc/xrebuild.sh`
- 1: `/system/bin/watchdogd`
- 2: `jp.naver.line.android`
- 3: `com.viber.voip`
- 4: `com.facebook.orca`
- 5: `com.skype.raider`
- 6: `com.skype.raider`
- 7: `com.futurebits.instamessage.free`
- 8: `com.bbm`
- 9: `org.telegram.messenger`
- 10: `ch.threema.app`
- 11: `com.whatsapp`
- 12: `%08X`
- 13: `calFile`
- 14: `contFile`
- 15: `content://icc/adn`
- 16: `smFile`
- 17: `content://sms`
- 18: `calfile`
- 19: `calFile`
- 20: `contFile`
- 21: `smFile`
- 22: `calfile`
- 23: `yyyy-MM-dd HH:mm:ss`
- 24: `alarm`
- 25: `duration`
- 26: `module`
- 27: `stopTime`
- 28: `rmil5e`
- 29: `helper`
- 30: `android.intent.action.SCREEN_ON`
- 31: `android.intent.action.SCREEN_OFF`
- 32: `rmil5e`
- 33: `MESSENGER_INSTALLED`
- 34: `packageName`



... missing content ...



> Note: I used https://www.jdoodle.com/online-java-compiler-ide/ to run the Java snippets above and decrypt the strings.



`org.xmlpush.p011v3.Init` 



```java
private static String decryptWith4(int i) {
    byte[] bArr = {48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 97, 98, 99, 100, 101, 102};
    byte[] bArr2 = {102, 101, 100, 99, 98, 97, 57, 56, 55, 54, 53, 52, 51, 50, 49, 48};
    ArrayList arrayList = new ArrayList();
    arrayList.add(new byte[]{114, 94, 93, 71, 119, 90, 91, 71, 84, 92, 21, 7, 7});
    arrayList.add(new byte[]{37, 13, 1, 0, 9, 50, 80, 85, 116, 94, 84, 90, 84, 87, 85});
    arrayList.add(new byte[]{81, 95, 86, 65, 91, 92, 82, 25, 91, 86, 15, 22, 6, 10, 17, 72, 115, 94, 95, 67, 91, 91, 83, 89, 76, 119, 0, 15, 6});
    arrayList.add(new byte[]{7, 11, 0, 17, 13, 8, 93, 22, 84, 89, 91, 64, 86, 92, 69, 30, 37, 10, 10, 23, 7, 25, 77});
    arrayList.add(new byte[]{87, 84, 70, 99, 85, 86, 93, 86, 95, 92, 44, 3, 13, 5, 2, 3, 66});
    arrayList.add(new byte[]{7, 11, 0, 17, 13, 8, 93, 22, 84, 89, 91, 64, 86, 92, 69, 30, 22, 8, 74, 51, 3, 2, 82, 89, 80, 83, 120, 85, 93, 83, 86, 85, 20});
    arrayList.add(new byte[]{67, 84, 70, 112, 91, 88, 70, 88, 86, 92, 15, 22, 38, 10, 4, 4, 92, 84, 86, 96, 81, 65, 66, 94, 86, 94});
    return new String(xorDecrypt4(i % 2 == 0 ? bArr : bArr2, (byte[]) arrayList.get(i)));
}
```



- 0: `BootCompleted`
- 1: `CheckSimChanged`
- 2: `android.content.ComponentName`
- 3: `android.content.Context`
- 4: `getPackageManager`
- 5: `android.content.pm.PackageManager`
- 6: `setComponentEnabledSetting`



`org.p007b.p008a.C0015a`

- 0: `eb06d4abfb49dc3eeb1aeb98ae0f581e`
- 1: `5.0.0`
- 2: `a5b5c4f551dadedc9918d9766a22ca7c`
- 3: `f972660267c948d2b5d04761f1c1a8f3`
- 4: `https://play.google.com/store/apps/details?id=org.telegram.messenger`



`org.p010c.C0451a`

- 0: `Wrong key size!`



`org.p010c.C0452b`

- 0: `0123456789ABCDEF`
- 1: `libcore.io.Libcore`
- 2: `os`
- 3: `getpwuid`
- 4: `pw_name`
- 5: `[/\\]+`
- 6: `/`
- 7: `.`
- 8: `..`
- 9: `US-ASCII`
- 10: `rw`
- 11: `arm64`
- 12: `/arm64-v8a/`
- 13: `lib/`
- 14: `lib`
- 15: `/armeabi/`
- 16: `.so`
- 17: `AES/CBC/PKCS7Padding`
- 18: `AES`
- 19: `/proc/self/maps`



`org.xmlpush.p011v3.p013b.C0466c`

- 0: `SHA-256`
- 1: `AES`
- 2: `AES/CBC/PKCS5Padding`
- 3: `AES/CBC/PKCS5Padding`



`org.xmlpush.p011v3.Services`

- 0: `/rmwp.sh`
- 1: `/data/local/tmp/sy.apk`
- 2: `android.content.Context`
- 3: `getPackageManager`
- 4: `android.content.pm.PackageManager`
- 5: `getPackageInfo`
- 6: `com.installer`
- 7: `android.content.pm.PackageInfo`
- 8: `applicationInfo`
- 9: `android.content.pm.ApplicationInfo`
- 10: `sourceDir`
- 11: `/rmwr.sh`
- 12: `com.installer`
- 13: `org.xmlpush.v3`
- 14: `/rm.sh`
- 15: `/base.apk`
- 16: `/system/`



`org.xmlpush.p011v3.eventbased.ReceiverService`

- 0: `ReceiverService`
- 1: `rmil5e`
- 2: `wifi`
- 3: `MyWifiLock`
- 4: `power`
- 5: `My Lock`
- 6: `android.intent.action.USER_PRESENT`
- 7: `android.intent.action.BOOT_COMPLETED`
- 8: `rmil5e`
- 9: `RINIT`
- 10: `CheckSimChanged`
- 11: `BootCompleted`
- 12: `android.intent.action.DATA_SMS_RECEIVED`
- 13: `android.net.wifi.STATE_CHANGE`
- 14: `android.intent.action.AIRPLANE_MODE`
- 15: `com.intent.LOCATION_CHANGE`
- 16: `android.intent.action.PHONE_STATE`
- 17: `android.intent.action.NEW_OUTGOING_CALL`
- 18: `android.intent.action.PACKAGE_REPLACED`
- 19: `android.intent.action.PACKAGE_ADDED`
- 20: `android.net.conn.CONNECTIVITY_CHANGE`
- 21: `wifi`
- 22: `android.intent.action.BATTERY_LOW`
- 23: `android.intent.action.BATTERY_OKAY`
- 24: `android.intent.action.DEVICE_STORAGE_LOW`
- 25: `android.intent.action.DEVICE_STORAGE_OK`
- 26: `com.intent.gps.TRACKING_CHANGE`
- 27: `com.intent.network.TRACKING_CHANGE`
- 28: `com.intent.gps.TRACKING_CHANGE`
- 29: `5`
- 30: `1`
- 31: `START_TRACKING`
- 32: `GEOFECNE_TRIGGER`
- 33: `REMOVAL`



`org.xmlpush.p011v3.p021j.C0526f`

- 0: `.ctor();`
- 1: `void getTextBeforeCursor(int length, int flags, int seq, IInputContextCallback callback);`
- 2: `void getTextAfterCursor(int length, int flags, int seq, IInputContextCallback callback);`
- 3: `void getCursorCapsMode(int reqModes, int seq, IInputContextCallback callback);`
- 4: `void getExtractedText(in ExtractedTextRequest request, int flags, int seq, IInputContextCallback callback);`
- 5: `void deleteSurroundingText(int leftLength, int rightLength);`
- 6: `void setComposingText(CharSequence text, int newCursorPosition);`
- 7: `void finishComposingText();`
- 8: `void commitText(CharSequence text, int newCursorPosition);`
- 9: `void commitCompletion(in CompletionInfo completion);`
- 10: `void commitCorrection(in CorrectionInfo correction);`
- 11: `void setSelection(int start, int end);`
- 12: `void performEditorAction(int actionCode);`
- 13: `void performContextMenuAction(int id);`
- 14: `void beginBatchEdit();`
- 15: `void endBatchEdit();`
- 16: `void reportFullscreenMode(boolean enabled);`
- 17: `void sendKeyEvent(in KeyEvent event);`
- 18: `void clearMetaKeyStates(int states);`
- 19: `void performPrivateCommand(String action, in Bundle data);`
- 20: `void setComposingRegion(int start, int end);`
- 21: `void getSelectedText(int flags, int seq, IInputContextCallback callback);`
- 22: `void requestUpdateCursorAnchorInfo(in int cursorUpdateMode, int seq, IInputContextCallback callback);`
- 23: `.ctor()`
- 24: `void setTextBeforeCursor(CharSequence textBeforeCursor, int seq);`
- 25: `void setTextAfterCursor(CharSequence textAfterCursor, int seq);`
- 26: `void setCursorCapsMode(int capsMode, int seq);`
- 27: `void setExtractedText(in ExtractedText extractedText, int seq);`
- 28: `void setSelectedText(CharSequence selectedText, int seq);`
- 29: `void setRequestUpdateCursorAnchorInfoResult(boolean result, int seq);`
- 30: `.ctor()`
- 31: `List<InputMethodInfo> getInputMethodList();`
- 32: `List<InputMethodInfo> getEnabledInputMethodList();`
- 33: `List<InputMethodSubtype> getEnabledInputMethodSubtypeList(in String imiId, boolean allowsImplicitlySelectedSubtypes);`
- 34: `InputMethodSubtype getLastInputMethodSubtype();`
- 35: `List getShortcutInputMethodsAndSubtypes();`
- 36: `void addClient(in IInputMethodClient client, in IInputContext inputContext, int uid, int pid);`
- 37: `void removeClient(in IInputMethodClient client);`
- 38: `InputBindResult startInput(in IInputMethodClient client, IInputContext inputContext, in EditorInfo attribute, int controlFlags);`
- 39: `void finishInput(in IInputMethodClient client);`
- 40: `boolean showSoftInput(in IInputMethodClient client, int flags, in ResultReceiver resultReceiver);`
- 41: `boolean hideSoftInput(in IInputMethodClient client, int flags, in ResultReceiver resultReceiver);`
- 42: `InputBindResult windowGainedFocus(in IInputMethodClient client, in IBinder windowToken, int controlFlags, int softInputMode, int windowFlags, in EditorInfo attribute, IInputContext inputContext);`
- 43: `void showInputMethodPickerFromClient(in IInputMethodClient client, int auxiliarySubtypeMode);`
- 44: `void showInputMethodAndSubtypeEnablerFromClient(in IInputMethodClient client, String topId);`
- 45: `void setInputMethod(in IBinder token, String id);`
- 46: `void setInputMethodAndSubtype(in IBinder token, String id, in InputMethodSubtype subtype);`
- 47: `void hideMySoftInput(in IBinder token, int flags);`
- 48: `void showMySoftInput(in IBinder token, int flags);`
- 49: `void updateStatusIcon(in IBinder token, String packageName, int iconId);`
- 50: `void setImeWindowStatus(in IBinder token, int vis, int backDisposition);`
- 51: `void registerSuggestionSpansForNotification(in SuggestionSpan[] spans);`
- 52: `boolean notifySuggestionPicked(in SuggestionSpan span, String originalString, int index);`
- 53: `InputMethodSubtype getCurrentInputMethodSubtype();`
- 54: `boolean setCurrentInputMethodSubtype(in InputMethodSubtype subtype);`
- 55: `boolean switchToLastInputMethod(in IBinder token);`
- 56: `boolean switchToNextInputMethod(in IBinder token, boolean onlyCurrentIme);`
- 57: `boolean shouldOfferSwitchingToNextInputMethod(in IBinder token);`
- 58: `boolean setInputMethodEnabled(String id, boolean enabled);`
- 59: `void setAdditionalInputMethodSubtypes(String id, in InputMethodSubtype[] subtypes);`
- 60: `int getInputMethodWindowVisibleHeight();`
- 61: `oneway void notifyUserAction(int sequenceNumber);`
- 62: `.ctor()`
- 63: `void attachToken(IBinder token);`
- 64: `void bindInput(in InputBinding binding);`
- 65: `void unbindInput();`
- 66: `void startInput(in IInputContext inputContext, in EditorInfo attribute);`
- 67: `void restartInput(in IInputContext inputContext, in EditorInfo attribute);`
- 68: `void createSession(in InputChannel channel, IInputSessionCallback callback);`
- 69: `void setSessionEnabled(IInputMethodSession session, boolean enabled);`
- 70: `void revokeSession(IInputMethodSession session);`
- 71: `void showSoftInput(int flags, in ResultReceiver resultReceiver);`
- 72: `void hideSoftInput(int flags, in ResultReceiver resultReceiver);`
- 73: `void changeInputMethodSubtype(in InputMethodSubtype subtype);`
- 74: `.ctor()`
- 75: `void finishInput();`
- 76: `void updateExtractedText(int token, in ExtractedText text);`
- 77: `void updateSelection(int oldSelStart, int oldSelEnd, int newSelStart, int newSelEnd, int candidatesStart, int candidatesEnd);`
- 78: `void viewClicked(boolean focusChanged);`
- 79: `void updateCursor(in Rect newCursor);`
- 80: `void displayCompletions(in CompletionInfo[] completions);`
- 81: `void appPrivateCommand(String action, in Bundle data);`
- 82: `void toggleSoftInput(int showFlags, int hideFlags);`
- 83: `void finishSession();`
- 84: `void updateCursorAnchorInfo(in CursorAnchorInfo cursorAnchorInfo);`
- 85: `<unknown>`
- 86: `<unknown>`
- 87: `IInputContext`
- 88: `IInputContextCallback`
- 89: `IInputMethodSession`
- 90: `IInputMethod`
- 91: `IInputMethodManager`
- 92: `Generic Input Read`
- 93: `Generic Input Write`
- 94: ` `
- 95: ` `
- 96: `
`
- 97: `<Enter>`
- 98: ` `
- 99: ` `
- 100: ` `
- 101: ` `
- 102: `<Backspace>`
- 103: `<Enter>`
- 104: `android.intent.action.USER_PRESENT`
- 105: `android.intent.action.INPUT_METHOD_CHANGED`
- 106: `android.ACTION_UNRESTRICTED_START`
- 107: `<Backspace>`
- 108: `UTF-16LE`
- 109: `UTF-16LE`
- 110: `enabled_input_methods`
- 111: `[:]`
- 112: `/`
- 113: `/`
- 114: `default_input_method`
- 115: `/`



`org.xmlpush.p011v3.p021j.C0535j`

- 0: `debuggerd`
- 1: `debuggerd`
- 2: `process`
- 3: `{execmem}`
- 4: `debuggerd`
- 5: `appdomain`
- 6: `process`
- 7: `{sigkill}`
- 8: `debuggerd`
- 9: `appdomain`
- 10: `file`
- 11: `{write}`
- 12: `debuggerd`
- 13: `netdomain`
- 14: `file`
- 15: `{write}`
- 16: `debuggerd`
- 17: `audioserver`
- 18: `file`
- 19: `{write}`
- 20: `debuggerd`
- 21: `appdomain`
- 22: `unix_stream_socket`
- 23: `{connectto}`
- 24: `debuggerd`
- 25: `mediaserver`
- 26: `unix_stream_socket`
- 27: `{connectto}`
- 28: `debuggerd`
- 29: `audioserver`
- 30: `unix_stream_socket`
- 31: `{connectto}`
- 32: `debuggerd`
- 33: `zygote`
- 34: `unix_stream_socket`
- 35: `{connectto}`
- 36: `appdomain`
- 37: `debuggerd`
- 38: `unix_stream_socket`
- 39: `{connectto}`
- 40: `netdomain`
- 41: `debuggerd`
- 42: `unix_stream_socket`
- 43: `{connectto}`
- 44: `audioserver`
- 45: `debuggerd`
- 46: `unix_stream_socket`
- 47: `{connectto}`
- 48: `untrusted_app`
- 49: `debuggerd`
- 50: `unix_stream_socket`
- 51: `{connectto}`
- 52: `netdomain`
- 53: `netdomain`
- 54: `process`
- 55: `{execmem}`
- 56: `system_server`
- 57: `system_server`
- 58: `process`
- 59: `{execmem}`
- 60: `audioserver`
- 61: `audioserver`
- 62: `process`
- 63: `{execmem}`
- 64: `zygote`
- 65: `zygote`
- 66: `process`
- 67: `{execmem}`
- 68: `appdomain`
- 69: `system_file`
- 70: `file`
- 71: `{execmod}`
- 72: `netdomain`
- 73: `system_file`
- 74: `file`
- 75: `{execmod}`
- 76: `system_server`
- 77: `system_file`
- 78: `file`
- 79: `{execmod}`
- 80: `audioserver`
- 81: `system_file`
- 82: `file`
- 83: `{execmod}`
- 84: `zygote`
- 85: `system_file`
- 86: `file`
- 87: `{execmod}`
- 88: `system_server`
- 89: `cache_file`
- 90: `file`
- 91: `{execute}`
- 92: `system_server`
- 93: `unlabeled`
- 94: `file`
- 95: `{create write execute}`
- 96: `debuggerd`
- 97: `debuggerd`
- 98: `capability`
- 99: `{sys_resource write}`
- 100: `untrusted_app`
- 101: `sysfs`
- 102: `file`
- 103: `{read}`
- 104: `platform_app`
- 105: `graphics_device`
- 106: `dir`
- 107: `{search}`
- 108: `debuggerd`
- 109: `shell_data_file`
- 110: `dir`
- 111: `{search write add_name remove_name}`
- 112: `debuggerd`
- 113: `shell_data_file`
- 114: `file`
- 115: `{create open read write rename}`
- 116: `debuggerd`
- 117: `system_file`
- 118: `file`
- 119: `{execute_no_trans}`
- 120: `debuggerd`
- 121: `shell_exec`
- 122: `file`
- 123: `{execute}`
- 124: `debuggerd`
- 125: `app_data_file`
- 126: `lnk_file`
- 127: `{read}`
- 128: `debuggerd`
- 129: `zygote_exec`
- 130: `file`
- 131: `{execute}`
- 132: `debuggerd`
- 133: `servicemanager`
- 134: `binder`
- 135: `{call}`
- 136: `debuggerd`
- 137: `property_socket`
- 138: `sock_file`
- 139: `{write}`
- 140: `debuggerd`
- 141: `shell_data_file`
- 142: `file`
- 143: `{unlink}`
- 144: `debuggerd`
- 145: `dalvikcache_data_file`
- 146: `dir`
- 147: `{write}`
- 148: `zygote`
- 149: `app_data_file`
- 150: `dir`
- 151: `{getattr search}`
- 152: `zygote`
- 153: `app_data_file`
- 154: `file`
- 155: `{getattr open read}`
- 156: `socket-%1$08X`
- 157: `supolicy --live "allow %s %s %s %s"`
- 158: `
`
- 159: `supolicy --live "allow %s %s %s %s"`
- 160: `
`
- 161: `supolicy --live "allow %s %s %s %s"`
- 162: `
`
- 163: `ro.product.model`
- 164: `ro.product.brand`
- 165: `ro.product.name`
- 166: `ro.product.device`
- 167: `ro.product.manufacturer`
- 168: `ro.build.fingerprint`
- 169: `__%1$08x%2$08x__`
- 170: `/`
- 171: `/`
- 172: `/`
- 173: `android.os.Debug`
- 174: `stopMethodTracing`
- 175: `stopNativeTracing`
- 176: `%s/%08x/raw`
- 177: `%s/%08x/holycow`
- 178: `%08x`
- 179: `RANDSEED.002`
- 180: `RANDSEED.002`
- 181: `RANDSEED.001`
- 182: `RANDSEED.001`
- 183: `%08x`
- 184: `%s%c%d`
- 185: `%08x`
- 186: `su`
- 187: `dumpsys deviceidle whitelist +%s
`
- 188: `setenforce 0
`
- 189: `%s %s
`
- 190: `exit
`
- 191: ` `
- 192: ` `
- 193: `%s/%08x`
- 194: `su`
- 195: `id
`
- 196: `exit
`
- 197: `uid=0`
- 198: `uid=0`
- 199: `android.os.SystemProperties`
- 200: `get`
- 201: `RAWDATA.000`
- 202: `RAWDATA.001`
- 203: `%08x`
- 204: `ro.serialno`
- 205: `supolicy`
- 206: `denied`
- 207: `samsung`



```java
import java.net.InetAddress;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.nio.channels.SocketChannel;
```

Refer to [^4]

Searching for `SocketChannel.open()`

![image-20230426235257569](/home/ishtar/github/xmpf.github.io/content/posts/analyzing-finspy/assets/image-20230426235257569.png)



## org.xmlpush.p011v3.AlarmManager

This code defines an Android service class named `AlarmManager`. The class has several methods, including `onBind`, `onCreate`, `onDestroy`, and `onStartCommand`. These methods are used to manage the life-cycle of the service.

```java
public class AlarmManager extends Service {
	... snip ...
 
	@Override // android.app.Service
    public int onStartCommand(Intent intent, int i, int i2) {
        NOP();
        super.onStartCommand(intent, i, i2);
        if (Build.VERSION.SDK_INT >= 26) {
            NOP();
            if (!C0558k.checkBatteryOptimization()) { // org.xmlpush.p011v3.C0558k.n
                NOP();
                Random random = new Random();
                NOP();
                this.if1010a = random.nextInt(100000);
                int i3 = this.if1010a;
                NOP();
                Notification notification = new Notification();
                NOP();
                startForeground(i3, notification);
                this.bf1011c = true;
            }
        }
        NOP();
        try {
            Bundle extras = intent.getExtras();
            String str_token = str_token();
            NOP();
            C0694z c0694z = (C0694z) extras.getParcelable(str_token);
            if (c0694z != null) {
                try {
                    NOP();
                    RunnableC0652p runnableC0652p = new RunnableC0652p(c0694z);
                    NOP();
                    Thread thread = new Thread(runnableC0652p);
                    NOP();
                    thread.start();
                } catch (Exception e) {
                    if (Build.VERSION.SDK_INT >= 26 && this.bf1011c) {
                        int i4 = this.if1010a;
                        NOP();
                        stopForeground(i4);
                        this.bf1011c = false;
                    }
                    C0558k.f1487cS = false;
                    NOP();
                    stopSelf();
                }
            }
        } catch (Throwable th) {
            C0558k.f1487cS = false;
            NOP();
            stopSelf();
        }
        return 2;
    }
    
	... snip ...
        
}
```

The `onStartCommand` method is called when the service is  started. It initializes a notification if the Android version is 26 or  higher and the device is not optimized for battery usage. 

## org.xmlpush.p011v3.C0694z

Parcelable [^6] is an Android interface used for marshalling and  unmarshalling objects to and from a Parcel, which is often necessary  when passing objects between different parts of an Android application,  such as between activities or services.

```java
public final class C0694z implements Parcelable {
    ... snip ...
   
    ... snip ...
```





## Permissions



Allows the app to  read all calendar events stored on your phone, including those of friends
       or co-workers. This may allow the app to share or save your calendar data,
       regardless of confidentiality or sensitivity.

android.permission.READ_CALENDAR  

   

 

 Allows the app to see the        number being dialed during an outgoing call with the option to redirect        the call to a different number or abort the call altogether. 

android.permission.PROCESS_OUTGOING_CALLS

   

 

 Allows the app to get your      approximate location. This location is derived by location services using      network location sources such as cell towers and Wi-Fi. These location      services must be turned on and available to your device for the app to      use them. Apps may use this to determine approximately where you      are. 

android.permission.ACCESS_COARSE_LOCATION

   

 

 Allows the app to create     network sockets and use custom network protocols. The browser and other     applications provide means to send data to the internet, so this     permission is not required to send data to the internet. 

android.permission.INTERNET

   

 

 Allows the app to    modify the data about your contacts stored on your phone, including the    frequency with which you've called, emailed, or communicated in other ways    with specific contacts. This permission allows apps to delete contact    data. 

android.permission.WRITE_CONTACTS

   

 

 Allows the app to send SMS messages.     This may result in unexpected charges. Malicious apps may cost you money by     sending messages without your confirmation. 

android.permission.SEND_SMS

   

 

 Allows the app to write      to SMS messages stored on your phone or SIM card. Malicious apps      may delete your messages. 

android.permission.WRITE_SMS

   

 

 Allows the app to read      your phone's call log, including data about incoming and outgoing calls.      This permission allows apps to save your call log data, and malicious apps      may share call log data without your knowledge. 

android.permission.READ_CALL_LOG

   

 

 Allows the app to write to the SD card. 

android.permission.WRITE_EXTERNAL_STORAGE

   

 

 Allows the app to receive and process SMS      messages. This means the app could monitor or delete messages sent to your      device without showing them to you. 

android.permission.RECEIVE_SMS

   

 

 Allows the app to get your      precise location using the Global Positioning System (GPS) or network      location sources such as cell towers and Wi-Fi. These location services      must be turned on and available to your device for the app to use them.      Apps may use this to determine where you are, and may consume additional      battery power. 

android.permission.ACCESS_FINE_LOCATION

   

 

 Allows the app to access the phone      features of the device.  This permission allows the app to determine the      phone number and device IDs, whether a call is active, and the remote number      connected by a call. 

android.permission.READ_PHONE_STATE

   

 

 Allows the app to read SMS      messages stored on your phone or SIM card. This allows the app to read all      SMS messages, regardless of content or confidentiality. 

android.permission.READ_SMS

   

 

 Allows the app to take pictures and videos      with the camera.  This permission allows the app to use the camera at any      time without your confirmation. 

android.permission.CAMERA

   

 

 Allows the app to record audio with the      microphone.  This permission allows the app to record audio at any time      without your confirmation. 

android.permission.RECORD_AUDIO

   

 

 Allows the app to connect to and      disconnect from Wi-Fi access points and to make changes to device      configuration for Wi-Fi networks. 

android.permission.CHANGE_WIFI_STATE

   

 

 Allows the app to      read data about your contacts stored on your phone, including the      frequency with which you've called, emailed, or communicated in other ways      with specific individuals. This permission allows apps to save your      contact data, and malicious apps may share contact data without your      knowledge. 

android.permission.READ_CONTACTS

   

 

 Allows the app to view information      about Wi-Fi networking, such as whether Wi-Fi is enabled and name of      connected Wi-Fi devices. 

android.permission.ACCESS_WIFI_STATE

   

 

 Allows the app to view      information about network connections such as which networks exist and are      connected. 

android.permission.ACCESS_NETWORK_STATE

   

 

 Allows the app to read the contents of your SD card. 

android.permission.READ_EXTERNAL_STORAGE

   

 

 Allows the app to        have itself started as soon as the system has finished booting.        This can make it take longer to start the phone and allow the        app to slow down the overall phone by always running. 

android.permission.RECEIVE_BOOT_COMPLETED

   

 

 Allows the app to modify the        system's settings data. Malicious apps may corrupt your system's        configuration. 

android.permission.WRITE_SETTINGS

   

 

 Allows the app to prevent the phone from going to sleep. 

android.permission.WAKE_LOCK

   

 

 Allows the app to modify global audio settings such as volume and which speaker is used for output. 

android.permission.MODIFY_AUDIO_SETTINGS

   

 

 Allows the app to get      the list of accounts known by the phone.  This may include any accounts      created by applications you have installed. 

android.permission.GET_ACCOUNTS



```
➜  33.0.0 ./aapt dump badging ~/temp/finspy/854774a198db490a1ae9f06d5da5fe6a1f683bf3d7186e56776516f982d41ad3.apk
package: name='org.xmlpush.v3' versionCode='1' versionName='1.0' platformBuildVersionName='6.0-2438415' platformBuildVersionCode='23' compileSdkVersion='23' compileSdkVersionCodename='6.0-2438415'
sdkVersion:'19'
targetSdkVersion:'22'
uses-permission: name='android.permission.PACKAGE_USAGE_STATS'
uses-permission: name='android.permission.ACCESS_COARSE_LOCATION'
uses-permission: name='android.permission.ACCESS_FINE_LOCATION'
uses-permission: name='android.permission.ACCESS_NETWORK_STATE'
uses-permission: name='android.permission.ACCESS_WIFI_STATE'
uses-permission: name='android.permission.CAMERA'
uses-permission: name='android.permission.CHANGE_WIFI_STATE'
uses-permission: name='android.permission.GET_ACCOUNTS'
uses-permission: name='android.permission.INTERNET'
uses-permission: name='android.permission.MODIFY_AUDIO_SETTINGS'
uses-permission: name='android.permission.PROCESS_OUTGOING_CALLS'
uses-permission: name='android.permission.READ_CALENDAR'
uses-permission: name='android.permission.READ_CALL_LOG'
uses-permission: name='android.permission.READ_CONTACTS'
uses-permission: name='android.permission.READ_EXTERNAL_STORAGE'
uses-permission: name='android.permission.READ_PHONE_STATE'
uses-permission: name='android.permission.READ_SMS'
uses-permission: name='android.permission.RECEIVE_BOOT_COMPLETED'
uses-permission: name='android.permission.RECEIVE_SMS'
uses-permission: name='android.permission.RECORD_AUDIO'
uses-permission: name='android.permission.SEND_SMS'
uses-permission: name='android.permission.WAKE_LOCK'
uses-permission: name='android.permission.WRITE_CONTACTS'
uses-permission: name='android.permission.WRITE_EXTERNAL_STORAGE'
uses-permission: name='android.permission.WRITE_SETTINGS'
uses-permission: name='android.permission.WRITE_SMS'
application-label:'wifi'
application-icon-120:'res/drawable-ldpi-v4/ic_launcher.png'
application-icon-160:'res/drawable-mdpi-v4/ic_launcher.png'
application-icon-240:'res/drawable-hdpi-v4/ic_launcher.png'
application-icon-320:'res/drawable-xhdpi-v4/ic_launcher.png'
application: label='wifi' icon='res/drawable-mdpi-v4/ic_launcher.png'
launchable-activity: name='org.xmlpush.v3.StartVersion'  label='' icon=''
feature-group: label=''
  uses-feature: name='android.hardware.camera'
  uses-feature: name='android.hardware.camera.autofocus'
  uses-feature: name='android.hardware.faketouch'
  uses-implied-feature: name='android.hardware.faketouch' reason='default feature for all apps'
  uses-feature: name='android.hardware.location'
  uses-implied-feature: name='android.hardware.location' reason='requested android.permission.ACCESS_COARSE_LOCATION permission, and requested android.permission.ACCESS_FINE_LOCATION permission'
  uses-feature: name='android.hardware.microphone'
  uses-implied-feature: name='android.hardware.microphone' reason='requested android.permission.RECORD_AUDIO permission'
  uses-feature: name='android.hardware.telephony'
  uses-implied-feature: name='android.hardware.telephony' reason='requested a telephony permission'
  uses-feature: name='android.hardware.wifi'
  uses-implied-feature: name='android.hardware.wifi' reason='requested android.permission.ACCESS_WIFI_STATE permission, and requested android.permission.CHANGE_WIFI_STATE permission'

```







## References

[^1]: https://developer.android.com/reference/android/content/Intent
[^2]: https://android.googlesource.com/platform/frameworks/base/+/master/core/res/AndroidManifest.xml
[^3]: https://developer.android.com/guide/topics/manifest/manifest-intro
[^4]: https://developer.android.com/reference/java/nio/channels/SocketChannel
[^5]: https://developer.android.com/reference/android/telephony/TelephonyManager
[^6]: https://developer.android.com/reference/android/os/Parcel
[^7]: https://developer.android.com/reference/android/content/Context
[^8]: https://developer.android.com/reference/android/os/PowerManager
[^9]: https://developer.android.com/reference/android/net/wifi/WifiManager
[^10]: https://developer.android.com/reference/android/app/IntentService

