# Ingest and store real-time data from IoT sensors 
>> อธิบาย 3 ส่วนนี้ สร้างมาได้อย่างไร

## iot-sensor-1
>> คืออะไร IoT sensor 1 เป็น sensor ที่ถูกจําลองด้วยไมโครเซอร์วิสที่ใช้ใน Spring Boot (ผ่านไลบรารี Eclipse Paho MQTT) ที่ถูกติดตั้งอยู่บนเซิฟเวอร์ โดยจะส่งข้อมูล telemetry ไปยังโบรกเกอร์ Eclipse Mosquitto ข้อมูลที่ถูกจำลองนี้ generate ค่า ทุกอย่างภายใน payload มาจาก Callable โดยจะถูกสร้างขึ้นทุกวินาทีและ มี payload ในรูปแบบที่สร้างขึ้นให้ตรงกันคืออะไร 

## iot-sensor-2
>> คืออะไร เป็น sensor ที่ถูกจําลองด้วยไมโครเซอร์วิสที่ใช้ใน Spring Boot (ผ่านไลบรารี Eclipse Paho MQTT)เช่นเดียวกันกับ sensor 1 เพียงแต่ติดตั้งอยู่ในเครื่องของคนในทีม

## iot-sensor-3-10
>> คืออะไร เซ็นเซอร์ 3 จะเป็นค่าจริงจาก sensor ที่เป็นการนำค่าที่อ่านได้จาก Cucumber RS ของกลุ่มตนเอง แตกต่างจาก iot-sensor-1, iot-sensor-2 ที่เป็นการ Mock-up แล้วส่งผ่าน MQTT เพื่อนำมาแสดง และ Payload ต่างๆก็ถูกตั้งให้อยู่ใน format เดียวกัน แล้ว เพื่อการรำไปใช้ต่อได้ในทุกๆ sensor Sensor 4 - 10 นั้น จะเป็นค่าจริงเช่นเดียวกัน แต่จะเป็นค่าจาก จาก sensor ของกลุ่มอื่น ที่เป็นการนำค่าที่อ่านได้จาก Cucumber RS ของแต่ละกลุ่ม ที่ส่งผ่าน MQTT มาให้เช่นเดียวกัน เพื่อนำมาแสดง และ Payload ต่างๆก็ถูกตั้งให้อยู่ใน format เดียวกัน แล้ว เพื่อการแสดงผ่าน Cucumber แบบแสดงผลพร้อมกัน 10 sensor


# MQTT

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