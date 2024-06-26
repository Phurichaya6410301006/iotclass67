# Install Server and Docker


## How to install Server

ขั้นตอนการติดตั้ง servers ในอุปกรณ์ Dell Server
1.ขั้นตอนแรกสิ่งที่ต้องเตรียมคือ Dell Server และ Monitor ที่ใช้สำหรับจัดการข้อมูลที่ใช้สำหรับติดตั้ง Mouse, Keyboard, สายLan, Flashdrive
2.เมื่อต่ออุปกรณ์เสร็จแล้ว ต่อมาคือการติดตั้ง ระบบปฏิบัติการ Ubuntu Server ในเครื่อง Dell Server โดยเราจะต้องเข้าไปดาวน์โหลด File Driver สำหรับติดตั้งจาก Website ของ Ubuntu ในแต่ละ version ซึ่งเราจะเลือกใช้ ubuntu -v 22.04 (https://ubuntu.com/download/server) แล้วเก็บไว้ใน flashdrive
3.เมื่อดาวน์โหลดเรียบร้อยแล้ว ให้เราเปิดเครื่อง Dell server และ Boot ระบบด้วย flashdrive (กด F2) หลังจากนั้น Monitor ก็จะแสดงผลเครื่องมือที่ให้เราใช้สำหรับติดตั้ง Ubuntu Server  โดยเป็นการ setting ค่าต่างๆของระบบก่อนจะทำการ setup เช่น พื้นที่เก็บไฟล์ ตั้งค่า user และ password ของ computer หรือลง driver อื่นๆ ที่จำเป็นใน ubuntu server (โดย project นี้เราไม่ได้ตั้งค่าพื้นที่เก็บไฟล์สำหรับระบบปฏิบัติการดังนั้น ระบบจะ boot แค่ single mode คือ ubuntu 22.04 เท่านั้น เสมือนเป็นการลงทับ window อันเดิม)
4.หลังจากที่เรา setting เสร็จถึงขั้นตอนสุดท้ายแล้ว ระบบจะ setup ค่าต่างๆ ขั้นตอนนี้ให้เรารอจนกว่าระบบจะติดตั้งสำเร็จจากนั้น restart เครื่องแล้วเปิดใหม่ เพื่อ test ว่าเวลาเราเปิด Dell Server ขึ้นมา Ubuntu server สามารถใช้งานได้ตามปกติ (หรือจะเช็คผ่านหน้า bios ก็ได้)



## How to install Docker
1.ขั้นตอนแรกให้เราเข้าใช้งาน Dell Server ตามปกติ โดยการ login User และ Password ที่เรา Setting ไว้ในตอนที่เราทำการ install
2.ต่อมาตรวจสอบว่า internet ได้ทำการเชื่อมต่อหรือไม่โดยการทดลองใช้คำสั่ง sudo apt update หรือ upgrade ดูว่า command ใช้งานได้ตามปกติโดยไม่ Error เป็นต้น หากยังไม่ได้เชื่อมต่อ จะมีวิธีการเชื่อมต่อดังนี้ โดยวิธีการเชื่อมต่อ เราจะทำการเชื่อมต่อ internet แบบ bridge เพื่อให้ Dell Server สามารถใช้งาน internet ได้ ขั้นตอนนี้เราจะทำการ setting IPV4 และ set ค่า metrix ของ wifi และ Ethernet ให้มีค่า = 15 (ตรวจสอบให้เรียบร้อยว่า wifi และ Ethernet มีการเชื่อมต่ออยู่) หลังจากนั้นให้เรา Alt+Rightclick ที่ Wifi และ Ethernet แล้ว set ค่า metrix ให้เท่ากับ 15 เหมือนกันเสร็จแล้วให้รอ identifying (หลังจากนี้ให้รอจนกว่า Bridgenet จะขึ้นเป็น wifi)
3.เมื่อเข้ามาแล้ว เราสามารถใช้ command line ของ ubuntu ได้ตามปกติ โดยเราจะติดตั้ง Docker ด้วย command ดังนี้

    sudo apt update
    (Update List ของ Package)

    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    (ติดตั้ง Package เบื้องต้น ที่อนุญาตให้ apt ใช้แพ็คเกจผ่าน HTTPS:)

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    (จากนั้นเพิ่ม Key GPG สำหรับที่เก็บ Docker เป็นแบบ official ในระบบของเรา)

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    (เพิ่มที่เก็บ Docker ไปยัง APT)

    sudo apt update
    (Update List ของ Package อีกครั้ง)

    apt-cache policy docker-ce

    sudo apt install docker-ce
    (Install Docker)

    sudo systemctl status docker
    (เมื่อติดตั้ง Docker แล้ว daemon จะเริ่มทำงาน เพื่อตรวจสอบว่า Docker กำลังทำงานอยู่:)

    Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-04-01 21:30:25 UTC; 22s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 7854 (dockerd)
      Tasks: 7
     Memory: 38.3M
        CPU: 340ms
     CGroup: /system.slice/docker.service
             └─7854 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

    Output จะเป็นประมาณนี้

4.หลังจากติดตั้ง Docker เรียบร้อยแล้วให้เราลงเครื่องมือที่จำเป็นสำหรับการใช้งานใน Docker ด้วย commandline
    sudo apt update
    sudo apt upgrade
    sudo apt-get install net-tools
    sudo apt-get install vim

    เป็นอันเสร็จขั้นตอนการติดตั้ง Docker



