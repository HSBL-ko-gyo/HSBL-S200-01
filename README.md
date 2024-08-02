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

### M5Unified.h不使用(競合してしまった際などにご利用ください)

```cpp

//#include <Arduino.h>
#include <Adafruit_NeoPixel.h>
#include <Wire.h> // I2C communication header

#define PIN        5  // ネオピクセルピンデフォ S3の場合
#define NUMPIXELS  4  // ネオピクセルの数

// NeoPixelオブジェクトを作成
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

// Constants for USB power control
static constexpr const uint8_t aw9523_i2c_addr = 0x58; // AW9523 I2C address
static constexpr const uint32_t _core_s3_bus_en = 0b00000010; // BUS EN
static constexpr const uint32_t _core_s3_usb_en = 0b00100000; // USB OTG EN
static constexpr const uint8_t port0_reg = 0x02;
static constexpr const uint32_t port1_bitmask_boost = 0b10000000; // BOOST_EN

// core_s3_output関数
void _core_s3_output(uint8_t mask, bool enable)
{
    uint8_t buf[2];

    // Read I2C register
    Wire.beginTransmission(aw9523_i2c_addr);
    Wire.write(port0_reg);
    Wire.endTransmission();
    Wire.requestFrom(aw9523_i2c_addr, (uint8_t)2);
    if (Wire.available() == 2) {
        buf[0] = Wire.read();
        buf[1] = Wire.read();
    } else {
        Serial.println("Failed to read from AW9523");
        return;
    }

    uint8_t p0 = buf[0] | mask;
    uint8_t p1 = buf[1] | port1_bitmask_boost;

    if (!enable) {
        p0 = buf[0] & ~mask;
        if (0 == (p0 & _core_s3_bus_en)) {
            p1 &= ~port1_bitmask_boost;
        }
    }

    buf[0] = p0;
    buf[1] = p1;

    // Write to I2C register
    Wire.beginTransmission(aw9523_i2c_addr);
    Wire.write(port0_reg);
    Wire.write(buf, 2);
    Wire.endTransmission();
}

void setup() {
  Wire.begin(); // Initialize I2C
  // Enable BUS output
  _core_s3_output(_core_s3_bus_en, true);

  pixels.begin(); // NeoPixelの初期化

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
