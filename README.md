# Thai Government Document Skills — `/thaidocreview` and `/reviewthaiofficialdoc`

Claude Skills สำหรับงานหนังสือราชการไทย ตาม **ระเบียบสำนักนายกรัฐมนตรีว่าด้วยงานสารบรรณ พ.ศ. 2526** (แก้ไขถึงฉบับที่ 4 พ.ศ. 2564) และมติ ครม. ที่เกี่ยวข้อง

Repo นี้บรรจุ **2 Skill** แยกกันชัด:

| Skill | คำสั่งเรียก | ทำอะไร |
|---|---|---|
| `thaidocreview` | `/thaidocreview` | ร่างหนังสือราชการใหม่ (6 ชนิดตามข้อ 9) |
| `reviewthaiofficialdoc` | `/reviewthaiofficialdoc` | ตรวจร่างหนังสือราชการที่มีอยู่แล้ว เปรียบเทียบกับมาตรฐาน |

ทั้งคู่ตั้ง `disable-model-invocation: true` ใน frontmatter แปลว่า **Claude จะไม่โหลด Skill นี้อัตโนมัติ** จากการเดาคำขอ — ผู้ใช้ต้องพิมพ์ slash command ที่ตรงเท่านั้น Skill จึงจะเข้ามาในบริบท

เหตุผล: หนังสือราชการเป็นรูปแบบเฉพาะวงราชการ ผู้ใช้ Claude ทั่วไปไม่ใช้ การให้ Skill auto-load จะเปลือง token และอาจ "context-poison" คำตอบในงานทั่วไป — จึงให้โหลดเฉพาะเมื่อตั้งใจเรียกใช้

## ติดตั้ง

วาง**ทั้งสองโฟลเดอร์**ที่ `~/.claude/skills/` แล้วเปิด Claude Desktop ใหม่:

```bash
mkdir -p ~/.claude/skills
cp -R thaidocreview ~/.claude/skills/thaidocreview
cp -R reviewthaiofficialdoc ~/.claude/skills/reviewthaiofficialdoc
```

ตรวจว่าทั้งคู่โหลดได้โดยเปิด Claude Desktop > Customize > Skills แล้วมองหารายชื่อ `thaidocreview` และ `reviewthaiofficialdoc`

## วิธีใช้

### สำหรับการร่างเอกสารใหม่

ใน Cowork พิมพ์:

```
/thaidocreview
ช่วยร่างหนังสือเชิญประชุมจากกรม X ถึงปลัดกระทรวง Y
```

Claude จะถาม Preflight (ชั้นข้อมูล → ชนิดหนังสือ → ฐานะผู้รับ) ก่อนเริ่มร่าง รายละเอียด decision flow อยู่ใน `thaidocreview/SKILL.md`

### สำหรับการตรวจเอกสารที่มีอยู่

แนบไฟล์ร่าง .docx หรือ .pdf หรือวางข้อความใน Cowork แล้วพิมพ์:

```
/reviewthaiofficialdoc
```

Claude จะตรวจตาม checklist (รูปแบบ, เลขไทย/อารบิก, คำขึ้นต้น/ลงท้าย, ข้อผิดพลาดที่พบบ่อย) แล้วรายงานเป็นรายข้อ

## ขอบเขตและข้อจำกัด

Skill เหล่านี้ทำเฉพาะ:

- ร่างหนังสือราชการ 6 ชนิดตามข้อ 9 ของระเบียบสารบรรณ
- ตรวจรูปแบบ คำขึ้นต้น คำลงท้าย สรรพนาม ตามภาคผนวก 2
- จัดรูปแบบเอกสาร (TH Sarabun PSK 16, ขอบกระดาษ 2.5/2/3/2 ซม., เลขไทย default)
- ตรวจข้อผิดพลาดที่พบบ่อยตามคู่มือ ก.พ. / OCSC

Skill เหล่านี้**ไม่**ทำ:

- จำแนกชั้นความลับหรือชั้นข้อมูลส่วนบุคคล (ส่งต่อ DPO/PDPA officer ของหน่วยงาน)
- ใส่เลขที่หนังสือหรือวันที่ออกเลขจริง (เป็นหน้าที่ของงานสารบรรณกลางตามข้อ 43)
- รับรองความปลอดภัยของแพลน Cowork ใด ๆ สำหรับข้อมูลประชาชน
- ลงนามแทนผู้มีอำนาจ — ทุกร่างต้องผ่านการตรวจของผู้บังคับบัญชาก่อนเสนอลงนาม

## โครงสร้าง repo

```
Writing-doc-skill/
├── README.md                       # ไฟล์นี้
├── thaidocreview/                  # Skill 1: ร่างหนังสือ
│   ├── SKILL.md                    # Frontmatter + decision flow + format rules
│   ├── references/                 # ข้อมูลอ้างอิงที่ Claude อ่านตามต้องการ
│   │   ├── 01-types.md             # คำอธิบายหนังสือทั้ง 6 ชนิด
│   │   ├── 02-format.md            # ฟอนต์ ขอบกระดาษ ระยะห่าง
│   │   ├── 03-thai-numerals.md     # กฎเลขไทย/อารบิก
│   │   ├── 04-salutations.md       # ภาคผนวก 2 — คำขึ้นต้น/ลงท้าย/สรรพนาม
│   │   ├── 05-numbering.md         # เลขที่หนังสือ + วันที่
│   │   ├── 06-data-safety.md       # คำตอบ deferral สำหรับข้อมูลส่วนบุคคล
│   │   ├── 07-regulation-index.md  # บัญชีอ้างอิง + URL ราชกิจจา
│   │   ├── 08-common-errors.md     # ข้อผิดพลาดที่พบบ่อย (Round 2)
│   │   ├── 09-urgency-classification.md  # ด่วน + ลับ markings (Round 2)
│   │   ├── 10-agency-codes.md      # ภาคผนวก 1 รหัสหน่วยงาน (Round 2)
│   │   └── 11-royal-letters.md     # ราชาศัพท์ basics (Round 2)
│   ├── templates/                  # โครงร่างแต่ละชนิด (6 ไฟล์)
│   └── examples/                   # ตัวอย่างกรอกเสร็จ (7 ไฟล์ + sample-prompts)
│
└── reviewthaiofficialdoc/          # Skill 2: ตรวจหนังสือ
    ├── SKILL.md                    # Review flow + checklist
    ├── checklist.md                # Step-by-step review checklist
    └── errors-quick-reference.md   # Quick lookup ข้อผิดพลาดบ่อย (subset of 08)
```

## ทดสอบหลังติดตั้ง

รัน prompts ใน `thaidocreview/examples/sample-prompts.md` ใน Cowork — ดูว่าพฤติกรรมตรงกับ "expected" หรือไม่ ถ้าข้อใดไม่ผ่าน เปิดไฟล์ reference/template ที่ระบุไว้ในข้อนั้นเพื่อแก้

## License

ใช้ภายในหน่วยงานราชการไทยได้โดยเสรี เนื้อหากฎหมาย/ระเบียบที่อ้างถึงเป็นของทางราชการอยู่แล้ว แหล่งที่มาทุกข้อระบุ URL ใน `thaidocreview/references/07-regulation-index.md`
