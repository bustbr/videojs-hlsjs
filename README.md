# Videojs hls.js Plugin

<img align="right" height="30" src="http://www.srgssr.ch/fileadmin/templates/images/SRGLogo.gif">

> An HLS plugin for video.jas based on hls.js

Videojs hls.js offers hls playback using [hls.js](https://github.com/dailymotion/hls.js). For more details on browser compatibility see th hls.js github page.

- [Getting Started](#getting-started)
- [Documentation](#documentation)
  - [Dependencies](#dependencies)
  - [CORS Considerations](#cors-considerations)
  - [Options](#options)
  - [Event Listeners](#event-listeners)
- [Original Author](#original-author)

## Getting Started

Download videojs-hlsjs and include it in your page along with video.js:

```html
<video id="video" preload="auto" class="video-js vjs-default-skin" controls>
    <source src="http://www.streambox.fr/playlists/x36xhzz/x36xhzz.m3u8" type="application/vnd.apple.mpegurl">
</video>
<script src="hlsjs.min.js"></script>
<script src="video.min.js"></script>
<script src="videojs-hlsjs.min.js"></script>
<script>
    var player = videojs('video', {
        // hlsjs tech should come before html5, if you want to give precedence to native HLS playback
        // use the favorNativeHLS option.
        techOrder: ["hlsjs", "html5", "flash"]
    });
</script>
```

There's also a [demo](https://srgssr.github.io/videojs-hlsjs/demo) of the plugin that you can check out.

## Changelog

- 1.4.5: Added text and audio tracks compatibility.

## Documentation

### Dependencies
This project depends on:

- [video.js](https://github.com/videojs/video.js) 5.8.5+.
- [hls.js](https://github.com/dailymotion/hls.js) 0.7.0+.

### CORS Considerations

All HLS resources must be delivered with
[CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) allowing GET requests.

### Options

You may pass in an options object to the hls playback technology at player initialization.

#### hlsjs.favorNativeHLS
Type: `Boolean`

When the `favorNativeHLS` property is set to `true`, the plugin will prioritize native hls
over MSE. Note that in the case native streaming is available other options won't have any effect.

#### hlsjs.disableAutoLevel
Type: `Boolean`

When the `disableAutoLevel` property is set to `true`, the plugin will completely disable auto leveling based on bandwidth and remove it from the list of available level options.
If no level is specified in `hlsjs.startLevelByHeight` or `hlsjs.setLevelByHeight` the plugin will start with the best quality available when this property is set to true.
Useful for browsers that have trouble switching between different qualities.

#### hlsjs.startLevelByHeight
Type: `Number`

When the `startLevelByHeight` property is present, the plugin will start the video on the closest quality to the
specified height but the auto leveling will still be enabled unless `hlsjs.disableAutoLevel` was set to `true`. If height metadata is not present in the HLS playlist this property will be ignored.

#### hlsjs.setLevelByHeight
Type: `Number`

When the `setLevelByHeight` property is present, the plugin will start the video on the closest quality to the
specified height. The auto leveling will be disabled but it will still be selectable unless `hlsjs.disableAutoLevel` was set to `true`. If height metadata is not present in the HLS playlist this property will be ignored.

This property takes precedence over `hlsjs.startLevelByHeight`.

#### hlsjs.hls
Type `object`

An object containing hls.js configuration parameters, see in detail:
[Hls.js Fine Tuning](https://github.com/dailymotion/hls.js/blob/master/doc/API.md#fine-tuning).

**Exceptions:**

* `autoStartLoad` the loading is done through the `preload` attribute of the video tag. This property is always set to `false` when using this plugin.
* `startLevel` if you set any of the level options above this property will be ignored.

### Event listeners

This plugin offers the possibility to attach a callback to any hls.js runtime event, see the documetation
about the different events here: [Hls.js Runtime Events](https://github.com/dailymotion/hls.js/blob/master/doc/API.md#runtime-events). Simply precede the name of the event in camel case by `on`, see an example:

```js
var player = videojs('video', {
    hlsjs: {
        /**
         * Will be called on Hls.Events.MEDIA_ATTACHED.
         *
         * @param {Hls} hls      The hls instance from hls.js
         * @param {Object} data  The data from this HLS runtime event
         */
        onMediaAttached: function(hls, data) {
            // do stuff...
        }
    }
});
```

## Original Author

This project was orginally forked from: [videojs-hlsjs](https://github.com/benjipott/videojs-hlsjs), credits to the
original author.