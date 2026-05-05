# Thai Government Document Writer (Skill)

Claude Skill สำหรับร่างหนังสือราชการไทยให้ถูกต้องตาม **ระเบียบสำนักนายกรัฐมนตรีว่าด้วยงานสารบรรณ พ.ศ. 2526** (แก้ไขถึงฉบับที่ 4 พ.ศ. 2564) และมติคณะรัฐมนตรีที่เกี่ยวข้อง

## ติดตั้ง

วางทั้งโฟลเดอร์นี้ที่ `~/.claude/skills/thai-gov-document-writer/` แล้วเปิด Claude Desktop ใหม่ Cowork จะโหลด Skill อัตโนมัติเมื่อพบคำขอร่างหนังสือราชการ

```bash
# macOS / Linux
mkdir -p ~/.claude/skills
cp -R . ~/.claude/skills/thai-gov-document-writer
```

ตรวจว่า Skill โหลดได้โดยเปิด Claude Desktop > Customize > Skills แล้วมองหา `thai-gov-document-writer`

## ขอบเขต

Skill นี้ทำเฉพาะ:

- ร่างหนังสือราชการ 6 ชนิดตามข้อ 9 ของระเบียบสารบรรณ
- ตรวจรูปแบบ คำขึ้นต้น คำลงท้าย สรรพนาม ตามภาคผนวก 2
- จัดรูปแบบเอกสาร (TH Sarabun PSK 16, ขอบกระดาษ 2.5/2/3/2 ซม., เลขไทย default)

Skill นี้**ไม่**ทำ:

- จำแนกชั้นความลับหรือชั้นข้อมูลส่วนบุคคล (ส่งต่อ DPO/PDPA officer ของหน่วยงาน)
- ใส่เลขที่หนังสือหรือวันที่ออกเลขจริง (เป็นหน้าที่ของงานสารบรรณกลางตามข้อ 43)
- รับรองความปลอดภัยของแพลน Cowork ใด ๆ สำหรับข้อมูลประชาชน

## โครงสร้างไฟล์

```
thai-gov-document-writer/
├── SKILL.md                    # Decision flow + format rules
├── README.md                   # ไฟล์นี้
├── references/                 # ข้อมูลอ้างอิงที่ Claude อ่านตามต้องการ
│   ├── 01-types.md             # คำอธิบายหนังสือทั้ง 6 ชนิด
│   ├── 02-format.md            # ฟอนต์ ขอบกระดาษ ระยะห่าง
│   ├── 03-thai-numerals.md     # กฎเลขไทย/อารบิก
│   ├── 04-salutations.md       # ภาคผนวก 2 — ตารางคำขึ้นต้น/ลงท้าย/สรรพนาม
│   ├── 05-numbering.md         # เลขที่หนังสือ + วันที่ (placeholder rules)
│   ├── 06-data-safety.md       # คำตอบ deferral สำหรับข้อมูลส่วนบุคคล
│   └── 07-regulation-index.md  # บัญชีรายการอ้างอิงระเบียบ + URL ราชกิจจา
├── templates/                  # โครงร่างแต่ละชนิด
│   ├── 01-external.md
│   ├── 02-internal.md
│   ├── 03-stamped.md
│   ├── 04-directive.md
│   ├── 05-announcement.md
│   └── 06-evidence.md
└── examples/
    └── sample-prompts.md       # 5 prompts ตรวจสอบหลังติดตั้ง
```

## ทดสอบหลังติดตั้ง

รัน prompts ทั้ง 5 ใน `examples/sample-prompts.md` ใน Cowork ดูว่าพฤติกรรมตรงกับ "expected" หรือไม่ ถ้าข้อใดไม่ผ่าน เปิดไฟล์ reference/template ที่ระบุไว้ในข้อนั้นเพื่อแก้

## License

ใช้ภายในหน่วยงานราชการไทยได้โดยเสรี เนื้อหากฎหมาย/ระเบียบที่อ้างถึงเป็นของทางราชการอยู่แล้ว
