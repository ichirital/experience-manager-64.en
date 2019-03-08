---
title: Set up environment for AEM Forms app
seo-title: Set up environment for AEM Forms app
description: Hardware, software, and licenses to build and deploy the AEM Forms app.
seo-description: Hardware, software, and licenses to build and deploy the AEM Forms app.
uuid: da334355-c76f-4ce5-b20f-1c9ea99a7931
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: forms-app
discoiquuid: 347ce743-a2a6-4e4b-a1b7-090f1106343c
index: y
internal: n
snippet: y
---

# Set up environment for AEM Forms app{#set-up-environment-for-aem-forms-app}

You need the following hardware, software, and licenses to build and deploy the AEM Forms app:

## For Windows devices {#for-windows-devices}

* Microsoft Windows 8.1 or Windows 10
* Microsoft Visual Studio 2015
* Microsoft Visual Studio Tools for Apache Cordova

## For iOS devices {#for-ios-devices}

* Intel-based Apple Mac running Mac OS X 10.9.5 or above
* iOS SDK 8.4 or above
* Xcode version: Xcode 6.4 for OS X or above
* Membership of the iOS Developer Enterprise program
* Enterprise certificate for distributing in-house iOS apps
* Apple iPad with iOS 8.4 or later

## For Android devices {#for-android-devices}

* Android Development Toolkit (ADT bundle) that can be downloaded from [http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)
* If the environment is set up on a MAC system, the ADT should be installed in the Applications folder.
* If the ADT is installed in any other location on MAC, or if the environment is set up on a Windows system, the ADT SDK path needs to be updated in `local.properties` file that is available in `src\android` folder in the extracted the source archive `mobileworkspace-src.zip`. In this file, point the `sdk.dir` variable to ADT SDK location on your desktop.

>[!NOTE]
>
>The adobe-lc-mobileworkspace-src.zip contains PhoneGap SDK 5.0. Ensure that PhoneGap SDK is not pre-installed.

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

<!--
<related-links>
<a href="../../forms/using/setup-environment-mobile-workspace.md" target="_blank">Set up your environment</a>
<a href="../../forms/using/setup-xcode-project-build-installer.md" target="_blank">Set up the Xcode project and build the iOS app</a>
<a href="../../forms/using/setup-eclipse-project-build-installer.md" target="_blank">Set up the Eclipse project and build the Android app</a>
<a href="../../forms/using/setup-visual-studio-project-build-installer.md" target="_blank">Set up the Visual Studio project and build the Windows app</a>
<a href="../../forms/using/distribute-mobile-workspace-app.md" target="_blank">Distribute the AEM Forms app</a>
<a href="../../forms/using/building-secure-mobile-workspace-app.md" target="_blank">Building a secure AEM Forms app for iOS</a>
</related-links>
-->
