# Strapi คืออะไร?
Strapi เป็นระบบจัดการเนื้อหา (Content Management System หรือ CMS) ที่ใช้สำหรับการสร้างและจัดการ API (Application Programming Interface) ให้ง่ายโดยที่ไม่จำเป็นต้องเขียนโค้ดจากศูนย์
## Use case
* ให้บริการ API สำหรับการดึงข้อมูลและจัดการข้อมูล
* จัดการข้อมูลสินค้า หมวดหมู่ และคำสั่งซื้อ
* สร้างระบบจัดการงาน และการติดตามสถานะต่างๆ
## องค์ประกอบของ Strapi
ฟีเจอร์หลักๆของ Strapi จะมีด้วยกับ 2 ตัว คือ `content-type builder` กับ `content manager` 
- `content-type builder`
**ตัวช่วยในการออกแบบโครงสร้างของข้อมูล** ซึ่งข้อมูลของเราในการจัดเก็บลง Database จะมีชนิดของข้อมูลหลายอย่างเช่น (Text, Boolean, Number, JSON, Media, UID, etc.) สามารถทำการออกแบบและสร้าง API ไปในเวลาเดียวกันได้
แบ่งออกได้ 2 ประเภท คือ
	- Collection Type ใช้สำหรับจัดการข้อมูลที่มีหลายรายการ มีการค้นหาและกรองข้อมูลที่หลากหลาย เช่น User
	- Single Type ใช้สำหรับจัดการข้อมูลที่มีเพียงรายการเดียว มีการจัดการข้อมูลที่เป็น global settings หรือข้อมูลที่ไม่เปลี่ยนแปลงบ่อย
- `content manager`
เป็น **ระบบจัดการเนื้อหา** หลังจากที่เราสร้างโครงสร้างของข้อมูลจาก `content-type builder`แล้ว เราสามารถจัดการเนื้อหา สร้าง, แก้ไข, อ่านและลบ ของเราได้ โดยการ Input ข้อมูลต่างๆลงไป ก็จะเปลี่ยนไปตามชนิดของข้อมูลที่เราออกแบบ และยังสามารถค้นหาข้อมูลด้วยการกรองได้
ref : [Strapi อย่างง่าย. ความสามารถต่างๆ ของ Strapi… | by Dewo D. Benedix | Medium](https://medium.com/@c.panuwong/strapi-%E0%B8%AD%E0%B8%A2%E0%B9%88%E0%B8%B2%E0%B8%87%E0%B8%87%E0%B9%88%E0%B8%B2%E0%B8%A2-bf1ab74664d1)
## Installation
### ความต้องการของระบบก่อนติดตั้ง
* Node.js (v.18 หรือ v.20 )
* npm (v.6 หรือมากว่า)

### เริ่มสร้างโปรเจค
เข้าโฟลเดอร์ที่สร้างไว้สำหรับโปรเจค
 1. เปิด Terminal แล้วพิมพ์คำสั่ง
	``` bash
	    npx create-strapi-app@latest my-project
	```
    
 2. เลือกรูปแบบการติดตั้ง
	- เลือก `Quickstart (recommended)`
	- หรือ `Custom (manual setting)`
 3. Strapi Cloud login
	 - ถ้ามีเลือก `Login/signup`
	 - ถ้าไม่มีเลือก `skip`
 4. Run Strapi
 พิมพ์คำสั่ง
	```bash
		npm run develop
	```
## AWS Deployment
 ### สร้าง AWS Instance
 1. จากในหน้า**AWS Management Console** 
	-   `Find Services`, search for  `ec2`  and click on  `EC2`
2.  กดปุ่ม  `Launch Instance`  สีส้ม
	-   `Select`  **Ubuntu**
	- **Instance Type** เลือกเป็น `t2.small`หรือใหญ่กว่านั้น
	- **Security Group**
		-	**Type:**  `SSH`,  **Protocol:**  `TCP`,  **Port Range**  `22`,  **Source:**  `::/0`
		-   **Type:**  `HTTP`,  **Protocol:**  `TCP`,  **Port Range**  `80`,  **Source:**  `0.0.0.0/0, ::/0`
		-   **Type:**  `HTTPS`,  **Protocol:**  `TCP`,  **Port Range**  `443`,  **Source:**  `0.0.0.0/0, ::/0`
		-   **Type:**  `Custom TCP Rule`,  **Protocol:**  `TCP`,  **Port Range**  `1337`,  **Source:**  `0.0.0.0/0` 
3. **Key pair**
	- เลือก `create new key pair` แล้วตั้งชื่อ key pair

### Set up Ubuntu
1. Connect Instance ด้วย SSH
2. พิมพ์คำสั่ง
	``` bash
	cd ~
	sudo apt-get update
	...#เพื่ออัพเดตระบบ
	sudo apt-get install -y ca-certificates curl gnupg
	...#ติดตั้ง curl
	sudo mkdir -p /etc/apt/keyrings
	curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
	NODE_MAJOR=20
	sudo apt-get update
	...
	sudo apt-get install nodejs -y
	...#ติดตั้ง Node.js
	node -v && npm -v
	```
3. สร้าง `.npm-global` เพื่อเป็น `node_module`
	```bash
	cd ~
	mkdir ~/.npm-global
	npm config set prefix '~/.npm-global'
	```
4. modify `~/.profile`
	```bash
	sudo nano ~/.profile
	```
	```bash
	#และนำข้อความนี้ไปวางไว้ล่างสุด
	export PATH=~/.npm-global/bin:$PATH
	```
5. update `~/.profile`
	```bash
	source ~/.profile
	```
### Git
1.  ใช้คำสั่ง  `git clone <url>`  ดึงโปรเจ็คจาก GitHub Repository
2.  `cd` เข้าไปยังโฟลเดอร์ที่ได้มา
	```bash
	cd ./my-project/
	npm install
	NODE_ENV=production npm run build
	```
3. modify `.env`
	```bash
	nano .env
	```
	แล้ว copy `.env` จากเครื่องเราไปใส่และพิมพ์ในบรรทัดล่างสุด
	```bash
	NODE_ENV=production
	```
	จากนั้น `ctrl O`, `Enter`, `ctrl X`
4. Start Strapi
	```bash
	npm start
	```