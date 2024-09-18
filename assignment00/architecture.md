# Main technologies of architecture

## Architecture Overview

![IoT Event Streaming Architecture](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*IUaBLlbVKgmsjbjqzew0ZQ.png)

## Eclipse Mosquitto
MQTT Broker และมีโปรแกรมที่ทำหน้าที่เป็น MQTT Clients สำหรับการเผยแพร่และสมัครรับข้อความกับโบรกเกอร์

## Apache ZooKeeper
เป็นหัวหน้าจัดการ การทำงานของ broker


## Apache Kafka
เป็นการเอาข้อมูลมาต่อคิวก่อนที่จะส่ง เเต่ว่าต่อคิวกันเเบบหลายๆ คิว เรียกว่า distributed queue


## Apache Kafka Connect
extention ที่ช่วยในการสร้าง producer และ consumer โดยอัตโนมัติ


## Apache Kafka Streams
extention ที่ช่วยเปลี่ยนเเปลงข้อมูลใน kafka


## Prometheus
ระบบที่จะคอยตรวจสอบ เเล้วก็เเจ้งเตือนให้เรารู้ก่อนล่วงหน้า ว่าจะมี error เหมือนกับ logo ที่เป็นคบเพลิง เอาไว้เผา bug เผา เเมลงให้ตายให้หมด


## MongoDB
database เเบบ not only sql ที่ดังที่สุดในตอนนี้


## Grafana
ตัวช่วยสร้าง dash board อัตโนมัติ

