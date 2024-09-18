# IoT Docker compose

## How to start docker compose
1. ติดตั้ง rufas
2. ติดตั้ง ubantu iso ของ version 22.04.xx
3. ใช้ rufas burn flashrdrive ให้ กลายเป็น boot device
4. เข้า bios ในเครื่อง dell 5000
5. เปลี่ยน boot privority ให้ boot ผ่าน flashdrive sandisk ก่อนเสมอ
6. กด save config ใน bios
7. เครื่องจะ restart เอง
8. Chose try or install buntu Server
9. Chose longuage..
10. Chose ubuntu server(minimize)
11. Confie my ip address ต้องไม่ซ้ำกัน
12. Chose custom storage layout
13. Delete old doto (parsition)
14. เอา free space ที่มีอยู่มาตั้งเป็น swap ขนาด 1GB ชนิดข้อมูลเเบบ ext4
15. เอาที่ว่างที่เหลือทั้งหมดมาทำเป็น /
16. กด continue
17. ตั้ง username password ตั้งชื่อเครื่อง ตั้งชื่อเจ้าของเครื่อง
18. หลัง download เสร็จ เลือก Reboot now
19. login user-password ใน terminal
20. ดูพื้นที่เก็บในไดร์ฟ ด้วยคำสั่ง df -h
21. ติดตั้ง text editor ที่ชื่อว่า nano ด้วยคำสั่ง sudo apt-get install nano -y
22. cd/etc/netplan
25. sudo nano 00-network-config.yaml  # เพื่อเเก้ configuration ของ network ให้เป็นเเบบ static
เเละเลือก domain name server เป็นของ รร เเล้วก็ตั้ง domain สำรองเป็นของ 8.8.8.8 (google DNS)
24. ctrl + s เพื่อ save
25. ctrl + x เพื่อ exit
26. sudo netplan apply
27. sudo reboot now 
28. ติดตั้ง github
29. ติดตั้ง docker
30. clone git hub ของอาจารย์มาลง
31. docker compose up

## Error we founud
1. kafka ไม่ start
2. kafka error 404

## How to solve the problems.
- docker compose restart kafka
- docker compose log -f |egrep 'WARN|ERR' (ใช้สำหรับดู logs จาก Docker Compose ที่มีข้อความที่มีคำว่า "WARN" หรือ "ERR" โดยใช้คำสั่ง egrep เพื่อกรองเฉพาะบรรทัดที่มีข้อความเหล่านี้อยู่เท่านั้น เช่น คำสั่งที่ให้มาจะทำการเรียกดู logs แบบ real-time (-f) และกรองเฉพาะข้อความที่มี "WARN" หรือ "ERR" ด้วย 'egrep'.)

- วิธีดังกล่าวไม่ได้ผล จนครูตัดสินใจทำ IOT compose ขึ้นมาเองเลย
[https://github.com/hanattaw/iot_event_streaming_architecture](https://github.com/hanattaw/iot_event_streaming_architecture)

- clone github ครูไปลงใน server dell

- ตอน ```docker compose up``` จะมี container หลายๆ ตัว เกิด dependency error ซึ่งก็คือ container นั้นๆ อาจจะต้องการให้ container run เสร็จก่อน เช่น A ต้องการให้ database เปิดก่อน ถึงจะไปเชื่อมได้ เเต่ในที่นี้ database ยัง start ไม่เสร็จ A ก็เชื่อมต่อ database ไม่สำเร็จ เเล้วก็ค้างไปเลย

- วิธีเเก้ dependency error ทำได้ 2 วิธี คือ ใส่ dependency array ให้ dockercompose.yaml หรือ start ไปเลยไม่ต้องสนใจอะไรทั้งนั้น container ใหนมีปัญหา ค่อยมานั่งใส่คำสั่ง docker restart เอา ยกตัวอย่างตัวที่ error บ่อยจนผมฝังใจเช่น

```bash
docker compose restart grafana
```

grafana มีปัญหาเยอะสุดน่าจะเพราะว่ามันคือตัว dashboard มันจะต้องเชื่อมต่อกับทุกอย่าง เอาข้อมูลทุกอย่างมาเเสดงผลให้เราดู

## Output

- [ ✓ ] IoT Sensor - Dashboards - Grafana 
- [ ✓ ] UI for Apache Ka
- [ ✓ ] Mongo Expr
- [ ✓ ] Node Expor
- [ ✓ ] Prometheus Time Series Collection and Processing Ser
- [ ✓ ] Prometheus Pushgateway
- [ ✓ ] ZooNavigator


# คำเตือน : url พวกนี้อย่ากดโดยไม่รู้เรื่อง เพราะใน project นี้ ทางเราได้ทำ private network มาเพื่อทำการทดลองนี้โดยเฉพาะ
### IoT Sensor - Dashboards - Grafana URL

http://172.16.46.71:8085/

### UI for Apache Kafka
ยังไม่เคยเข้า เเต่น่าจะเป็น 
http://172.16.46.71:8081/ หรือ http://172.16.46.71:8082/

### Mongo Express
ยังไม่เคยเข้า เเต่น่าจะเป็น 
http://172.16.46.71:8084/

### Node Exporter
ยังไม่เคยเข้า เเต่น่าจะเป็น 


### Prometheus Time Series Collection and Processing Server
ยังไม่เคยเข้า เเต่น่าจะเป็น 


### Prometheus Pushgateway
ยังไม่เคยเข้า เเต่น่าจะเป็น 


### ZooNavigator
ยังไม่เคยเข้า เเต่น่าจะเป็น 