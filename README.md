# Pendo Resource Center Integration Kit

## Pendo-Extension-SDK

_Getting the SDK_

You can download the SDK from here:

`https://pendo-io-extensions.storage.googleapis.com/sdk/0.1.0/js/pendo-extensions.sdk.js`

_Using the SDK_

```
const client = PendoSDK.getConnection("app-name");
client.initialize();

client.get('extension.config').then((result) => {
    const options = result.data.config.provider.options;
    const {apiKey, subdomain} = options;
    
    // do something with these customer specific configuration values here
});

client.
```



Here are master set of commands available today.

### Triggers

Triggers are used to issue a command to the outer frame Agent. There is no feedback to status. The command will often require arguments.

 * triggers.resizeHeader - Takes integer argument of pixels to adjust the header height to. `0` hides the header.

 * triggers.updateNotificaitonBadge - Takes integer argument to show the notifications indicator on the Guide Center badge with the number passed in.

 * content.zoom - Takes an argument object like 

   ```
   {
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
   }
   ```

   

 * triggers.toggleLauncher

 * triggers.returnToMenu

   

### Gets

Gets will ask for information and return that via a Promise. They typically don't require arguments.

 * browser 

    * env
    * url

 * metadata

 * extension

    * config

    * stylesheet

      

### Listeners 

_(not implemented yet but will be in version 0.1.1)_

Listeners are used to set up a callback function in the extension app for whenever the events occur. Typically don't have arguments and the callbacks can be called many times.

 * events.onMenuItemClick
 * events.onToggleClick
