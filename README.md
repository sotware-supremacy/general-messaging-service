[![](https://jitpack.io/v/sotware-supremacy/General-Messaging-Service.svg)](https://jitpack.io/#sotware-supremacy/General-Messaging-Service)

# General Messaging Service (GMS)
> **for Google & Huawei**


## Installation
To get a Git project into your build:

Step 1. Add the JitPack repository to your build file
Add it in your root build.gradle at the end of repositories:

```gradle
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
}
```

Step 2. Add the dependency

```gradle
dependencies {
	        implementation 'com.github.sotware-supremacy:General-Messaging-Service:<VERSION>'
}
```

Step 3. Add this to build.gradle in project-level:

```groovy
buildscript {
    ext {
        ...
        gms_service_version = '4.3.13'
        hms_service_version = '1.6.0.300'
    }
    ...
    dependencies {
        ...
        classpath "com.huawei.agconnect:agcp:$hms_service_version"
        classpath "com.google.gms:google-services:$gms_service_version"
    }
}
```

## Usage
> **put this to build.gradle in app-level**

```groovy
plugins {
    // Comment one who you needn't
    id 'com.huawei.agconnect'
    id 'com.google.gms.google-services'
}
```

> **put this in AndroidManifest.xml**
```xml
<application>
    ...
    
    <!-- For Google Services-->
           
    <service
        android:name="com.gateway.gms.data.GoogleService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT" />
        </intent-filter>
    </service>


    <!-- For Huawei Services-->
    
    <service
        android:name="com.gateway.gms.data.HuaweiService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.huawei.push.action.MESSAGING_EVENT" />
        </intent-filter>
    </service>
</application>
```

> **Don't miss to put authentication json file in app package.**

```
app/
    google-services.json
    [OR]
    huawei-services.json
```
    

```kotlin
import com.gateway.gms.di.GMServiceLocator

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
	
	// initialize service 
	
	GMServiceLocator.initializeService(this.application)
	
	// Use Repository
	
	CoroutineScope(Dispatchers.IO).launch{
            with(GMServiceLocator.cloudMessagingRepository) {
                this.subscribeToTopic("Technology").collect {
                    // Do Something
                }
            }
        }
    }
}
```


> Also you can implement `CloudMessagingServiceListener`


```kotlin
...
import com.gateway.gms.domain.interfaces.CloudMessagingServiceListener

class MainActivity : AppCompatActivity(), CloudMessagingServiceListener {
	...
    
    override fun onMessageReceived(message: com.google.firebase.messaging.RemoteMessage) {
        // Do Something
    }

    override fun onMessageReceived(message: com.huawei.hms.push.RemoteMessage?) {
        // Do Something
    }

    override fun onNewToken(token: String) {
        // Do Something
    }
}
```
