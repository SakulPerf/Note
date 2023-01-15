# Features
1. Mobile map
	1. GPS search
	1. Routing
	1. Navigation
1. ระบบช้า
	1. Waiting dialog ขึ้นพร่ำเรื่อ
	1. Progressbar
1. Security {PIN, Face, Finger, Voice}
1. Zip มีขนาดใหญ่
1. ทำพฤติกรรมของ Mana URL แบบ Medium
1. รองรับหลายภาษา 🔥🎨
	1. รองรับหน้าเปลี่ยนตามประเทศ ไม่ได้เปลี่ยนแค่ภาษา
	1. รองรับการแสดงผลเวลา
	1. ผู้ใช้คนนั้นๆอยู่ประเทศไหน เพื่อเลือก country code
		1. Idea1: หาวิธีทำ automation
		1. Idea2: ออกแบบ UI ให้ใช้งานง่ายขึ้น (ex.filter)
1. BA Header 🎨
1. Consent form 🎨
1. หน้าคั่น 🎨
1. Option dialog 🎨
	1. เลือกไอเท็มโดยไม่ต้องกดปิด
	1. เลือกได้หลายรายการ
	1. Re-order, Delete, Filtering
1. Consent Management 🎨
1. Dialog คืนเงิน 🎨❓
1. Voice call
1. Automation testing (E2E)
1. เราต้องมี API version เพื่อจัดการกรณีมี breaking change 🔥
1. Login with username & password
1. แยก ctx เป็นของ 3rd กับ mana โดยดูจาก prefix endpoint
1. Club เพื่อ register api 🔔

# Technical debt
1. MContentVmBase's injections (GetServiceCommand)

# Operation design 🎨
1. User หลบการ Freeze account
1. ลืมรหัสผ่าน
1. เอาแอพขึ้น
	1. แอพมีการอัพเดท
	1. ปิดปรับปรุงชั่วคราว
	1. มีปัญหาต้องลงแอพใหม่

# Bugs
1. Sync GPS header
1. ปุ่มใน toolbar กดยาก
1. หน้า Home ไม่โหลด feed (บางครั้ง)
1. ปุ่มถูก disable ตลอดกาล ❓
1. IoC
1. IDP Token endpoint มีปัญหาบางทีออก token ไม่ได้ 🔥
	1. Create new user
	1. Refresh token
1. ผู้ใช้กดปุ่มซ้ำแล้วเปิดหน้าเบิล (delay 2s)
1. Result dialog ไม่แสดงผลซ้ำ (delay 30s)

# Release Eng
1. Staging server
1. Endpoint
1. JS deserializer error (newline)
1. BA ฺHeader
1. ถอด appcenter ออก ย้ายไปใช้ azure devops
	* Step 1 ย้ายการ build android ก่อน (ใช้ tracing บน appcenter เหมือนเดิม)
	* Step 2 ย้าย tracing
1. ย้าย IDP ไปอยู่ใน K8S
1. อยากให้ Testflight เป็นตัวสุดท้ายที่ถูก build หลังจากนั้นเปลี่ยน zip เอา
1. 3rd party pipeline


# IDP
1. Signout page
1. แยก config 3rd party ออกจากกัน
	1. 3rd party register (client id)
	1. การอ่านค่าจาก permanent storage ใน IDP จะทำยังไง
1. Review code
	1. Configurations (Scopes, Grants, Clients, RefreshToken)
	1. User consent (allow anonymous)
	1. Refresh token (🔥 ต้องทำตัวนี้ก่อนที่จะเปิดให้ 3rd เข้ามาใช้)
1. Mana sandbox login
1. GetKeyM🔥
1. PILOT: Callback ตอนนี้ register ได้แค่ 1 ตัว เราจะเปิดช่องให้เขา reg เพิ่มไหม แบบ google?
1. PILOT: IDP จะส่ง jobtitle ทั้งหมดมาด้วย เช่น Manager, Operator, Rider
1. PILOT: ไปดูเรื่อง token ที่ออกให้ 3rd party สามารถเอาไปใช้ login ข้ามไปที่อื่นได้หรือไม่ (🔥 ต้องทำตัวนี้ก่อนที่จะเปิดให้ 3rd เข้ามาใช้)
	* Idea ตอนนี้ปล่อยไปก่อนได้ ง่ายสุดให้เช็คจาก
		1. ส่ง role เข้าไปใน job token (เราต้องไป lookup เสมอ เลยเลือกที่จะไม่ส่ง)
		2. ส่ง id ของ job token ไปดึงว่าเป็น role อะไร (เราปัดตกเพราะมันต้อง call เสมอ)
		3. ถ้าเขาเป็น operator เราจะบันทึกลง redis แล้วให้ 3rd check จาก redis แทนการ call sv

# Playground
1. setup DA ยาก
	1. เราไม่เคยคุยกันว่าถ้าจะเดฟจริงๆ กระบวนการสร้าง DA, Servic จะต้องเป็นยังไงบ้าง และต้องมีการ register อะไรเพิ่มเติมไหม เช่น employee
	1. ไอเดีย กรณีเดฟภายในของ mana อาจมีการสร้าง DA กลาง ให้ทีมเราสามารถใช้งานได้ง่ายๆ
1. API ใน server จริงเปลี่ยน ซึ่งเป็น api set เดียวกับที่ playground ใช้ เราจะแก้ยังไง?
	1. ไอเดีย ใน shared lib อาจจะมี contract ที่ใช้ร่วมกันทั้ง server & playground
1. หา Endpoint/McId ยาก
	1. ไอเดีย น่าจะมีช่องทางให้ dev สามารถเรียก API เพื่อแสดงของที่ register ได้ง่ายๆ (เพราะหลังจาก register เสร็จแล้ว 3rd อาจจะลืมก็ได้)
	1. ไอเดีย ให้ zip tools หรือ playground สามารถ list endpoint ทั้งหมดในไฟล์ zip ออกมาได้ (3rd ไม่ควรเรียกใช้ของที่เกี่ยวกับ zip ได้)
	1. ไอเดีย ให้ DA มานะเป็นตัวช่วยในการจัดการ mcontent ต่างๆของ mana ให้สามารถจัดการได้ง่ายๆ
1. การส่งข้อมูลข้ามหน้า
	1. กรณีมีหน้า A ส่งข้อมูลให้หน้า B -> เราจะไม่สามารถทำงานกับหน้า B ได้ถ้าไม่มีหน้า A หรือ WF ของหน้า A มาก่อน
1. Submit form
	1. ลืมวิธีการเรียกใช้งานคำสั่งต่างๆ เช่น Init, Submit, ....
1. ถ้าไม่มี WF เก่า ทีมต้องสร้าง WF เป็น

# Zip tools
1. หลายครั้งที่แอพมีปัญหาแล้วเป็นเพราะ build zip ผิด หรือ apptext พัง อยากให้ zip tool มีการเช็คเรื่องนี้ด้วย (yaml)

# Remark
1. ที่อยู่
	1. เราจะย่อคำว่า "ตำบล" และ "อำเภอ" เป็น "อ." กับ "ต." และข้อมูล ตำบล, อำเภอ จะต้องมีคำย่อนำหน้าเสมอ เช่น "ตำบลศิลา" จะถูกแก้เป็น "ต.ศิลา""อำเภอเมือง" จะถูกแก้เป็น "อ.เมือง"
	1. เราจะตัดคำว่า "จังหวัด" และ "จ." ทิ้ง เช่น "จังหวัดขอนแก่น" จะถูกแก้เป็น "ขอนแก่น" "จ.ขอนแก่น" จะถูกแก้เป็น "ขอนแก่น"
	1. "แขวง" และ "เขต" จะใช้เฉพาะใน กรุงเทพ โดยกำหนดให้
		* ต. เทียบเท่า แขวง
		* อ. เทียบเท่า เขต
		* เช่น "เขตคลองเตย แขวงคลองตัน"
