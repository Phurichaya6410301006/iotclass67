# Ingest and store real-time data from IoT sensors 
>> อธิบาย 3 ส่วนนี้ สร้างมาได้อย่างไร

## iot-sensor-1
>> คือIoT sensor 1 ซึ่งเป็นเซนเซอร์ที่ถูกจำลองด้วยไมโครเซอร์วิสใน Spring Boot โดยใช้ไลบรารี Eclipse Paho MQTT เซนเซอร์นี้ส่งข้อมูล telemetry ไปยังโบรกเกอร์ Eclipse Mosquitto ข้อมูลใน payload จะถูกสร้างขึ้นทุกวินาทีจาก Callable และมีรูปแบบ payload ที่กำหนดไว้ล่วงหน้าเพื่อให้ตรงกัน 

## iot-sensor-2
>> เซนเซอร์ที่ถูกจำลองด้วยไมโครเซอร์วิสใน Spring Boot โดยใช้ไลบรารี Eclipse Paho MQTT เช่นเดียวกับ "IoT sensor 1 แต่เซนเซอร์นี้ติดตั้งอยู่ในเครื่องของสมาชิกในทีม

## iot-sensor-3-10
>> sensor-3 ซึ่งเป็นเซนเซอร์ที่ใช้ค่าจริงจาก Cucumber RS ของกลุ่มตนเอง แตกต่างจาก "iot-sensor-1" และ "iot-sensor-2" ที่เป็นการจำลอง (mock-up) ส่งผ่าน MQTT โดย payload ถูกตั้งให้อยู่ในรูปแบบเดียวกัน สำหรับการใช้งานร่วมกันกับเซนเซอร์อื่นๆ เซนเซอร์ 4-10 ก็เป็นค่าจริงจากเซนเซอร์ของกลุ่มอื่น ที่อ่านค่าจาก Cucumber RS และส่งผ่าน MQTT ด้วย payload ที่มีรูปแบบเดียวกัน เพื่อแสดงผลพร้อมกัน 10 เซนเซอร์ใน Cucumber.

![mqtt plan](../img/partNetwork/Generic-operation-scheme-of-the-MQTT-protocol.png)

## หลักการทำงานคร่าวๆ ของ MQTT มีดังนี้
- MQTT Broker
- MQTT Clients
- MQTT Topics
- MQTT Payload
- MQTT actors

### MQTT Broker
ทำหน้าที่เหมือนกับนายหน้าขายของ amway เเต่ในที่นี้คือจะเป็นนายหน้าจะคอยจัดการระบบ `pipe line` ซึ่งระบบนี้ มันไม่เหมือน restfull api มันจะส่ง data เป็นสาย(streaming data) broker ทุกตัวจะบริหารจัดการท่อ pipeline ให้ส่งข้อมูล stream ข้อมูลมหาศาลได้

### MQTT Clients
คือตัวลุกข่ายที่จะต้อง ``subscribe topic`` เเละรับข้อมูลมาจาก topic นั้นๆ เเบบรับเป็นสายข้อมูล streamdata เช่น รับอุณหภุมิมาอย่างต่อเนื่อง เพื่อจะเอามาเเสดงในหน้า dashboard

### MQTT Topics
เป็นหัวข้อเรื่อง ให้ actors มา `subscribe` เเล้ว public ข้อมูลลงเเบบ realtime
ให้ client มา subscribe แล้วดึงข้อมูลออกมาเเบบ realtime  เพื่อมาเเสดงข้อมูลลงหน้า dashboard

### MQTT Payload
ข้อมูลที่ถุกส่งไปใน pipline

### MQTT actors
ตามจริงเรียกได้หลายเเบบ เเต่หลักๆ เลย มันจะเป็นตัวที่ `subscribe topic` เเละ `publish data` ลงไปใน topic นั้นๆ

เเหล่งข้อมูลเพิ่มเติม

[https://www.youtube.com/watch?v=jTeJxQFD8Ak&list=PLRkdoPznE1EMXLW6XoYLGd4uUaB6wB0wd](https://www.youtube.com/watch?v=jTeJxQFD8Ak&list=PLRkdoPznE1EMXLW6XoYLGd4uUaB6wB0wd)
