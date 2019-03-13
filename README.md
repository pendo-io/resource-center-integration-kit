# Pendo Resource Center Integration Kit

The Pendo Resource Center provides an integration framework that allows third party developers to create applications that Pendo customers can activate in their web applications and provide to their users.

At a high level, an integration consists of a single JSON configuration file hosted by Pendo, and a web application (integration) hosted by the third party developer.  There is also a lightweight Javascript SDK (`pendo-extension-sdk`) that can be leveraged to communicate between the integration and the Pendo Resource Center to retrieve configuration information, invoke triggers, and listen for events from the Resource Center.

If you would like to create a Resource Center integration please contact integrations@pendo.io for more information.  Pendo can assist in creating the correct `pendo.json` configuration file based on the `pendo-template.json` example provided in this repo.

## pendo-extension-sdk

### _Getting the SDK_

There are multiple ways to leverage the `pendo-extension-sdk` from your project.  

* You can download the file and host it locally:

    ```bash
    $ wget https://pendo-io-extensions.storage.googleapis.com/sdk/0.1.0/js/pendo-extensions.sdk.js
    ```

* You can reference the file from a `script` tag in your web application:
    
    ```html
    <script src="https://pendo-io-extensions.storage.googleapis.com/sdk/0.1.0/js/pendo-extensions.sdk.js"></script>
    ```

* You can install the NPM package:

    ```bash
    $ npm install pendo-extension-sdk
    ```

### _Using the SDK_

The following code demonstrates retrieving configuration information associated with a particular installation of the extension.

#### Initialization

```javascript
const client = PendoSDK.getConnection("app-name");
client.initialize();

client.get('extension.config').then((result) => {
    const options = result.data.config.provider.options;
    const {apiKey, subdomain} = options;
    
    // do something with these customer specific configuration values here
    // e.g. redirect to URL and pass in apiKey as query parameter
});
```

#### Triggers

Triggers are used to issue a command to the outer frame agent of the Resource Center.

 * triggers.resizeHeader - Takes integer argument of pixels to adjust the header height to. `0` hides the header.

    ```javascript
    client.triggers.resizeHeader(10);
    ```

 * triggers.updateNotificationBadge - Takes an integer argument and updates the notification indicator on the Guide Center badge with the number passed in.

    ```javascript
    client.triggers.updateNotificationBadge(3);
    ```

 * content.zoom - Displays an image in a shadow box overlaying the parent web application according to the passed in configuration object. 

   ```javascript
   client.content.zoom({
       type: 'IMG', // or 'PICTURE'
       top: TopNumberOfPixels, // Integer value
       left: LeftNumberOfPixels,
       width: width, // of the target element
       height: height, // of the target element
       src: src, // url of the image
       options: {
           transitionDuration: NumMilliSecs,
           margin: NumPixels
       }
   });
   ```

 * triggers.toggleLauncher

 * triggers.returnToMenu

   

#### Gets

Gets will ask the outer frame agent of the Resource Center for information and return that data via a Promise. These typically don't require arguments.

 * browser 
    * env - retrieve browser environment information
    ```javascript
    client.get('browser.env').then((result) => {...}
    ```
    * url - retrieve the current browser URL
    ```javascript
    client.get('browser.url').then((result) => {...}
    ```

 * metadata - return all agent metadata associated with the current visitor
    ```javascript
    client.get('metadata').then((result) => {...}
    ```

 * extension
    * config - retrieve customer-specific configuration information (e.g. API key)
    ```javascript
    client.get('extension.config').then((result) => {...}
    ```
    * stylesheet - retrieve customer-specific stylesheet information
    ```javascript
    client.get('extension.stylesheet').then((result) => {...}
    ```

      

#### Listeners 

_(not currently implemented)_

Listeners are used to set up a callback function in the extension app for whenever the events occur. Typically don't have arguments and the callbacks can be called many times.

 * events.onMenuItemClick
 * events.onToggleClick
