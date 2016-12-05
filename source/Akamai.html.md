---
title: Akamai Documentation

language_tabs:
  - shell: cURL
  - php: PHP



search: true
---
<div>
	<nav>
	<a> Introduction</a>
	<a> Getting Started</a>
	<a> How PCD works</a>
</nav>
</div>

# INTRODUCTION

### Welcome!

Akamai's Predictive Content Delivery technology is constructed and designed so as to provide steady, high-quality mobile video experiences with no hassles of time lag, buffering, stalling or any other interruption - regardless of geographic location, time of day, or environmental obstructions.

PCD provides an exceptional video experience to the mobile consumer - the playback is instantaneous, easy to dig videos and more to it, they are continuously available. Also, the cCODE ontent delivered to the mobile user is according to his/her preferences.

### The Benefits PCD will bring to your Business

* With instant start-up videos, the user experience is going to sore high, providing the users with superior video experience. No buffering or stalling regardless of location or network conditions.
* Automatic content refresh will increase discovery and consumption of mobile videos with personalized downloads
* Build customer loyalty with a differentiated and a unique service quality.
* Provide continuous access to your content by caching content on the device.

### Fascinating Facts about PCD

* Off board cellular capacity and Wi-Fi
* Device storage
* Predictive analytics
* Smart battery management
* Extending video consumption
* Improving audience engagement
* Increasing quality
* Optimizing video delivery

# GETTING STARTED

Welcome to Akamai Predictive Content Delivery API Guide

The first step to using or evaluating any API is to get an API client up and running. This section of the documentation is intended to get you up-and-running with - Predictive Content Delivery API and give a high level conceptual overview of the API. We'll cover everything you need to know.

Understanding this API has been made as simple as possible with the use of images, which better explain the various steps.

This API has been developed to provide a platform into which you can manage content that will be pre-positioned on end user mobile devices.


## ANDROID

Predictive Content Delivery (PCD) SDK prepositions content, based on user preferences and policies set up between the client and server. There is absolutely no need for a developer to initiate downloads as prepositioning operates on a notification basis. Downloads also continue whether the app is active in the foreground, running in the background, or entirely closed. The SDK acts as a data cache, providing access to content along with its current download state. 

Configuring your project to support these features is covered below.

## Prerequisites for using the SDK (Obtaining key for GCM)

To generate a key from Google's firebase console using GCM in android app, follow these instructions:

* Go to [firebase console] (https://console.firebase.google.com/project/_/settings/cloudmessaging).
* Create a new project.
* It will give you a server-key that will be used by backend developers when creating push requests
* You also need to save the Sender ID given here that will be used on Android for receiving push notifications.

![Prerequisites for using the SDK](1.png)


## File Location to Add Dependencies

Android projects quite often rely on 3rd party libraries and SDKs, and have thier own way to add the dependencies.

You need to add these 3rd party dependencies to a file named build.gradle(Module:app) which is present by default in Android (Refer to the image, the file is highlighted in blue). This file is located in a folder named 'app'.

> Code Snippet

``` shell
apply plugin: 'com.android.application'


android {
  compileSdkVersion 24
  buildToolsVersion "24.0.1"


  defaultConfig {
      applicationId "akamai.sdk.demo"
      minSdkVersion 15
      targetSdkVersion 24
      versionCode 1
      versionName "1.0"
  }
  buildTypes {
      release {
          minifyEnabled false
          proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
      }
  }
}


dependencies {
  compile fileTree(dir: 'libs', include: ['*.jar'])
  testCompile 'junit:junit:4.12'
  compile 'com.android.support:appcompat-v7:24.2.0'
}
```


![File Location to Add Dependencies](2.png)


## Dependencies to be Added

To start using the SDK, you will be required to add the following dependencies:

<code>
compile 'com.google.android.gms:playservicesbase:7.0.0'<br>
compile 'com.google.android.gms:playservicesgcm:7.0.0'<br>
compile 'com.google.android.gms:playservicesads:7.0.0'<br>
compile(name:'voc­sdk­release­{version}', ext:'aar') <br>
</code>

The requirement is that, the project should be implementing API 15 or greater. These dependencies include Google Play services, Google Cloud Messaging and all the dependencies required for PCD.

> Code Snippet

``` shell
apply plugin: 'com.android.application'


android {
  compileSdkVersion 24
  buildToolsVersion "24.0.1"


  defaultConfig {
      applicationId "akamai.sdk.demo"
      minSdkVersion 15
      targetSdkVersion 24
      versionCode 1
      versionName "1.0"
  }
  buildTypes {
      release {
          minifyEnabled false
          proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
      }
  }
}


dependencies {
compile 'com.google.android.gms:playservicesbase:7.0.0'
compile 'com.google.android.gms:playservicesgcm:7.0.0'
compile 'com.google.android.gms:playservicesads:7.0.0'
compile(name:'voc­sdk­release­{version}', ext:'aar')
}
```

![Dependencies to be Added](3.png)

## Component Details File

All the details of the components being used in the project are present in a file known as AndroidManifest.xml, which is more like an index. It contains information about all the Activity, Broadcast Receiver, User Permissions and more.. To locate this file, you need to navigate to app>src>main.

![Component Details File](4.png)

## Add Permissions

For proper functioning of the SDK, you need some permissions from the user. Permissions like Internet, Access Network State, WiFi State Access and the permission to write to external storage. All these permissions are required to be added to the _AndroidManifest.xml_ file.

Following are the permissions:

> Code Snippet 

``` Shell

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="akamai.sdk.demo">

  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />


  <application
      android:allowBackup="true"
      android:icon="@mipmap/ic_launcher"
      android:label="@string/app_name"
      android:supportsRtl="true"
      android:theme="@style/AppTheme">
      <activity android:name=".MainActivity">
          <intent-filter>
              <action android:name="android.intent.action.MAIN" />


              <category android:name="android.intent.category.LAUNCHER"/>
          </intent-filter>
      </activity>
  </application>

</manifest>
```


![Add Permissions](5.png)

## Add a Receiver and a Provider

For the SDK to work correctly, we need to add a receiver to listen to Voc Status messages and a provider to access the cached content.

It can be added in _AndroidManifest.xml_ inside the application tag.

> Code Snippet 

``` Shell
<receiver
android:name="akamai.sdk.demo.VocBroadcastReceiver">
<intent­filter>
<action android:name="com.akamai.android.sdk.ACTION_VOC_STATUS" />
<category android:name="akamai.sdk.demo" />
</intent­filter>
</receiver>


<provider
android:name="com.akamai.android.sdk.db.AnaContentProvider"
android:authorities="akamai.sdk.demo.AnaContentProvider" >
</provider>
```

![Add a Receiver and a Provider](66.png)

## iOS

Predictive Content Delivery (PCD) SDK prepositions content, based on user preferences and policies set up between the client and server. There is absolutely no need for a developer to initiate downloads as prepositioning operates on a notification basis. Downloads also continue whether the app is active in the foreground, running in the background, or entirely closed. The SDK acts as a data cache, providing access to content along with its current download state. Configuring your project to support these features is covered below.

## Prerequisites

The PCD SDK requires **iOS 8** or higher.

For registration, a **PCD SDK license key** is a mandate.

The application's product name **(Project → Choose target → Build Settings → Packaging → Product name)** must match the name provided on the PCD Web portal SDK license page. The portal field for this is **"iOS Application ID."** The app must enable Background Execution and Remote Notifications in order to preposition the content.

![Prerequisites](7.png)

## Add the framework

- Download the PCD SDK and unzip it in a folder under your project, e.g. _~/myproject/pcd_sdk._
- Navigate to **VocSdk.framework** under _~/myproject/pcd_sdk/iphoneos/_ and copy it to your project folder, e.g. ~/myproject/VocSdk.framework. This is done so that the framework is available before the first build and there-on you can add it to your project. On each build afterward it will be copied here by the copy_vocsdk build phase script.
- Add the framework to your **Xcode** project.
  - Open your project in Xcode.
  - Open the **File** menu.
  - Click **Add Files** to
  - Choose **~/myproject/VocSdk.framework**

  ![Add the framework](8.png)

## Link the SDK to the project

Now, you will need to link the SDK to your project, follow these steps:

By clicking the project name in the Project navigator, open **project settings.**
Click the **General tab.**
Under **Embedded Binaries,** click '+' and choose **VocSdk.framework**
Click Add.

![Link the SDK to the project](9.png)

## Add a build phase to Project settings

Before you go ahead building your app, the correct framework for your platform simulator or device must be copied into the right place and this requires a build phase to be added to your project settings.

Follow these set of instructions:

* Choose your **build target* in Project Settings, then click on **Build Phases.**
* Next, to add a new build phase, click the '+' and choose **'New Run Script Phase.'**
* Drag this build phase to position it after Target Dependencies.
* Optionally, single click its name and rename it to **copy_vocsdk.**
* To expand the copy_vocsdk row, click the arrow.
* Leave the default shell setting of _/bin/sh._
* The following script needs to be added to the script area.

The "copy_vocsdk.sh" path is relative to your .xcodeproj file and will need modification depending on where you unzipped it (in this example, it is pcd_sdk).

<code>. ./pcd_sdk/copy_vocsdk.sh "${PROJECT_DIR}/pcd_sdk"</code>

The second parameter, "${PROJECT_DIR}/pcd_sdk", is the location of the /iphoneos and /iphonesimulator folders from the distribution archive.

In this example, the folder structure is as follows: <br>
/myproject/myproject.xcodeproj /myproject/pcd_sdk/copy_vocsdk.sh /myproject/pcd_sdk/iphoneos/ /myproject/pcd_sdk/iphonesimulator/ 

The script will copy the correct VocSdk.framework to /myproject/ folder at the start of every build.

This is how the build phase will finally look.

![Add a build phase to Project settings](10.png)

## Integrate your iOS

Integrating with your iOS Application - Including the SDK.

Follow these instructions to integrate your iOS.

Wherever the SDK is required, import the VocSdk header in any of the class files. For your app, you will need to define a single VocService object.

Create your VocService object in the app delegate to make the SDK available early in the app lifecycle and to simplify access to the VocService from other classes.

**#import @property (strong, nonatomic) id vocService;**

#### Initialization

The SDK is initialized by the VocServiceFactory call

createServiceWithDelegate:delegateQueue:options:error:.

To begin prepositioning files as early as possible, initialization and registration should take place early in the app lifecycle. The recommended place for this is in AppDelegate's application:didFinishLaunchingWithOptions:

The create call inputs a reference to the SDK delegate as well as a configuration options dictionary. To respond to SDK activity, the SDK delegate is the class you need to designate.

The example below is inserted into **AppDelegate.m.** It initializes the SDK and tells it that AppDelegate will be the delegate to handle SDK messages.

> Example code for iOS integration

```php
(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{ 
NSError *error = nil;
self.vocService = [VocServiceFactory createServiceWithDelegate:self delegateQueue:[NSOperationQueue mainQueue] options:nil error:&error]; 
if (!self.vocService)
{
// error handling ­ could not start service 7 
return NO;
} 
// app initialized return YES;
} 

```

![Integrate your iOS](11.png)

Need Help? 
If questions come up along the way, check out the frequently asked questions. Still puzzled? Visit our community discussion forums, probably there are others who have already bumped into the same issue as yours.

# HOW PCD WORKS

![How PCD Works] (pcdarch.jpg) 

The App has 3 features:

* **Predictive Analysis** - Akamai's recommendation engine curates personalized content either through a set of customer preference settings or as users interact with the application.
* **Download manager** - The download manager downloads all the content to the device without any barriers or hassle for better mobile user experience.
* **Storage and Battery Management** - When a mobile users launches the video, the video is launched instantly with no buffering, lag or stalling. This is because the videos are stored on the mobile device and hence the playback delivered is of the highest quality and not affected by network conditions. 
Mobile users do not have to worry about manually clearing the stored or outdated video content as the videos refresh on a regular basis and outdated videos are removed.
* The App and Akamai PCD communication is bidirectional. They communicate with each other over cellular data and Wi-Fi for content to be uploaded on the app with Https being the transport medium.

Internally, that is within the Akamai Predictive Content Delivery, the client API, web server, predefined window download manager communicate with the predictive engine & content interface.

The predictive engine & content interface in turn bidirectionally communicate with the content aggregators and media companies & OVPs to generate and gather the required content.

Once the content aggregators and media companies & OVPs send the content, the content is stored in the storage.

The Predictive engine then looks into the user's preferences and picks the right content that will be displayed to the user.

The videos are either picked from the Akamai cloud or the general internet.

In case, the content generators have no content to push, general content from the internet will be uploaded to the app. E.g. Bloomberg TV.

# CODE SAMPLES

## Introduction

The Predictive Content Delivery (PCD) solution provides a platform into which you can manage content to be pre-positioned on end user mobile devices. We have created this guide to help you provision content in the PCD application.

The API is currently in its first iteration. 
All calls must use the format <code>/ingest/v1/{api_call}</code>.

For example: <code> /ingest/v1/updateContentById </code>

## Authentication

> Example of Content Provider ID and Access Token authentication:

```shell
curl -H "Content-Type: application/json" -d 
'{"access_token":"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81",
content_id":"5907", "content_provider_id":"cnbc"}'
-k 'https://23.79.234.237/ingest/v1/getContentById' 

curl -H "Content-Type: application/json" -d 
'{"access_token":"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81",
"content_provider_id":"cnbc"}'
-k 'https://23.79.234.237/ingest/v1/getContentIds' 

curl -H "Content-Type: application/json" -d 
'{"access_token":"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81",
content_id":"5907", "content_provider_id":"cnbc"}'
-k 'https://23.79.234.237/ingest/v1/getActivityStatusById' 
```

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"cnbc\",\"access_token\": \"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81\"}",\"conetnt_id\": \"5907\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?> 

<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentIds",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"cnbc\",\"access_token\": \"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81\",\"conetnt_id\": \"5907\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?> 

<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getActivityStatusById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"cnbc\",\"access_token\": \"5278fb26f31bcbf29594bf23bd411879d122a2508d511448d478ab81\",\"conetnt_id\": \"5907\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?> 
```

An access token and a content provider ID are provided to content providers who are authorized to use the API .

Also, to submit content to the PCD as an aggregation of multiple content providers, you must have an access token for each content provider. To ensure a successful request, you must use each access token with the corresponding provider.

You must use both values in the body of any post call to authenticate the API request.

The following provides a sample part of an API request (application/json) containing the content provider ID and the access token:

<code>
{ <br>
	&nbsp;&nbsp;&nbsp;&nbsp;content_provider_id": "akamai_internal",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "ab203jduns03kd0",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;...<br>
}
</code>
### Return Codes

The response containing a "return code" value returns each API call with a JSON. The meaning of the return code depends on the context of the API call.

A sample of return codes and their interpretation are listed in the following table:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Success Success. | Content Successfully.
400 | Missing data  | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
422 | Duplicate | Provider tried to add a piece of content with a unique ID that is already in the database.
500 | Unknown | An unknown error occurred while processing the request.
403 | Bad content id  | The provided content ID is not associated with the content provider.
403 | Update not permitted  | The access token cannot update the specified content.
403 | Updating unique id error  | The request is unable to update the unique ID to a value that matches another record in the database.
403 | Purge not permitted | The access token is unable to purge the specified content.


Example of response from a request with error code:

<code>
Response 400 (application/json)
<br>
{<br>
&nbsp;&nbsp;"return_code": "Access denied"<br>
}<br>
</code>


## GETTING A CATEGORY LIST
> Example Code for Getting a Category List:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentCategories",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?> 
```

```shell
curl -X POST -H "Content-Type: application/json" -d 
'{"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE" 
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentCategories"
```

When using the API to add or update content, you must know the list of available categories as the initial PCD application release supports only a certain set of categories with which you can associate content.

To view a list of available content categories, you can use the getContentCategories API call.

*Note:* "Trending" is currently a PCD reserved keyword; it is not a content category.

To get a list of content categories, use the following command:

<code> POST /ingest/v1/getContentCategories </code>

*URI Parameters:* _content_provider_id,access_token_


Parameter | Required | Description
--------- | -------- | ------------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider

### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | ------------
200 | Success Success. | Request completed.
400 | Missing data |  Request json is missing essential data to complete the request
401 | Access denied | Access Token validation failed
500 | Unknown | Unknown error occurred while processing the request


### Request

Headers

Content-Type: application/json
<br>
<code>
Body<br>
{ <br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}<br>
</code>

#### Response 200

Headers

Content-Type: application/json
<br>
<code>
Body <br>
{ <br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_category":"string of a supported content category" <br>
&nbsp;&nbsp;&nbsp;&nbsp;...<br>
}
</code>

#### Response 400

Headers

Content-Type: application/json
<br>
<code>
Body <br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here" <br>
}
</code>

#### Response 500

Headers

Content-Type: application/json
<br>
<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown" <br>
}
</code>


#### Result:

Response 200

HEADERS

Content-Type:application/json

> Response body

```json
[
"Ads",
"Entertainment",
"Business" 
]
```

## ADDING CONTENT

> Example Code - Adding Content (mp4 format & m3u8 format)


```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/addContent",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_video_width\": 1024,\"content_video_height\": 720,\"content_bit_rate\": 4096,\"content_categories\": [\"Entertainment\"],\"content_tags\": [{\"text\": \"stehd\",\"weight\": \"2\"}],\"thumb_width\": 1024,\"thumb_height\": 720,\"thumb_length\": 1280,\"content_title\": \"Akamai-Edge 2016\",\"content_description\": \"Where the world \'\s leading brands unlock the opportunities of the internet. You'll meet 2,000 Attendees from 30+ Countries and can network with innovators from brands big and small. Don't miss out on the most productive event for your online business.\",\"content_duration\": 65,\"content_unique_id\": \"a-lL66tBZZA\",\"streams\": [{\"url\": \"http://tools.watch-now.co/a-lL66tBZZA/index.mp4\",\"type\": \"mp4\",\"size\": \"12324\"}],\"publication_timestamp\": \"2016-08-09\",\"thumb_url\": \"http://tools.watch-now.co/a-lL66tBZZA/video.jpg\"}", CURLOPT_HTTPHEADER => array(
"content-type: application/json"
),
));

$response = curl_exec($curl);

?> 
```

```php
<?php 
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/addContent",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_video_width\": 1024,\"content_video_height\": 720,\"content_bit_rate\": 4096,\"content_categories\": [\"Entertainment\"],\"content_tags\": [{\"text\": \"stehd\",\"weight\": \"2\"}],\"thumb_width\": 1024,\"thumb_height\": 720,\"thumb_length\": 1280,\"content_title\": \"Akamai-Edge 2016\",\"content_description\": \"Where the world\'s leading brands unlock the opportunities of the internet. You'll meet 2,000 Attendees from 30+ Countries and can network with innovators from brands big and small. Don't miss out on the most productive event for your online business.\",\"content_duration\": 65,\"content_unique_id\": \"a-lL66tBZZA\",\"streams\": [{\"url\": \"http://tools.watch-now.co/a-lL66tBZZA/index.m3u8\",\"type\": \"m3u8\",\"size\": \"12324\"}],\"publication_timestamp\": \"2016-08-09\",\"thumb_url\": \"http://tools.watch-now.co/a-lL66tBZZA/video.jpg\"}", CURLOPT_HTTPHEADER => array(
"content-type: application/json"
),
));

$response = curl_exec($curl);

?> 
```

```shell
curl -X POST -H "Content-Type: application/json" -d '{"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_video_width": 1024,
"content_video_height": 720,
"content_bit_rate": 4096,
"content_categories": [
"Entertainment"
],
"content_tags": [{
"text": "stehd",
"weight": "2"
}],
"thumb_width": 1024,
"thumb_height": 720,
"thumb_length": 1280,
"content_title": "Akamai-Edge 2016",
"content_description": "Where the worlds leading brands unlock the opportunities of the internet. You'll meet 2,000 Attendees from 30+ Countries and can network with innovators from brands big and small. Don't miss out on the most productive event for your online business.",
"content_duration": 65,
"content_unique_id": "a-lL66tBZZA",
"streams": [{
"url": "http://tools.watch-now.co/a-lL66tBZZA/index.m3u8",
"type": "m3u8",
"size": "12324"
}],
"publication_timestamp": "2016-08-09",
"thumb_url": "http://tools.watch-now.co/a-lL66tBZZA/video.jpg"}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/addContent"
```


This section will explain how to add content to the PCD platform. Currently, PCD enables you to submit video information using two formats - mp4 and m3u8 (HLS). You can add as many variants of the video as you like for a particular video.

Note: Future versions of PCD will support other video formats (for example, dash and flv) using the streams field.

To add content, use the following command:

<code> POST/ingest/v1/addContent </code>

To add an mp4 variant, add a JSON object to the streams array using the following format:

<code>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"url":"mp4 url", <br>
&nbsp;&nbsp;&nbsp;&nbsp;"size":"length of mp4 in bytes",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"type":"mp4"<br>
}
</code>


To add an m3u8 variant, add a JSON object to the streams array using the following format:

<code> 
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"preferred_stream": "bitrate to choose from the m3u8 file" , <br>
&nbsp;&nbsp;&nbsp;&nbsp;"url":"m3u8 url", <br>
&nbsp;&nbsp;&nbsp;&nbsp;"size":"length of file in bytes", <br>
&nbsp;&nbsp;&nbsp;&nbsp;"type":"m3u8"<br>
} 
</code>


URI Parameters: _content_provider_id,access_token_

Parameter | Required | Description
--------- | -------- | -----------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider
content_video_width | true |  integer - width in pixels
content_video_height | true | integer - height in pixels
content_bit_rate | true | integer - bits per second
content_categories | true | Content category
content_tags | true |  
thumb_width | true | thumbnail width
thumb_height | true | thumbnail height
thumb_length | true | thumbnail length in bytes
content_title | true | Title associated with the content
content_description | true | description of the content
content_duration | true | thumbnail length in bytes
content_unique_id | true | Unique id of the content as determined by the provider
streams| true | Format of objects (mp4 or m3u8)
publication_timestamp | true | Published date of the content
thumb_url | true | HTTP URL associated with the thumbnail


### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Success Success. | Request completed.
400 | Does not conform to spec | Something in the request does not conform to specification; the request cannot continue.
400 | Missing data| The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Add not permitted | The access token cannot add content.
403 | Content category not permitted | The content category is not supported by the PCD application.
403 | No variants of content provided | No mp4 or m3u8 variants of the content were provided.
422 | Duplicate | The provider attempted to add a piece of content with a unique ID that already resides in the database.
500 | Unknown | An unknown error occurred while processing the request.

### Request

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_width": integer - width in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_height": integer - height in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_bit_rate": integer - bits per second,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_categories": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;""<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_description": "string - textual long form description of the content"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_duration": integer - seconds,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_unique_id": string - Unique id of the content as determined by the provider,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_tags": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"text": "string - Tags associated with the Content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"weight": "string - Weight associated with the Tag"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_title": "string - Title associated with the content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"streams": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string - mp4 url of content",<br> 
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "mp4"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "string - length of mp4 in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_AND/OR objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"preferred_stream": "bitrate to choose from the m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string = url of m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "m3u8",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "length of stream in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"publication_timestamp": "string "YYYY-MM-DD HH:MM:SS",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_width": integeger - thumbnail width,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_height": integer - thumbnail height,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_length": integer - thumbnail length in bytes,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_url": "string - HTTP URL associated with the thumbnail" ,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"ad_url": "OPTIONAL: string - HTTP URL of ad to play with content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"sdk_metadata_passthrough": "OPTIONAL: string- any metadata a customer wants to pass directly through the sdk for later use (e.g. a json, xml,text as a string) ",<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "string - id of content"<br>
}<br>
</code>

### Response 400

Headers

Content-Type: application/json

<code> Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown" <br>
}
</code>

### Result:
### Response 200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": 346708,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>

## UPDATING CONTENT METADATA BY ID

> Example Code - Updating Content Metadata By ID

```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/updateContentById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"content_id\":346708,\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_title\": \"Akamai-Edge 2016\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```

```shell
curl -X POST -H "Content-Type: application/json" -d '{"content_provider_id": "akamai_internal",
"content_id":346708,
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_title": "Akamai-Edge 2016"}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/updateContentById"
```

This action enables you to update a single content entry using its unique ID.

To update content by ID, use the following command:

<code> POST/ingest/v1/updateContentById </code>



URI Parameters: content_provider_id,access_token,content_id

Parameter | Required | Description
--------- | -------- | ------------
access_token | true | Access token associated with each provider
content_provider_id |true | ID associated with each provider
content_id | true |  ID of the content created by the provider
content_video_width | true | integer - width in pixels
content_video_height | true | integer - height in pixels
content_bit_rate | true | integer â€“ bits per second
content_categories | true | Content category
content_tags  | true |  
thumb_width | true | thumbnail width
thumb_height | true |  thumbnail height
thumb_length  | true | thumbnail length in bytes
content_title | true | Title associated with the content
content_description | true | description of the content
content_duration | true | thumbnail length in bytes
content_unique_id | true | Unique id of the content as determined by the provider
streams | true | Format of objects (mp4 or m3u8)
publication_timestamp | true | Published date of the content
thumb_url | true | HTTP URL associated with the thumbnail


### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | ------------
200 | Success Success. | Request completed.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id  | The provided content ID is not associated with any content.
403 | Update not permitted | The provided access token does not have permission to update content.
403 | Update unique id error | Updating the content ID results in more than one entry with the same ID; IDs must be unique.
403 | Content category not permitted | You are attempting to update the content with an unsupported category.
500 | Unknown |An unknown error occurred while processing the request.

### Request 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "id of a content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_width": integer - width in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_height": integer - height in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_bit_rate": integer - bits per second,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_categories": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;""<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_description": "string - textual long form description of the content"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_duration": integer - seconds,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_unique_id": string - Unique id of the content as determined by the provider,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_tags": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"text": "string - Tags associated with the Content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"weight": "string - Weight associated with the Tag"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_title": "string - Title associated with the content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"streams": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string - mp4 url of content",<br> 
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "mp4"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "string - length of mp4 in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_AND/OR objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"preferred_stream": "bitrate to choose from the m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string = url of m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "m3u8",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "length of stream in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"publication_timestamp": "string "YYYY-MM-DD HH:MM:SS",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_width": integeger - thumbnail width,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_height": integer - thumbnail height,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_length": integer - thumbnail length in bytes,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_url": "string - HTTP URL associated with the thumbnail" ,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"ad_url": "OPTIONAL: string - HTTP URL of ad to play with content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"sdk_metadata_passthrough": "OPTIONAL: string- any metadata a customer wants to pass directly through the sdk for later use (e.g. a json, xml,text as a string) ",<br>
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}<br>
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}<br>
</code>


### Result:
### Response200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>

## PURGE BY CONTENT ID

>Example Code for Purging Content By ID:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/requestPurgeById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",
\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",
\"content_id\":346708}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```
```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' 
"https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/requestPurgeById"
```

This action enables you to purge (remove) content from PCD using the content ID.

Use the following command to purge content by ID:

<code>POST/ingest/v1/requestPurgeById</code>

URI Parameters: content_provider_id,access_token,content_id

Parameter | Required |Description
--------- | -------- |-----------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider
content_id | true | ID of the content created by the provider


### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Success | Success. Request completed.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | The provided content ID is not associated with any content.
403 | Purge not permitted | The provided access token cannot purge content.
500 | Unknown | An unknown error occurred while processing the request.

### Request Get content ids

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "the id of content",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"</br>
}
</code>



### Result:

### Response200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>


## GETTING ALL CONTENT IDs

> Example Code for Getting All Content By IDs:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentByIds",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));
$response = curl_exec($curl);
?>
```
```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentByIds"
```

This action returns all associated content IDs.

To view all content IDs, use the following command:

<code> POST/ingest/v1/getContentIds </code>

URI Parameters: content_provider_id,access_token

Parameter | Required | Description
--------- | -------- | ------------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider




### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Active | Success. Request completed. The content is active.
200 | Inactive | Success. Request completed. The content is inactive.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | The provided content ID is not associated with any content.
500 | Unknown | An unknown error occurred while processing the request.

### Request Get content ids

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
[<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "content_id"<br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
]
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
} 
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}
</code>

### Result:
### Response 200

Headers

Content-Type:application/json

<code>
[<br>
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_id": 346708<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
] 
</code>

## GETTING CONTENT METADATA BY ID

> Example code for Getting Content Metadata By IDs:

```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_id\": \"346708\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```
```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getContentById"
```


This call enables you to retrieve meta-information of a particular content using the content ID.

Use the following command to view particular content by content ID:

<code> POST/ingest/v1/getContentById </code>

URI Parameters:content_provider_id,access_token,content_id

Parameter | Required | Description
--------- | -------- | -----------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider
content_id | true | ID of the content created by the provider



### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Active  | Success. Request completed. The content is active.
200 | Inactive | Success. Request completed. The content is inactive.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | The provided content ID is not associated with any content.
500 | Unknown | An unknown error occurred while processing the request.uly

### Request Get content by id

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "the id of content",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{
	&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "id of a content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_width": integer - width in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_video_height": integer - height in pixels,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_bit_rate": integer - bits per second,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_categories": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;""<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_description": "string - textual long form description of the content"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_duration": integer - seconds,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_unique_id": string - Unique id of the content as determined by the provider,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_tags": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"text": "string - Tags associated with the Content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"weight": "string - Weight associated with the Tag"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"content_title": "string - Title associated with the content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"streams": [<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string - mp4 url of content",<br> 
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "mp4"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "string - length of mp4 in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_AND/OR objects of this format_<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"preferred_stream": "bitrate to choose from the m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "string = url of m3u8 file",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "m3u8",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "length of stream in bytes"<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"publication_timestamp": "string "YYYY-MM-DD HH:MM:SS",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_width": integeger - thumbnail width,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_height": integer - thumbnail height,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_length": integer - thumbnail length in bytes,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"thumb_url": "string - HTTP URL associated with the thumbnail" ,<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"ad_url": "OPTIONAL: string - HTTP URL of ad to play with content",<br>
	&nbsp;&nbsp;&nbsp;&nbsp;"sdk_metadata_passthrough": "OPTIONAL: string- any metadata a customer wants to pass directly through the sdk for later use (e.g. a json, xml,text as a string) ",<br>
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}
</code>


### Result:

### Response 200

Headers

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_length": 12324,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_description": "Where the worlds leading brands unlock the opportunities of the internet.
You'll meet 2,000 Attendees from 30+ Countries and can network with innovators from brands big and small.
Don't miss out on the most productive event for your online business.",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "akamai_internal",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_video_height": "720",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"thumb_height": "720",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_title": "Akamai Edge 2016",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"content_expiration": null,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"preselected": null,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"persistToExpiration": null,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"publication_timestamp": "2016-08-09 00:00:00",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_tags": [<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"text": "stehd",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"weight": "2"<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;&nbsp;&nbsp;],<br>
&nbsp;&nbsp;&nbsp;&nbsp;"thumb_width": "1024",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"long_lived": true,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_type": "video/m3u8",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_categories": [<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Entertainment"<br>
&nbsp;&nbsp;&nbsp;&nbsp;],<br>
&nbsp;&nbsp;&nbsp;&nbsp;"thumb_url": "http://tools.watch-now.co/a-lL66tBZZA/video.jpg",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_video_width": "1024",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_unique_id": "a-lL66tBZZA",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_bit_rate": 4096,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"thumb_length": 1280,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"streams": [<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": "http://tools.watch-now.co/a-lL66tBZZA/index.m3u8",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type": "m3u8",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"lowcost_map": {<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url": null,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"use": false,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host": null<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"size": "12324"<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;&nbsp;&nbsp;],<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_duration": 65,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"ad_url": null<br>
}
</code>


## GETTING CONTENT STATUS

> Example code for Getting Content Status:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getActivityStatusById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_id\": \"346708\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```

```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/getActivityStatusById"
```

The status attribute associated with all the content that is used by the PCD application, enables you to activate and deactivate content without having to add and delete content from the PCD application. You can both obtain the content status and activate the content by ID.

This call enables you to determine the active state of content using the ID.

To get the activity status of content using a specific ID, use the following command:

<code> POST/ingest/v1/getActivityStatusById </code>

URI Parameters: content_provider_id,access_token,content_id

Parameter | Required | Description
--------- | -------- | -----------
access_token | true | Access token associated with each provider
content_provider_id |true | ID associated with each provider
content_id  | true | ID of the content created by the provider



### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code |  Description
----------- | ---------- | -------------
200 | Active | Success. Request completed. The content is active.
200 | Inactive | Success. Request completed. The content is inactive.
200 | Purged | Success. The content is purged from the PCD application.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | The provided content ID is not associated with any content.
500 | Unknown | An unknown error occurred while processing the request.

### Request Get status of content

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "the id of content",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}
</code>


### Result:

### Response200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Active"<br>
}
</code>

## ACTIVATING CONTENT

> Example code for Activating Content:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/setContentActiveById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_id\": \"346708\"}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```

```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/setContentActiveById"
```

This call enables you to set a given content status as ACTIVE.

Using the following command, you can set a given content status to ACTIVE:

<code> POST/ingest/v1/setContentActiveById </code>

URI Parameters: content_provider_id,access_token,content_id

Parameter | Required | Description
--------- | -------- | -----------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider
content_id | true | ID of the content created by the provider



### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Active | Success. Request completed.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | The content ID is not associated with any content.
403 | Already active | The content is already marked as active.
403 | Content is purged from PVOC | The content does not reside on the PCD application.
500 | Unknown | An unknown error occurred while processing the request.


### Request Set content as Active

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "the id of content",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 400

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}
</code>

### Result:

### Response 200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>

## SETTING CONTENT AS INACTIVE

> Example Code Setting Content As Inactive:

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
CURLOPT_URL => "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/setContentInactiveById",
CURLOPT_RETURNTRANSFER => true,
CURLOPT_CUSTOMREQUEST => "POST",
CURLOPT_POSTFIELDS => "{\"content_provider_id\": \"akamai_internal\",\"access_token\": \"B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE\",\"content_id\": \"346708\",}",
CURLOPT_HTTPHEADER => array(
"content-type: application/json"
)
));

$response = curl_exec($curl);
?>
```

```shell
curl -X POST -H "Content-Type: application/json" -d '{
"content_provider_id": "akamai_internal",
"access_token": "B5672A98CB9A989755C5D419296237F5E00F9D759F4533B5E6E2541C88798ABE",
"content_id":346708
}' "https://ingest-sdktrial2.pvoc-anaina.com/ingest/v1/setContentInactiveById"
```

This call enables you to set a specific content status as INACTIVE.

Using the following command, set a given content status to INACTIVE:

<code> POST/ingest/v1/setContentInactiveById </code>

URI Parameters: content_provider_id,access_token,content_id

Parameter | Required | Description
--------- | -------- | ------------
access_token | true | Access token associated with each provider
content_provider_id | true | ID associated with each provider
content_id | true | ID of the content created by the provider



### RETURN CODES

The following table provides a list of possible return codes:

HTTP Status | Error Code | Description
----------- | ---------- | -----------
200 | Active | Success. Request completed.
400 | Missing data | The request JSON is missing essential data to complete the request.
401 | Access denied | The access token validation failed.
403 | Bad content id | Content ID is not associated with any content.
403 | Aready inactive | The content is already marked as active.
403 | Content is purged from PVOC | The content does not reside on the PCD application.
500 | Unknown | An unknown error occurred while processing the request.

### Request Set content as Inactive

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_id": "the id of content",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"content_provider_id": "the id of the content provider",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"access_token": "access token as provisioned by the MNO portal"<br>
}
</code>

### Response 200

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "a return code here"<br>
}
</code>

### Response 500

Headers

Content-Type: application/json

<code>
Body<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Unknown"<br>
}
</code>


### Result:

### Response 200

HEADERS

Content-Type:application/json

<code>
Body:<br>
{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"return_code": "Success"<br>
}
</code>

 # API TOOL
<a href="http://enterprisesmail.com/akamai_API/tool.html"> API TOOL </a>