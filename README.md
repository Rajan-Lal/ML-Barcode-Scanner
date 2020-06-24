# ML Barcode Scanner
 Barcode scanner sample application using CameraX and Android ML vision API's

With ML Kit's barcode scanning API, you can read data encoded using most standard barcode formats. Barcode scanning happens on the device, and doesn't require a network connection.

There are two ways to integrate barcode scanning: by bundling the model as part of your app, or by using an unbundled model that depends on Google Play Services. If you select the unbundled model, your app will be smaller, however the bundled model has performance advantages over the unbundled model.

This Example application make use of bundling the model as part of the application.

### For bundling the model with your app
```
dependencies {
      // ...
      // Use this dependency to bundle the model with your app
      implementation 'com.google.mlkit:barcode-scanning:16.0.0'
    }
```

    
### Manifest Permissions
```
<uses-permission android:name="android.permission.CAMERA" />
```

### Configure the barcode scanner

```
void configureScanner() {
    BarcodeScannerOptions options =
                new BarcodeScannerOptions.Builder()
                        .setBarcodeFormats(
                                Barcode.FORMAT_ALL_FORMATS)
                        .build();
        scanner = BarcodeScanning.getClient(options);
    }
```
    
### To start the preview and start receiving the image buffer, we need to bind to the lifecyle:
```
cameraProvider.bindToLifecycle((LifecycleOwner) this, cameraSelector, preview, imageAnalysis, imageCapture);
```

### Stop the preview for processing the received data from the Scanner
```
cameraProvider.unbindAll();
``` 

***The scanner will not work if the target resolution of the ImageAnalysis is not set to that of the device maximum supported resolution. Hence to set the resolution of the ImageAnalysis, we need to get the device screen resolution and set that as the resolution for ImageAnalysis***

### Setting the resolution
```
DisplayMetrics metrics = new DisplayMetrics();
Size screenSize = new Size(metrics.widthPixels, metrics.heightPixels);
getWindowManager().getDefaultDisplay().getMetrics(metrics);

imageAnalysis = new ImageAnalysis.Builder()
                        .setTargetResolution(screenSize)
                        .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                        .build();
```
