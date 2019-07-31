# Allow-Other-Apps-to-Start-Your-Activity
This Tutorial will discuss how to allow other applications to start your application and how you could benefit from this behavior. This is a part of Android App Links Series.

### This Tutorial discusses the following:
1. Why should I allow other Apps to open my App ?
2. What is Intent & Intent-Filter ?
3. Understand How Android System Deals with Implicit Intents
4. Allow Other Apps To Open A Certain Activity in Your App

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
