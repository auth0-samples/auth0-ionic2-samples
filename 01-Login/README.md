# Auth0 - Ionic 2 Login

## Auth0 Dashboard Setup

Before you get started, you should head to your Auth0 dashboard and configure a few client settings:

### Callback URL

The Callback URL is used to send authentication data back to your app after logging in with Auth0. It is in the format:

`YOUR_PACKAGE_ID://YOUR_AUTH0_DOMAIN/cordova/YOUR_PACKAGE_ID/callback`

Where:


* `YOUR_PACKAGE_ID` is the app identifier. Ex: `io.ionic.starter.auth0`.
* `YOUR_AUTH0_DOMAIN` is your Auth0 tenant name. Ex: `<tenant>.auth0.com`.

### CORS

You need to set up CORS (Cross Origin Resource Sharing) in your dashboard so Auth0 will actually accept your requests. To make that work, you'll want to add `file://*` to your CORS settings - that will make it work for all requests from within the app.

### Client Type

You'll need to set the Client Type to `Native` so Auth0 knows what kind of requests to expect. Even though this is a web app packaged as a native one, you'll need to set it to Native here.

## Install Dependencies

If you're using this sample as a base for your project, you can just run `npm install`, but if you're setting up your own Ionic project, install these dependencies:


* `angular2-jwt`
* `auth0-js`
* `@auth0/cordova`

Here they are as one install command, for convenience:

`npm install angular2-jwt auth0-js @auth0/cordova --save`

## Install Cordova Plugins

You'll need a few cordova plugins to get `@auth0/cordova` to work. You can add them like so:


* `ionic cordova plugin add cordova-plugin-safariviewcontroller`
* `ionic cordova plugin add cordova-plugin-customurlscheme --variable URL_SCHEME={YOUR_PACKAGE_ID} --variable ANDROID_SCHEME={YOUR_PACKAGE_ID} --variable ANDROID_HOST={YOUR_AUTH0_DOMAIN} --variable ANDROID_PATHPREFIX=/cordova/{YOUR_PACKAGE_ID}/callback`

The variables used when installing `cordova-plugin-customurlscheme` are the same from when you set up the Callback URL in your Auth0 Dashboard.

### Reinstalling Plugins

Instead of changing plugin configuration manually, it is better to re-install the plugin entirely. First, do a remove: `ionic cordova plugin remove <plugin>` then re-add it as done above.

## Set AndroidLaunchMode 

In your `config.xml` file, add this preference:
```
 <preference name="AndroidLaunchMode" value="singleTask" />
```

## Set Auth0 Variables

If you're using this sample as a base for your project, edit `src/services/auth.service.ts`. Otherwise, copy it to your own project. You'll need to change each variable:

If you downloaded this sample from Auth0's docs page, it will come pre-populated with the keys for your client. If you are cloning it directly from Github, you will need to supply the keys yourself. The necessary keys are `clientId`, `domain`, and `packageIdentifier` which are all set in `src/services/auth.service.ts`.

## Creating an Authentication Service

If you're using this sample as a base for your project, you don't have to do anything. Otherwise, you should copy `src/services/auth.service.ts` to your own project. This service will handle basic login, logout, authentication checking, and token refreshing for you, and can be extended easily. Don't forget to add this service to your `app.module.ts`!

## Set Up Auth0-Cordova

In your `app.component.ts` file, add this import: `import Auth0Cordova from '@auth0/cordova';`, and, in your `platform.ready()` callback, add this chunk of code:

```
(<any>window).handleOpenURL = (url) => {
  Auth0Cordova.onRedirectUri(url);
};
```

This will tell the Auth0Cordova library to handle url redirects created by `cordova-plugin-customurlscheme`, and ultimately get users back into your application.

## Running the app

First, make sure you have a platform added: `ionic cordova platform add android` or `ionic cordova platform add ios`. Then, you can do either `ionic cordova emulate <platform>` or `ionic cordova run <platform>`, depending on if you want to start the project in an emulator or run it on a real device, respectively.
