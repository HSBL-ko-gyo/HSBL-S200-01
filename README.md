# HSBL-S200-01

M5Stack CoreS3、SEの底面にRGBLEDを追加するハードウエアです。  
<img src="https://github.com/HSBL-ko-gyo/HSBL-S200-01/blob/main/Image/04.gif?raw=true" alt="image" width="300"/>

[スイッチサイエンスで販売中](https://www.switch-science.com/products/9815)

## 組み立て
<img src="https://github.com/user-attachments/assets/7e2919a2-27f3-4177-b19d-9b0f10b13d1a" alt="image" width="300"/>

上下があります。
光拡散部のマークがある方がM5の下側になるように取り付け附属のネジで締めてください。  

附属のネジ4本は[M3-L16](https://jlcmc.com/product/s/E02/EDLU/FA-%E7%B4%A7%E5%9B%BA%E9%9B%B6%E4%BB%B6-%E8%9E%BA%E9%92%89?productModelNumber=EDLU-C17-M-M3-L16)です。

## 光拡散部3Dデータ

[リンク](https://github.com/HSBL-ko-gyo/HSBL-S200-01/tree/main/3D)

| 項目       | 詳細       |
|------------|------------|
| 製造業者 | JLCPCB  |
| 3Dテクノロジー | SLA(樹脂)  |
| 材料       | 8001樹脂   |
| 色         | 透明       |
| 表面仕上げ   | いいえ     |



## 回路図
[リンク](https://github.com/HSBL-ko-gyo/HSBL-S200-01/tree/main/ElectricCircuit)

## サンプルスケッチ

### Arduino

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
