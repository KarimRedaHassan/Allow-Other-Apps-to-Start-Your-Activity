# Allow-Other-Apps-to-Start-Your-Activity
This Tutorial will discuss how to allow other applications to start your application and how you could benefit from this behavior.

### This Tutorial discusses the following:
1. Why should I allow other Apps to open my App ?
2. What is Intent & Intent-Filter ?
3. Understand How Android System Deals with Implicit Intents
4. Allow Other Apps To Open A Certain Activity in Your App
5. Available Actions, Categories, Data Types

### This is a part of Android App Links Series.

# Why Allowing Other Apps to start my App
The first question that should come up on your mind is Why should I allow other apps to open my App.

As an answer for this question, Allowing other apps to open your app actually benefits you more that the other app. Because it simply redirects users to your app and increases user engagement with your app. It also make it easier for the user himself to use your app. For Example, if you allow "Share Action" to your app, The user could simply grap a content from other apps (such as Facebook, gallery, ... whatever) by pressing the share button and post it in your app instead of writing the content again in your app.

# Intent & Intent-Filter
The main concept behind allowing other apps to open your app is the Intent & Intent-Filter. If you are asking what does it mean, Here is a snippet from the Official Android Documentation about the Intent "Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed"

https://developer.android.com/reference/android/content/Intent

So, The Intent is the main method to start an Activity which exactly what the other app will need to do "Start your App's Activity". 

There is Two types of intents: Explicit & Implicit. The Explicit Intent is the one you are using inside your app to navigate from one activity to another as you know the exact class name that you want to go to. The Implicit Intent is where you don't know the exact class name of your destination OR you don't mind any destination as long as it will perform the requested action, here comes the Intent-Filter.

The Intent Filter is the way that the activity identifies itself to the Android System. So, If the activity, for example, is used to share a post in a social app, This Activity will indentify itself by saying "Hey, I can handle a Share Action" or in more technical terms it will declare an Action_SEND in its manifest file.

So, If you want other apps to use your activities, You should declare what kind of actions that your activities could handle in your manifest file using the Intent-Filter. You could also specify the kind of data that your activity expect to recieve.

# Understand How Android System Deals with Implicit Intents
When some app makes an Implicit Intent, The Android System will scan all the Installed Applications and search in their Manifest Files for any Activity that could handle this specific action, category, and data type that is send along with the Implicit Intent by querying the PackageManager. Once the Android System finds some matching activities, Android System shows these activities in a Dialog so the user could choose the desired activity. If there is Only One Activity that could handle this action, Android System will automatically opens it. If no activity has been found, The Android System will return null to the requested app.

#### As We are in The Android App Links Series, It is worthy to be mentioned that
Here is one major difference between the Deep Link and Android App Link. If you are using a deep link, Android System will show the dialog mentioned in the above paragraph. If you are using Android App Link, The Android System will automatically opens your app showing this dialog.

# Allow Other Apps To Open A Certain Activity in Your App
First, allow other apps find & open a certain activity in your app by doing the following steps:
1. Declare an \<intent-filter> inside your \<activity> tag in the manifest file.
2. Specify the kind of \<action> that your activity could handle.
3. Specify the kind of \<category> that your activity could handle.
4. Specify the kind of \<data> that your activity expects.

Here is a code snippet for an activity allowing a Send Action

         <activity class=".YourActivityName">
             <intent-filter>
                    <action android:name="android.intent.action.SEND" />
                    <category android:name="android.intent.category.DEFAULT" />
                    <data android:mimeType="text/plain" />
             </intent-filter>
         </activity>


Second, prepare your activity to recieve the data sent by other app through the intent by doing the following steps:
1. Get a reference of the intent
2. Get the action sent along with the intent. You could ignore this if your activity recieves only one action.
3. Get the data type sent along with the intent by calling getType() on the intent reference.
2. Get the data sent along with the intent by calling getExtra() on the intent reference.
3. Perform your custom business logic to show or deal with the data recieved to meet the user's expectations

Here is a code snippet for an activity allowing more than one Action, This snippet has been taken from Android Documentation

https://developer.android.com/training/sharing/receive

        void onCreate (Bundle savedInstanceState) {
            ...
            // Get intent, action and MIME type
            Intent intent = getIntent();
            String action = intent.getAction();
            String type = intent.getType();

            if (Intent.ACTION_SEND.equals(action) && type != null) {
                if ("text/plain".equals(type)) {
                    handleSendText(intent); // Handle text being sent
                } else if (type.startsWith("image/")) {
                    handleSendImage(intent); // Handle single image being sent
                }
            } else if (Intent.ACTION_SEND_MULTIPLE.equals(action) && type != null) {
                if (type.startsWith("image/")) {
                    handleSendMultipleImages(intent); // Handle multiple images being sent
                }
            } else {
                // Handle other intents, such as being started from the home screen
            }
            ...
        }

        void handleSendText(Intent intent) {
            String sharedText = intent.getStringExtra(Intent.EXTRA_TEXT);
            if (sharedText != null) {
                // Update UI to reflect text being shared
            }
        }

        void handleSendImage(Intent intent) {
            Uri imageUri = (Uri) intent.getParcelableExtra(Intent.EXTRA_STREAM);
            if (imageUri != null) {
                // Update UI to reflect image being shared
            }
        }

        void handleSendMultipleImages(Intent intent) {
            ArrayList<Uri> imageUris = intent.getParcelableArrayListExtra(Intent.EXTRA_STREAM);
            if (imageUris != null) {
                // Update UI to reflect multiple images being shared
            }
        }

# Here is a list of available intent actions, categories, data types

### Intent Actions
- android.intent.action.ALL_APPS
- android.intent.action.ANSWER
- android.intent.action.APPLICATION_PREFERENCES
- android.intent.action.APP_ERROR
- android.intent.action.ASSIST
- android.intent.action.ATTACH_DATA
- android.intent.action.BUG_REPORT
- android.intent.action.CALL
- android.intent.action.CALL_BUTTON
- android.intent.action.CALL_EMERGENCY
- android.intent.action.CALL_PRIVILEGED
- android.intent.action.CARRIER_SETUP
- android.intent.action.CHOOSER
- android.intent.action.CREATE_DOCUMENT
- android.intent.action.CREATE_LIVE_FOLDER
- android.intent.action.CREATE_SHORTCUT
- android.intent.action.DELETE
- android.intent.action.DIAL
- android.intent.action.DISMISS_ALARM
- android.intent.action.DISMISS_TIMER
- android.intent.action.EDIT
- android.intent.action.EVENT_REMINDER
- android.intent.action.FACTORY_TEST
- android.intent.action.GET_CONTENT
- android.intent.action.INSERT
- android.intent.action.INSERT_OR_EDIT
- android.intent.action.INSTALL_FAILURE
- android.intent.action.INSTALL_INSTANT_APP_PACKAGE
- android.intent.action.INSTALL_PACKAGE
- android.intent.action.INSTANT_APP_RESOLVER_SETTINGS
- android.intent.action.MAIN
- android.intent.action.MANAGE_APP_PERMISSIONS
- android.intent.action.MANAGE_NETWORK_USAGE
- android.intent.action.MANAGE_PERMISSIONS
- android.intent.action.MANAGE_PERMISSION_APPS
- android.intent.action.MEDIA_SEARCH
- android.intent.action.MUSIC_PLAYER
- android.intent.action.OPEN_DOCUMENT
- android.intent.action.OPEN_DOCUMENT_TREE
- android.intent.action.PASTE
- android.intent.action.PICK
- android.intent.action.PICK_ACTIVITY
- android.intent.action.POWER_USAGE_SUMMARY
- android.intent.action.PROCESS_TEXT
- android.intent.action.QUICK_VIEW
- android.intent.action.REVIEW_PERMISSIONS
- android.intent.action.RINGTONE_PICKER
- android.intent.action.RUN
- android.intent.action.SEARCH
- android.intent.action.SEARCH_LONG_PRESS
- android.intent.action.SEND
- android.intent.action.SENDTO
- android.intent.action.SEND_MULTIPLE
- android.intent.action.SET_ALARM
- android.intent.action.SET_TIMER
- android.intent.action.SET_WALLPAPER
- android.intent.action.SHOW_ALARMS
- android.intent.action.SHOW_APP_INFO
- android.intent.action.SHOW_SUSPENDED_APP_DETAILS
- android.intent.action.SHOW_TIMERS
- android.intent.action.SNOOZE_ALARM
- android.intent.action.SYNC
- android.intent.action.SYSTEM_TUTORIAL
- android.intent.action.UNINSTALL_PACKAGE
- android.intent.action.UPGRADE_SETUP
- android.intent.action.VIEW
- android.intent.action.VIEW_DOWNLOADS
- android.intent.action.VOICE_ASSIST
- android.intent.action.VOICE_COMMAND
- android.intent.action.WEB_SEARCH

### Intent Categories
- android.intent.category.ALTERNATIVE
- android.intent.category.APP_BROWSER
- android.intent.category.APP_CALCULATOR
- android.intent.category.APP_CALENDAR
- android.intent.category.APP_CONTACTS
- android.intent.category.APP_EMAIL
- android.intent.category.APP_GALLERY
- android.intent.category.APP_MAPS
- android.intent.category.APP_MARKET
- android.intent.category.APP_MESSAGING
- android.intent.category.APP_MUSIC
- android.intent.category.BROWSABLE
- android.intent.category.CAR_DOCK
- android.intent.category.CAR_MODE
- android.intent.category.DEFAULT
- android.intent.category.DESK_DOCK
- android.intent.category.DEVELOPMENT_PREFERENCE
- android.intent.category.EMBED
- android.intent.category.HE_DESK_DOCK
- android.intent.category.HOME
- android.intent.category.INFO
- android.intent.category.LAUNCHER
- android.intent.category.LEANBACK_LAUNCHER
- android.intent.category.LE_DESK_DOCK
- android.intent.category.MONKEY
- android.intent.category.NOTIFICATION_PREFERENCES
- android.intent.category.OPENABLE
- android.intent.category.PREFERENCE
- android.intent.category.SELECTED_ALTERNATIVE
- android.intent.category.TAB
- android.intent.category.TYPED_OPENABLE
- android.intent.category.USAGE_ACCESS_CONFIG
- android.intent.category.VOICE
- android.intent.category.VR_HOME

### Intent Data Types
- audio/audioType, you could use * as subtype to indicate that all audio types are acceptable
- video/videoType, you could use * as subtype to indicate that all video types are acceptable
- image/imageType, you could use * as subtype to indicate that all image types are acceptable
- text/textType, you could use * as subtype to indicate that all text types are acceptable
- application/otherTypes
- */*, you could use */* to indicate that all media types are acceptable


# What's Next ?

Create-Deep-Links-to-Your-App-Content

https://github.com/KarimRedaHassan/Create-Deep-Links-to-Your-App-Content

# Android App Links Series

Understand-The-URL
- https://github.com/KarimRedaHassan/Understand-The-URL

Deep-Links-vs-Android-App-Links
- https://github.com/KarimRedaHassan/Deep-Links-vs-Android-App-Links

Allow-Other-Apps-to-Start-Your-Activity
- https://github.com/KarimRedaHassan/Allow-Other-Apps-to-Start-Your-Activity

Create-Deep-Links-to-Your-App-Content
- https://github.com/KarimRedaHassan/Create-Deep-Links-to-Your-App-Content

# Additional Resources

https://developer.android.com/training/basics/intents/filters.html

https://developer.android.com/training/basics/intents/filters.html#AddIntentFilter

https://developer.android.com/training/sharing/send

https://developer.android.com/training/sharing/receive

# Also See

#### A Full List Of All My Tutorials

https://github.com/KarimRedaHassan?tab=repositories
