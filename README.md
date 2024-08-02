# HSBL-S200-01

M5Stack CoreS3、SEの底面にRGBLEDを追加するハードウエアです。  

スイッチサイエンスで販売予定  

## 回路図
[リンク](https://github.com/HSBL-ko-gyo/HSBL-S200-01/tree/main/ElectricCircuit)

## サンプルスケッチ

### M5Unified.h使用

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
  M5.begin();
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
