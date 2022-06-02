---
layout: default
title: Quick Start
nav_order: 2
---

# Quick start
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

Use this Quick Start guide to get you up and running with a working chat hosted by your App.   

## System Requirements  

* Gradle 7.2 or higher.
* Android Studio.
* minSdk 21. 

## Set up the SDK on your App.
{: .d-inline-block }

1. ### Import SDK artifacts 
    {: .no_toc .strong-sub-title}   
    
    Chat SDK constructed of 4 modules. In order to be able to use the SDK those modules should be imported in the Apps build.gradle file.

    [Check here for latest SDK version release notes to find out how to import the SDKs artifacts]({{'/docs/release-notes#dependencies' | relative_url}}) 

    
2. ### Extra configurations on build.gradle:
    {: .no_toc .strong-sub-title}  
   
    Add the following to the _`android{...}`_ block definition

    ```gradle
    android {
        kotlinOptions {
            freeCompilerArgs = ['-Xjvm-default=compatibility']
            jvmTarget = "1.8"
        }

        //The following should be added in case build fail with the error: "More than one file was found with OS independent path..."
        packagingOptions {
            resources {
                pickFirsts += ['META-INF/DEPENDENCIES', 'META-INF/main_debug.kotlin_module', 'META-INF/main_release.kotlin_module', 'META-INF/main_sdktesting.kotlin_module', 'META-INF/ui_debug.kotlin_module', 'META-INF/ui_release.kotlin_module']
            }
        }
    }
    ```
    {: .mb-6}


    - If your app is configured with shrink and minify turned on, please read [Shrink and Obfuscattion support]({{'/docs/faq/shrink-and-obfuscate' | relative_url}}) and make shure the relevant rules are provided on the proguard file.
    
---

#### 👍  Now you are ready to start using the SDK capabilities.
{: .no_toc .strong-sub-title}

---

## Create and start a basic chat  
{: .d-inline-block .mt-4}

Follow the next steps in order to have a basic working chat integrated to your App.

1. ### Create an Account
    Chat basic details are provided over an account.
    Create the relevant account object and fill the relevant details needed for the chat creation.
    - [Create a `MessengerAccount`]({{'/docs/chat-configuration/chat-account/messenger-chat#messengeraccount' | relative_url}})
    {: .no_toc }
     
---

2. ### Create [ChatController]({{'/docs/chat-configuration/extra/chatcontroller' | relative_url}})
    With the ChatController one can create and control multiple chats.
    The chat type is configured by the Account that is provided on chat creation.

    ```kotlin
    val chatController = ChatController.Builder(context)
                                          .build(account, ...)
    ...
    // start a new chat, using same chatController:
    chatController.startChat(account)

    // restore active chat or starts new chat
    chatController.restoreChat(fragment?, account?)
    ```
---

3. ### Add the chat fragment to your activity.

    Implement the ChatLoadedListener interface and pass it in the `ChatController.Builder` build method.   
    Once the chat build succeeded and the fragment is ready to be displayed, `onComplete` will be called with the fragment on the result. 

    ```kotlin
    ChatController.Builder(context).build(account, object : ChatLoadedListener {
            override fun onComplete(result: ChatLoadResponse) {
                result.error?.run{
                  // report Chat load failed
                } ?: run{
                  // add result.getFragment() to the applications activity.
                }
            }
        })
    ```
---

##### **Notice** 
{: .no_toc } 
> Make sure to activate `ChatController.destruct()`, when the ChatController instance is no longer needed (e.g., Chats UI in app is being closed or the ChatController instance is being replaced). Destruct will verify resources release (including open sockets and communication channels).  

---

### Code Sample
{: .no_toc .text-delta}
[bold360ai samples](https://github.com/bold360ai/bold360-mobile-samples-android)
-