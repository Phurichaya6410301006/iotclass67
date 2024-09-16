# Store data.

1. การอ่านค่าจากเซ็นเซอร์
โค้ดในส่วนนี้จะอ่านข้อมูลจากเซ็นเซอร์ต่างๆ เช่น:

อุณหภูมิและความกดอากาศ จากเซ็นเซอร์ BMP280
อุณหภูมิและความชื้น จากเซ็นเซอร์ SHT4x
ความสว่าง จากเซ็นเซอร์แสง

ตัวอย่างจากโค้ด:

    float temp = bmp280.readTemperature();
    float pressure = bmp280.readPressure() / 100;
    float aTemperature = 0.0;
    float humid = 0.0;
    error = sensor.measureLowestPrecision(temp, humid);
    checkError(error, "Error to call STHX4");
    int analogval = analogRead(sensorPin);
    unsigned long epochTime = Get_EpochTime();

    // temp จะเก็บค่าอุณหภูมิที่อ่านได้จาก BMP280
    // pressure จะเก็บค่าความกดอากาศ
    // humid จะเก็บค่าความชื้น
    // analogval จะเก็บค่าความสว่างที่อ่านจากเซ็นเซอร์แสง
    // epochTime จะเก็บค่าเวลาปัจจุบันในรูปแบบของ Unix timestamp

2. การสร้าง JSON Document
เมื่อได้ข้อมูลจากเซ็นเซอร์แล้ว ข้อมูลเหล่านี้จะถูกจัดเก็บในรูปแบบของ JSON document โดยใช้ไลบรารี ArduinoJson ซึ่ง JSON document นี้จะเป็นโครงสร้างข้อมูลที่ใช้ในการส่งข้อมูลไปยัง MQTT broker

ตัวอย่างการสร้าง JSON document ภายในโค้ด :

    StaticJsonDocument<512> jsonDoc;
    jsonDoc["id"] = "43245253";
    jsonDoc["name"] = "iot_sensor_3";
    jsonDoc["place_id"] = "42343243";
    jsonDoc["date"] = NTP.getTimeDateString(time(NULL), "%Y-%m-%dT%H:%M:%S"); // ISO 8601 format
    jsonDoc["timestamp"] = epochTime;

    JsonObject payload = jsonDoc.createNestedObject("payload");
    payload["temperature"] = temp;
    payload["humidity"] = humid;
    payload["pressure"] = pressure;
    payload["luminosity"] = analogval;

    ข้อมูล "id", "name", "place_id" และ "date" จะถูกกำหนดใน JSON document
    ข้อมูลเซ็นเซอร์จะถูกจัดเก็บใน JSON object ภายใต้คีย์ "payload"

3. การแปลง JSON Document เป็นข้อความ
หลังจากสร้าง JSON document เสร็จแล้ว ขั้นตอนต่อไปคือการแปลง JSON document ให้เป็นข้อความ JSON ที่สามารถส่งผ่าน MQTT

ตัวอย่างการแปลง JSON document เป็นข้อความ JSON ในโค้ด:

    char jsonData[512];
    serializeJson(jsonDoc, jsonData);

4. การส่งข้อมูลผ่าน MQTT
ข้อความ JSON ที่ได้จะถูกส่งผ่าน MQTT ไปยัง MQTT broker โดยใช้หัวข้อ (topic) "iot-frames"

ตัวอย่างการส่งข้อมูลผ่าน MQTT:

    client.publish("iot-frames", jsonData);

สรุป:
ในหัวข้อ # Store data โค้ดจะทำการ:

- อ่านข้อมูลจากเซ็นเซอร์ต่างๆ
- สร้าง JSON document เพื่อเก็บข้อมูลที่อ่านได้
- แปลง JSON document เป็นข้อความ JSON
- ส่งข้อความ JSON ผ่าน MQTT ไปยัง MQTT broker เพื่อเก็บข้อมูลและใช้ต่อไป

