# Pendo Resource Center Integration Kit

The Pendo Resource Center provides an integration framework that allows third party developers to create applications that Pendo customers can activate and provide to their users.

At a high level, an integration consists of a single JSON configuration file hosted by Pendo, and a web application (integration) hosted by the third party developer.  There is also a lightweight Javascript SDK (`pendo-extension-sdk`) that can be leveraged to communicate between the integration and the Pendo Resource Center to retrieve configuration information, invoke triggers, and listen for events from the Resource Center.

If you would like to create a Resource Center integration please contact integrations@pendo.io for more information.  Pendo can assist in creating the correct `pendo.json` configuration file based on the `pendo-template.json` example provided in this repo.

## pendo-extension-sdk

### _Getting the SDK_

There are multiple ways to leverage the `pendo-extension-sdk` from your project.  

* You can download the file and host it locally:

    ```bash
    $ wget https://pendo-io-extensions.storage.googleapis.com/sdk/0.1.0/js/pendo-extensions-sdk.js
    ```

* You can reference the file from a `script` tag in your web application:
    
    ```html
    <script src="https://pendo-io-extensions.storage.googleapis.com/sdk/0.1.0/js/pendo-extensions-sdk.js"></script>
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

#### Gets

Gets will ask the outer frame agent of the Resource Center for information and return that data via a Promise. These typically don't require arguments.

 * extension.config - retrieve customer-specific configuration information (e.g. API key)
    ```javascript
    client.get('extension.config').then((result) => {...}
    ```
    
 * browser - retrieve browser environment information
    ```javascript
    client.get('browser').then((result) => {...}
    ```

 * metadata - return all agent metadata associated with the current visitor
    ```javascript
    client.get('metadata').then((result) => {...}
    ```

#### Triggers

Triggers are used to issue a command to the outer frame agent of the Resource Center.

 * resizeHeader - Takes integer argument of pixels to adjust the header height to. `0` hides the header.

    ```javascript
    client.trigger('resizeHeader', 10);
    ```

 * updateNotificationBadge - Takes an integer argument and increments the notification indicator on the Guide Center badge by the number passed in.

    ```javascript
    client.trigger('updateNotificationBadge', 3);
    ```

 * content.zoom - Displays an image in a shadow box overlaying the parent web application according to the passed in configuration object. 

   ```javascript
   client.trigger('content.zoom', {
       type: 'IMG',
       top: TopNumberOfPixels, // Integer value
       left: LeftNumberOfPixels, // Integer value
       width: width, // of the target element, Integer value
       height: height, // of the target element, Integer value
       src: src, // url of the image
       options: {
           transitionDuration: NumMilliSecs, //Integer value
           margin: NumPixels //Integer value
       }
   });
   ```

 * toggleLauncher - show or hide the Resource Center
    ```javascript
    client.trigger('toggleLauncher');
    ```

 * returnToMenu - return the user to the top level navigation of Resource Center
    ```javascript
    client.trigger('returnToMenu');
    ```

   
#### Listeners 

_(not currently implemented)_

Listeners are used to set up a callback function in the extension app for whenever the events occur. Typically don't have arguments and the callbacks can be called many times.

 * events.onMenuItemClick - invoked when menu item for this integration in Resource Center is clicked
    ```javascript
    client.listen('events.onMenuItemClick',
        function() {...}
    );
    ```

 * events.onToggleClick - invoked when the Resource Center is opened or closed
    ```javascript
    client.listen('events.onToggleClick',
        function() {...}
    );
    ```
