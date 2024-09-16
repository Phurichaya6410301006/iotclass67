# Ingest and store real-time data from IoT sensors.

การตั้งค่าและการเชื่อมต่อ

การตั้งค่า WiFi และ MQTT:
WiFi และ MQTT broker ตั้งค่าที่อยู่ IP, พอร์ต, และข้อมูล login (username,password) สำหรับการเชื่อมต่อ
ใช้ Adafruit_NeoPixel สำหรับการแสดงสถานะการเชื่อมต่อ WiFi และ MQTT ผ่าน LED

การเชื่อมต่อ WiFi:
ฟังก์ชัน setup_wifi() จะเชื่อมต่อกับ WiFi และแสดงสถานะการเชื่อมต่อผ่านการกระพริบของ LED
หากการเชื่อมต่อสำเร็จ LED จะเป็นสีเขียว ถ้าไม่สำเร็จ LED จะกระพริบสีแดง

การเชื่อมต่อ MQTT:
ฟังก์ชัน mqtt_handle() จัดการการเชื่อมต่อกับ MQTT broker
ถ้าเชื่อมต่อสำเร็จ LED จะเปลี่ยนเป็นสีเขียว
การเริ่มต้นเซ็นเซอร์และ NTP:

เซ็นเซอร์ต่างๆ เช่น BMP280 และ SHT4x จะถูกเริ่มต้นและตรวจสอบความผิดพลาด
NTP client ถูกตั้งค่าเพื่อซิงค์เวลา

## MQTT Topic

MQTT Topic "iot-frames"

## MQTT Payload

MQTT Payload
Payload ที่ส่งผ่าน MQTT มีรูปแบบ JSON ที่รวมข้อมูลจากเซ็นเซอร์และข้อมูลเวลา:

{
  "id": "12434349",
  "name": "iot_sensor_3",
  "place_id": "32347983",
  "date": "2024-08-28T12:34:56",  // เวลาปัจจุบันในรูปแบบ ISO 8601
  "timestamp": 1693222496,         // เวลาในรูปแบบ epoch
  "payload": {
    "temperature": 25.3,           // อุณหภูมิจาก BMP280
    "humidity": 45.0,              // ความชื้นจาก SHT4x
    "pressure": 1012.5,            // ความดันจาก BMP280
    "luminosity": 123              // ค่าแสงจาก LDR
  }
}


## ESP32

ESP32 ทำหน้าที่เป็น:

- Client สำหรับเชื่อมต่อกับ WiFi และ MQTT broker
- Collector สำหรับเก็บข้อมูลจากเซ็นเซอร์ต่างๆ
- Publisher สำหรับส่งข้อมูลที่เก็บได้ไปยัง MQTT broke

