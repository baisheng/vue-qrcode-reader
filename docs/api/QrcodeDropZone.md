# QrcodeDropZone

## Events

### `decode`
* **Payload Type:** `String`

You can drag-and-drop image files from your desktop or images embedded into other web pages anywhere in the area the component occupies. The images are directly scanned and positive results are indicated by the `decode` event. You can also drop multiple images at the same time (still one event per image though).

However, if no QR code is pictured or an error occurs, `decode` silently fails.

```html
<qrcode-drop-zone @decode="onDecode">
   <!-- ... -->
</qrcode-drop-zone>
```
```javascript
methods: {
  onDecode (decodedString) {
    // ...
  }
}
```

### `detect`
* **Payload Type:** `Promise<Object>`

The `detect` event is basically a verbose version of `decode`. `detect` is emitted as soon as you drop an image. It carries a Promise which resolves when scanning the dropped image has finished. The Promise rejects in case of errors. Additionally, `detect` gives you the unprocessed raw image data and the coordinates of the QR code in the image.

```html
<qrcode-drop-zone @detect="onDetect">
   <!-- ... -->
</qrcode-drop-zone>
```
```javascript
methods: {
  async onDetect (promise) {
    try {
      const {
        imageData,    // raw image data of image/frame
        content,      // decoded String or null
        location      // QR code coordinates or null
      } = await promise

      if (content === null) {
         // decoded nothing
      } else {      
         // ...
      }
    } catch (error) {
      if (error.name === 'DropImageFetchError') {
        // drag-and-dropped URL (probably just an <img> element) from different
        // domain without CORS header caused same-origin-policy violation
      } else if (error.name === 'DropImageDecodeError') {
        // drag-and-dropped file is not of type image and can't be decoded
      } else {
        // idk, open an issue ¯\_(ツ)_/¯
      }
    }
  }
}
```

## Props

> no props

## Slots

### default

This component merely renders a wrapper `div`. Its height is defined by the content inside so it will have zero height if you don't provide any content.

```html
<qrcode-drop-zone>
  <b>put anything here</b>
</qrcode-drop-zone>
```