# Linux でビデオデバイスを指定する

Momo ではデバイスを指定する際、デバイスファイルではなくデバイス名を指定します。

## --video-device

`--video-device` は linux 端末でビデオデバイス（つまりカメラ）を指定する機能です。
1 台の Raspberry Pi で複数のカメラを利用して複数の Momo を起動する時に、カメラデバイスを個別に割り当てることができます。

```bash
./momo --video-device "MX Brio" test
```

### デバイス名を取得する

デバイス名は、`v4l2-ctl` コマンドで取得できます。
例えば以下のような実行結果の場合、デバイス名は `HD USB CAMERA: HD USB CAMERA` と `Integrated_Webcam: Integrate` です。

```bash
v4l2-ctl --list-devices
HD USB CAMERA: HD USB CAMERA (usb-0000:00:00.0-1):
        /dev/video0
        /dev/video1
        /dev/media0

Integrated_Webcam: Integrate (usb-0000:00:00.0-12):
        /dev/video2
        /dev/video3
        /dev/media1
```

### 同一カメラデバイスを複数接続している場合

同一カメラデバイスを複数接続している場合、デバイス名が同じであるため、個別に指定することができません。
その場合は、デバイス名を指定する代わりに、`Bus info` を指定することができます。

例えば以下のような `v4l2-ctl` コマンド の実行結果の場合、`Bus info` は `usb-0000:00:00.0-1` と `usb-0000:00:00.0-12` です。

```bash
v4l2-ctl --list-devices
HD USB CAMERA: HD USB CAMERA (usb-0000:00:00.0-1):
        /dev/video0
        /dev/video1
        /dev/media0

Integrated_Webcam: Integrate (usb-0000:00:00.0-12):
        /dev/video2
        /dev/video3
        /dev/media1
```

`Bus info` を指定する実行例は以下の通りです。

```bash
./momo --video-device "usb-0000:00:00.0-1" test
```