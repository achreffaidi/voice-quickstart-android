# Twilio Voice Quickstart for Android

> This is a developer preview release of the Programmable Voice 3.X SDK for Android. This major version now uses WebRTC and is still under active development. APIs are subject to change and we recommend you look at known issues provided in the [changelog](https://www.twilio.com/docs/voice/voip-sdk/android/changelog). To use a generally available version of the Programmable Voice SDKs for Android, please see the master branch based on the 2.X APIs.

Get started with Voice on Android:

- [Quickstart](#quickstart) - Run the quickstart app
- [Migration Guide](#migration-guide) - Migrating from 2.X to 3.X
- [Emulator Support](#emulator-support) - Android emulator support
- [Reducing APK Size](#reducing-apk-size) - Use ABI splits to reduce APK size
- [Access Tokens](#access-tokens) - Using access tokens
- [More Documentation](#more-documentation) - More documentation related to the Voice Android SDK
- [Twilio Helper Libraries](#twilio-helper-libraries) - TwiML quickstarts.
- [Issues & Support](#issues-and-support) - Filing issues and general support

## Quickstart

To get started with the Quickstart application follow these steps. Steps 1-6 will allow you to make a call. The remaining steps 7-8 will enable push notifications using FCM.

1. [Open this project in Android Studio](#bullet1)
2. [Create a Voice API key](#bullet2)
3. [Configure a server to generate an access token to use in the app](#bullet3)
4. [Create a TwiML application](#bullet4)
5. [Configure your application server](#bullet5)
6. [Run the app](#bullet6)
7. [Generate google-services.json](#bullet7)
8. [Add a Push Credential using your FCM Server API Key](#bullet8)
9. [Receiving an Incoming Notification](#bullet9)
10. [Make client to client call](#bullet10)
11. [Make client to PSTN call](#bullet11)


### <a name="bullet1"></a>1. Open the project in Android Studio
<img width="700px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/import_project.png"/>

### <a name="bullet2"></a>2. Create a Voice API key

Go to the [API Keys page](https://www.twilio.com/console/voice/settings/api-keys) and create a new API key.

<img width="700px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/add_api_key.png"/>

Save the generated `API_KEY` and `API_KEY_SECRET` in your notepad. You will need them in the next step.


### <a name="bullet3"></a>3. Configure a server to generate an access token to use in the app

Download one of the starter projects for the server.

* [voice-quickstart-server-java](https://github.com/twilio/voice-quickstart-server-java)
* [voice-quickstart-server-node](https://github.com/twilio/voice-quickstart-server-node)
* [voice-quickstart-server-php](https://github.com/twilio/voice-quickstart-server-php)
* [voice-quickstart-server-python](https://github.com/twilio/voice-quickstart-server-python)

Follow the instructions in the server's README to get the application server up and running locally and accessible via the public Internet. For now just add the Twilio Account SID that you can obtain from the console, and  the `API_KEY` and `API_SECRET` you obtained in the previous step. 

    ACCOUNT_SID = 'AC***'
    API_KEY = 'SK***'
    API_KEY_SECRET = '***'

### <a name="bullet4"></a>4. Create a TwiML application

Next, we need to create a TwiML application. A TwiML application identifies a public URL for retrieving TwiML call control instructions. When your Android app makes a call to the Twilio cloud, Twilio will make a webhook request to this URL, your application server will respond with generated TwiML, and Twilio will execute the instructions you’ve provided.

To create a TwiML application, go to the TwiML app page. Create a new TwiML application, and use the public URL of your application server’s `/makeCall` endpoint as the Voice Request URL (If your app server is written in PHP, then you need `.php` extension at the end).

<img width="700px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/create_twml_app.png"/>

As you can see we’ve used our ngrok public address in the Request URL field above.

Save your TwiML Application configuration, and grab the TwiML Application SID (a long identifier beginning with the characters "AP").

### <a name="bullet5"></a>5. Configure your application server

Put the remaining `APP_SID` configuration info into your application server by setting the following constants with the information you gathered above.

    ACCOUNT_SID = 'AC***'
    API_KEY = 'SK***'
    API_KEY_SECRET = '***'
    APP_SID = 'AP***'

Once you’ve done that, restart the server so it uses the new configuration info. Now it's time to test.

Open up a browser and visit the URL for your application server's Access Token endpoint: `https://{YOUR_SERVER_URL}/accessToken` (If your app server is written in PHP, then you need `.php` extension at the end). If everything is configured correctly, you should see a long string of letters and numbers, which is a Twilio Access Token. Your Android app will use a token like this to connect to Twilio.

### <a name="bullet6"></a>6. Run the app

Paste the public URL of your application server’s `https://{YOUR_SERVER_URL}/accessToken` endpoint into `TWILIO_ACCESS_TOKEN_SERVER_URL` in VoiceActivity.java. Make sure to include `/accessToken` in the URL path.

<img width="600px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/paste_token_server_url.png"/>

Run the quickstart app on an Android device

<img width="423px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/voice_activity.png">

Press the call button to open the call dialog.

<img width="423px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/voice_make_call_dialog.png">

Leave the dialog text field empty and press the call button to start a call. You will hear the congratulatory message. Support for dialing another client or number is described in steps 10 and 11.

<img width="423px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/voice_make_call.png">

### <a name="bullet7"></a>7. Generate `google-services.json`

The Programmable Voice Android SDK uses Firebase Cloud Messaging push notifications to let your application know when it is receiving an incoming call. If you want your users to receive incoming calls, you’ll need to enable FCM in your application.

Follow the steps under **Use the Firebase Assistant** in the [Firebase Developers Guide](https://firebase.google.com/docs/android/setup). Once you connect and sync to Firebase successfully, you will be able to download the `google-services.json` for your application. 

Login to Firebase console and make a note of generated `Server API Key` and `Sender ID` in your notepad. You will need them in the next step.

<img width="700px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/server_key_sender_id.png">"

Make sure the generated `google-services.json` is downloaded to the `app` directory of the quickstart project to replace the existing `app/google-services.json` stub json file. If you are using the Firebase plugin make sure to remove the stub `google-services.json` file first.

As a final step re-run the application from Android Studio to ensure the APK now has the latest `google-services.json` file.

### <a name="bullet8"></a>8. Add a Push Credential using your FCM `Server API Key` 

You will need to store the FCM `Server API Key` with Twilio so that we can send push notifications to your app on your behalf. Once you store the API Key with Twilio, it will get assigned a Push Credential SID so that you can later specify which key we should use to send push notifications.

Go to the [Push Credentials page](https://www.twilio.com/console/voice/sdks/credentials) and create a new Push Credential.

Paste in the `Server API Key` and press Save.

<img width="700px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/add_fcm_push_cred.png">"

### <a name="bullet9"></a>9. Receiving an Incoming Notification

Put the `PUSH_CREDENTIAL_SID` configuration info into your application server by setting the following constants with the information you gathered above.

    ACCOUNT_SID = 'AC***'
    API_KEY = 'SK***'
    API_KEY_SECRET = '***'
    PUSH_CREDENTIAL_SID = 'CR***'
    APP_SID = 'AP***'

Once you’ve done that, restart the server so it uses the new configuration info. Now it's time to test. Use your browser to initiate an incoming call by navigating to the public URL of your application server’s `https://{YOUR_SERVER_URL}/placeCall` endpoint (If your app server is written in PHP, then you need `.php` extension at the end). This will trigger a Twilio REST API request that will make an inbound call to your mobile app.
Your application will be brought to the foreground and you will see an alert dialog. The app will be brought to foreground even when your screen is locked.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/incoming_call.png">"

You will receive an incoming call notification as well. If you pull down the notification drawer, you will be able to view the notification.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/incoming_notification.png">"

Once your app accepts the call, you should hear a congratulatory message.

### <a name="bullet10"></a>10. Make client to client call

To make client to client calls, you need the application running on two devices. To run the application on an additional device, make sure you use a different identity in your access token when registering the new device. For example, change the `identity` field to `bob` and run the application. 

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/access_token_identity_bob.png">

Press the call button to open the call dialog.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/voice_make_call_dialog.png">

Enter the client identity of the newly registered device to initiate a client to client call from the first device.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/make_call_to_client.png">
<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/incoming_call_from_alice.png">

### <a name="bullet11"></a>11. Make client to PSTN call

A verified phone number is one that you can use as your Caller ID when making outbound calls with Twilio. This number has not been ported into Twilio and you do not pay Twilio for this phone number.

To make client to number calls, first get a valid Twilio number to your account via https://www.twilio.com/console/phone-numbers/verified. Update your server code and replace `CALLER_NUMBER` with the verified number. Restart the server so that it uses the new value.

Press the call button to open the call dialog.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/voice_make_call_dialog.png">

Enter a PSTN number and press the call button to place a call.

<img height="500px" src="https://github.com/twilio/voice-quickstart-android/raw/master/images/quickstart/make_call_to_number.png">

## Migration Guide

This section describes API changes and additions to ease migration from Voice Android 2.X to Voice Android 3.X. Each section provides code snippets to assist in transitioning to the new API.

#### Call State

The call state `CallState` has moved to the Call class. It can be referenced as follows:

```Java
Call.State
```

#### CallInvite Changes

In Voice Android 3.X, the `MessageListener` no longer raises errors if an invalid message is provided, instead a `boolean` value is returned when `boolean handleMessage(context, data, listener)` is called. The `boolean` value returns `true` when the provided data resulted in a `CallInvite` or `CancelledCallInvite` raised by the `MessageListener`. If `boolean handleMessage(context, data, listener)` returns `false` it means the data provided was not a Twilio Voice push message.

The `MessageListener` now raises callbacks for a `CallInvite` or `CancelledCallInvite` as follows:

```Java
boolean valid = handleMessage(context, data, new MessageListener() {
           @Override
           void onCallInvite(CallInvite callInvite) {
               // Show notification to answer or reject call
           }

           @Override
           void onCancelledCallInvite(CancelledCallInvite callInvite) {
               // Hide notification
           }
       });
```

The `CallInvite` has an `accept()` and `reject()` method. The `getState()` method has been removed from the `CallInvite` in favor of distinguishing between call invites and call invite cancellations with discrete stateless objects. While the `CancelledCallInvite` simply provides the `to`, `from`, and `callSid` fields also available in the `CallInvite`. The class method `getCallSid()` can be used to associate a `CallInvite` with a `CancelledCallInvite`.

#### Specifying a media region

Previously, a media region could be specified via `Voice.setRegion(String)`. Now this configuration can be provided as part of `ConnectOptions` or `AcceptOptions` as shown below:

```Java
ConnectOptions connectOptions = new ConnectOptions.Builder()
            .region(String)
            .build();

AcceptOptions acceptOptions = new AcceptOptions.Builder()
            .region(String)
            .build();
```

#### ConnectOptions & AcceptOptions

To support configurability upon making or accepting a call new classes have been added. To create `ConnectOptions` the `ConnectOptions.Builder` must be used. Once `ConnectOptions` is created it can be provided when connecting  a `Call` as shown below:

```Java
ConnectOptions connectOptions = new ConnectOptions.Builder(accessToken)
            .setParams(params)
                    .build();
Call call = Voice.connect(context, connectOptions, listener);
```

A `CallInvite` can also be accepted using `AcceptOptions` as shown below:

```Java
AcceptOptions acceptOptions = new AcceptOptions.Builder()
            .build();
CallInvite.accept(context, acceptOptions, listener);
```

#### Ringing

Ringing is now provided as a call state. A callback corresponding to this state transition is emitted once before the `Call.Listener.onConnected(...)` callback when the callee is being alerted of a Call. The behavior of this callback is determined by the `answerOnBridge` flag provided in the `Dial` verb of your TwiML application associated with this client. If the `answerOnBridge` flag is `false`, which is the default, the `Call.Listener.onConnected(...)` callback will be emitted immediately after `Call.Listener.onRinging(...)`. If the `answerOnBridge` flag is `true`, this will cause the call to emit the onConnected callback only after the call is answered. See [answerOnBridge](https://www.twilio.com/docs/voice/twiml/dial#answeronbridge) for more details on how to use it with the Dial TwiML verb. If the TwiML response contains a Say verb, then the call will emit the `Call.Listener.onConnected(...)` callback immediately after `Call.Listener.onRinging(...)` is raised, irrespective of the value of `answerOnBridge` being set to `true` or `false`.

These changes are added as follows:

```Java
public class Call {

	public enum State {
		CONNECTING,
		RINGING, // State addition
		CONNECTED,
		DISCONNECTED
	}

	public interface Listener {
	   void onConnectFailure(@NonNull Call call, @NonNull CallException callException);
		void onRinging(@NonNull Call call); // Callback addition
		void onConnected(@NonNull Call call);
		void onDisconnected(@NonNull Call call, @Nullable CallException callException);
	}
}

```

#### Hold

Previously, there was no way to hold a call. Hold can now be called on the `Call` object as follows:

```Java
call.hold(boolean);
```

#### Media Establishment & Connectivity

The Voice Android 3.X SDK uses WebRTC. The exchange of real-time media requires the use of Interactive Connectivity Establishment(ICE) to establish a media connection between the client and the media server. In some network environments where network access is restricted it may be necessary to provide ICE servers to establish a media connection. We reccomend using the [Network Traversal Service (NTS)](https://www.twilio.com/stun-turn) to obtain ICE servers. ICE servers can be provided when making or accepting a call by passing them into `ConnectOptions` or `AcceptOptions` in the following way:

```Java
// Obtain the set of ICE servers from your preferred ICE server provider
Set<IceServer> iceServers = new HashSet<>();
iceServers.add(new IceServer("stun:global.stun.twilio.com:3478?transport=udp"));
iceServers.add(new IceServer("turn:global.turn.twilio.com:3478?transport=udp"));
...

IceOptions iceOptions = new IceOptions.Builder()
         .iceServers(iceServers)
         .build();

ConnectOptions.Builder connectOptionsBuilder = new ConnectOptions.Builder(accessToken)
         .iceOptions(iceOptions)
         .build();
```

### Custom Parameters

Custom Parameters is now supported in `CallInvite`. `CallInvite.getCustomParamaters()` returns a map of custom parameters sent from the caller side to callee.

```Java
// Pass custom parameters in TwiML
<?xml version="1.0" encoding="UTF-8"?>
	<Response>
		<Dial answerOnBridge="false" callerId="client:alice">
			<Client>
				<Identity>bob</Identity>
				<Parameter name="caller_first_name" value="alice"  />
				<Parameter name="caller_last_name" value="smith"  />
			</Client>
		</Dial>
	</Response>
```  
`callInvite.getCustomParameters()` returns a map of key-value pair passed in the TwiML.

```
"caller_first_name" -> "alice"
"caller_last_name" -> "smith"
```

## Emulator Support

The SDK supports using emulators except in the following known cases:

1. Emulators with API 22 or lower have bad audio emulation, the sound is generally inaudible
2. Emulators must have Google Play services support to use FCM to receive call invites
3. Running on x86 API 25 emulators results in application crashes

In general we advise using a real device when doing development with our SDK since real-time audio is a performance oriented operation.

## Reducing APK Size

Our library is built using native libraries. As a result, if you use the default gradle build you will generate an APK with all four architectures(armeabi-v7a, arm64-v8a, x86, x86_64) in your APK.

[APK splits](https://developer.android.com/studio/build/configure-apk-splits.html) allow developers to build multiple APKs for different screen sizes and ABIs. Enabling APK splits ensures that the minimum amount of files required to support a particular device are packaged into an APK.

The following snippet shows an example `build.gradle` with APK splits enabled.

    apply plugin: 'com.android.application'

    android {
        // Specify that we want to split up the APK based on ABI
        splits {
            abi {
                // Enable ABI split
                enable true

                // Clear list of ABIs
                reset()

                // Specify each architecture currently supported by the Voice SDK
                include "armeabi-v7a", "arm64-v8a", "x86", "x86_64"

                // Specify that we do not want an additional universal SDK
                universalApk false
            }
        }
    }

The adoption of APK splits requires developers to submit multiple APKs to the Play Store. Refer to [Google’s documentation](https://developer.android.com/google/play/publishing/multiple-apks.html)  for how to support this in your application.

## Access Tokens

The access token generated by your server component is a [jwt](https://jwt.io) that contains a `grant` for Programmable Voice, an `identity` that you specify, and a `time-to-live` that sets the lifetime of the generated access token. The default `time-to-live` is 1 hour and is configurable up to 24 hours using the Twilio helper libraries.

### Uses

In the Android SDK the access token is used for the following:

1. To make an outgoing call via `Voice.call(Context context, String accessToken, String twiMLParams, Call.Listener listener)`
2. To register or unregister for incoming notifications via GCM or FCM via `Voice.register(Context context, String accessToken, Voice.RegistrationChannel registrationChannel, String registrationToken, RegistrationListener listener)` and `Voice.unregister(Context context, String accessToken, Voice.RegistrationChannel registrationChannel, String registrationToken, RegistrationListener listener)`. Once registered, incoming notifications are handled via a `CallInvite` where you can choose to accept or reject the invite. When accepting the call an access token is not required. Internally the `CallInvite` has its own accessToken that ensures it can connect to our infrastructure.

### Managing Expiry

As mentioned above, an access token will eventually expire. If an access token has expired, our infrastructure will return error `EXCEPTION_INVALID_ACCESS_TOKEN_EXPIRY`/`20104` via a `CallException` or a `RegistrationException`.

There are number of techniques you can use to ensure that access token expiry is managed accordingly:

- Always fetch a new access token from your access token server before making an outbound call.
- Retain the access token until getting a `EXCEPTION_INVALID_ACCESS_TOKEN_EXPIRY`/`20104` error before fetching a new access token.
- Retain the access token along with the timestamp of when it was requested so you can verify ahead of time whether the token has already expired based on the `time-to-live` being used by your server.
- Prefetch the access token whenever the `Application`, `Service`, `Activity`, or `Fragment` associated with an outgoing call is created.

## More Documentation

You can find more documentation on getting started as well as our latest Javadoc below:


* [Getting Started](https://www.twilio.com/docs/api/voice-sdk/android/getting-started)
* [Javadoc](https://media.twiliocdn.com/sdk/android/voice/releases/3.0.0-preview1/docs/)

## Twilio Helper Libraries

To learn more about how to use TwiML and the Programmable Voice Calls API, check out our TwiML quickstarts:

* [TwiML Quickstart for Python](https://www.twilio.com/docs/quickstart/python/twiml)
* [TwiML Quickstart for Ruby](https://www.twilio.com/docs/quickstart/ruby/twiml)
* [TwiML Quickstart for PHP](https://www.twilio.com/docs/quickstart/php/twiml)
* [TwiML Quickstart for Java](https://www.twilio.com/docs/quickstart/java/twiml)
* [TwiML Quickstart for C#](https://www.twilio.com/docs/quickstart/csharp/twiml)

## Issues and Support

Please file any issues you find here on Github.
For general inquiries related to the Voice SDK you can file a support ticket.
Please ensure that you are not sharing any
[Personally Identifiable Information(PII)](https://www.twilio.com/docs/glossary/what-is-personally-identifiable-information-pii)
or sensitive account information (API keys, credentials, etc.) when reporting an issue.

## License
MIT
