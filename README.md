# HSBL-S200-01

M5Stack CoreS3、SEの底面にRGBLEDを追加するハードウエアです。  
![](https://github.com/HSBL-ko-gyo/HSBL-S200-01/blob/main/Image/04.gif?raw=true)

[スイッチサイエンスで販売中](https://www.switch-science.com/products/9815)

## 回路図
[リンク](https://github.com/HSBL-ko-gyo/HSBL-S200-01/tree/main/ElectricCircuit)

## サンプルスケッチ

```cpp

//#include <Arduino.h>
#include <Adafruit_NeoPixel.h>
#include <M5Unified.h>

#define PIN        5  // ネオピクセルピンデフォ S3の場合
#define NUMPIXELS  4  // ネオピクセルの数

// NeoPixelオブジェクトを作成
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  pixels.begin(); // NeoPixelの初期化
  M5.begin(); // CoreS3はこれを行わないと底面より5Vが出ないので注意
}

void loop() {
  // 赤色
  for(int i=0; i<NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(255, 0, 0));
    pixels.show();
    delay(500);
  }
  
  // 緑色
  for(int i=0; i<NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(0, 255, 0));
    pixels.show();
    delay(500);
  }
  
  // 青色
  for(int i=0; i<NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(0, 0, 255));
    pixels.show();
    delay(500);
  }
  
  // すべてのLEDを消す
  pixels.clear();
  pixels.show();
  delay(1000);
}

```
