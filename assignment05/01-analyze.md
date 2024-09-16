# Analyze and make aggregations.

การวิเคราะห์ข้อมูล (Analyze)

การวิเคราะห์สามารถทำได้โดยการประมวลผลข้อมูลจากเซ็นเซอร์เพื่อหาค่าที่ตรงกับค่าความเป็นจริง เช่น ความดัน ที่ถูกส่งมาจากเซ็นเซอร์มีค่า 100 bar ดังนั้นเพื่อที่จะให้ส่งข้อมูลไปยัง server ได้อย่างถูกต้อง ดังนั้นต้องนำค่าที่ได้ ไปหารกับ 100 เป็นต้น

  /// Data assign
  float temp = bmp280.readTemperature();
  float pressure = bmp280.readPressure() / 100;
  float aTemperature = 0.0;
  float humid = 0.0;

และรวมไปถึงการคำนวนอื่นๆ เช่น อุณหภูมิ ความชื้น เป็นต้น

การรวบรวมข้อมูล (Aggregations
)
การรวมข้อมูลหลาย ๆ ชุดก่อนส่งไปยัง MQTT broker ตัวอย่างเช่น:
การส่งข้อมูลสรุปทุก ๆ 10 นาที: แทนที่จะส่งข้อมูลทุกครั้งที่ทำการอ่านเซ็นเซอร์ เราสามารถเก็บรวบรวมข้อมูลและส่งเป็นชุดในทุก ๆ ช่วงเวลา เช่น ทุก ๆ 10 นาที เป็นต้น

ตัวอย่าง การทำ aggregation ในโค้ด

void loop() {
  if (!client.connected()) {
    mqtt_handle();
  }
  client.loop();

  // เมื่อทำงานกับ MQTT กระพริบไฟสีเขียว
  strip.setPixelColor(0, strip.Color(255, 0, 0)); // สีเขียว
  strip.show();
  delay(250); // รอ 250 มิลลิวินาที

  // ปิดไฟ (สีดำ)
  strip.setPixelColor(0, strip.Color(0, 0, 0)); // สีดำ (ปิด)
  strip.show();
  delay(250); // รอ 250 มิลลิวินาที

  /// Data assign
  float temp = bmp280.readTemperature();
  float pressure = bmp280.readPressure() / 100; (นำมา หาร 100 เพื่อให้ค่าที่ออกมาถูกต้องตามหน่วยวิทยาศาสตร์)
  float aTemperature = 0.0;
  float humid = 0.0;
  error = sensor.measureLowestPrecision(temp, humid);
  checkError(error, "Error to call STHX4");
  int analogval = analogRead(sensorPin);
  unsigned long epochTime = Get_EpochTime();

  Serial.println(temp);
  Serial.println(pressure);
  Serial.println(humid);
  Serial.println(analogval);

  /// json output (การรวบรวมข้อมูลเป็นไฟล์ JSON ก่อนส่งไปยัง server)
  StaticJsonDocument<512> jsonDoc;
  jsonDoc["id"] = "";  //id
  jsonDoc["name"] = ""; //name
  jsonDoc["place_id"] = ""; //place_id
  jsonDoc["date"] = NTP.getTimeDateString(time(NULL), "%Y-%m-%dT%H:%M:%S"); // ISO 8601 format
  jsonDoc["timestamp"] = epochTime;

  JsonObject payload = jsonDoc.createNestedObject("payload"); (รวบรวมข้อมูลเป็นไฟล์ JSON และสร้าง Payload เพื่อส่งไปยัง server)
  payload["temperature"] = temp;
  payload["humidity"] = humid;
  payload["pressure"] = pressure;
  payload["luminosity"] = analogval;

  char jsonData[512];
  serializeJson(jsonDoc, jsonData);

  // Export MQTT data
  client.publish("iot-frames", jsonData);

  Serial.print("Time:");
  Serial.print(NTP.getTimeDateString(time(NULL), "%Y-%m-%dT%H:%M:%S"));
  Serial.println("");
  delay(500);
}
