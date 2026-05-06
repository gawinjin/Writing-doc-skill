---
name: reviewthaiofficialdoc
description: Opt-in skill loaded only when the user explicitly invokes /reviewthaiofficialdoc. Reviews an existing draft of หนังสือราชการไทย against ระเบียบสารบรรณ 2526 and common-error checklists. Do NOT auto-load — Thai official writing style is used only inside the government circle.
disable-model-invocation: true
---

# Review Thai Official Document

Skill นี้ตรวจร่างหนังสือราชการที่ผู้ใช้แนบมา แล้วรายงานข้อผิดพลาดเป็นรายข้อ พร้อมข้อแนะนำแก้ไข

## ขั้นตอน

1. **รับร่าง** — ผู้ใช้แนบไฟล์ (.docx/.pdf/.md) หรือวางข้อความ
2. **จำแนกชนิดหนังสือ** — ดูจาก header / โครงสร้าง ว่าเป็นหนึ่งใน 6 ชนิดตามข้อ 9 ของระเบียบสารบรรณ
3. **รัน checklist** ตาม `checklist.md` — ตรวจรูปแบบ → เลขไทย/อารบิก → คำขึ้นต้น/ลงท้าย → โครงสร้าง → ลายมือชื่อ
4. **เปรียบเทียบกับ common errors** ที่บันทึกใน `errors-quick-reference.md`
5. **รายงานผล** เป็นตาราง: หมวด | ข้อพบ | คำแนะนำ | ความรุนแรง (สำคัญ/ปานกลาง/แต่ง)

## ขอบเขต

ตรวจ:
- รูปแบบ (ฟอนต์ ขอบกระดาษ ระยะห่าง)
- การใช้เลขไทย/อารบิก ตามมติ ครม. 2 พ.ค. 2543
- คำขึ้นต้น/สรรพนาม/คำลงท้าย ตามภาคผนวก 2 (ถ้ารู้ฐานะผู้รับ)
- โครงสร้างเนื้อหา (ตามที่ + จึงเรียนมาเพื่อ + ฯลฯ)
- ลายมือชื่อ + ตำแหน่ง
- เลขที่หนังสือเป็น placeholder หรือไม่
- ข้อผิดพลาดที่พบบ่อยตาม `errors-quick-reference.md`

ไม่ตรวจ:
- ความถูกต้องเชิงข้อเท็จจริงของเนื้อหา
- ความสอดคล้องกับนโยบาย/มติ ครม. ที่อ้างถึง
- ความปลอดภัยของข้อมูล (ส่ง DPO ของหน่วยงาน)

## Loading map

| สถานการณ์ | ไฟล์ที่อ่าน |
|---|---|
| ทุกครั้ง | `checklist.md` + `errors-quick-reference.md` |
| ตรวจคำขึ้นต้น/ลงท้าย | (ดูใน thaidocreview/references/04-salutations.md ถ้าได้ติดตั้ง) |
| ตรวจรูปแบบ | (ดูใน thaidocreview/references/02-format.md ถ้าได้ติดตั้ง) |

## Output contract

รายงานเป็นตารางเดียว 4 คอลัมน์: **หมวด | สิ่งที่พบ | คำแนะนำ | ระดับความสำคัญ**

ระดับความสำคัญ:
- **สำคัญ (must-fix)** — ผิดระเบียบหรือผิดมติ ครม. ส่งไม่ได้
- **ปานกลาง (should-fix)** — ผิดธรรมเนียม/ไม่เป็นทางการ ควรแก้
- **แต่ง (nice-to-have)** — ดีขึ้นได้แต่ไม่ผิด
