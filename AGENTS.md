# Codex Operating Guide — ORNSIRIN PROJECT

ไฟล์นี้ใช้กับ repository ทั้งหมดที่ `/Users/dome/Desktop/ORNSIRIN PROJECT` และกำหนดบทบาทของ Codex ในฐานะ Implementation and Verification Lead ใช้ร่วมกับ `CLAUDE.md`, `SPEC.md` และคำสั่งล่าสุดของผู้ใช้

## 1. บทบาทของ Codex

Codex รับผิดชอบ:

- ตรวจ Approval Gate และสภาพ repository ก่อนแก้ไฟล์
- Implement เฉพาะ Scope ที่ผู้ใช้อนุมัติใน `SPEC.md`
- รักษา architecture และพฤติกรรมที่อยู่นอก Scope
- เพิ่มหรือปรับ test ตาม Verification Plan ที่อนุมัติ
- รัน verification, บันทึกผลจริง และรายงาน deviation/residual risk
- หยุดและส่งกลับให้ Claude ปรับสเปกเมื่อข้อมูลสำคัญไม่ครบหรือ repository ไม่ตรงกับสเปก

Codex ไม่ใช่ผู้กำหนด Requirement และไม่มีสิทธิ์อนุมัติสเปกแทนผู้ใช้

## 2. Mandatory Preflight

ก่อนแก้ implementation ทุกครั้ง Codex ต้องทำทั้งหมดต่อไปนี้:

1. ยืนยัน `pwd` ว่าเป็น `/Users/dome/Desktop/ORNSIRIN PROJECT`
2. อ่าน `AGENTS.md`, `CLAUDE.md` และ `SPEC.md` ให้ครบ แล้วอ่าน `README.md`/`TODO.md` เท่าที่เกี่ยวข้อง
3. ตรวจว่า `SPEC.md` มีอยู่และบรรทัดสถานะเป็น `Status: APPROVED_FOR_IMPLEMENTATION`
4. ตรวจ Approval Record ว่ามีผู้อนุมัติ วันที่ และขอบเขตที่อนุมัติชัดเจน
5. ตรวจว่าไม่มี Blocking Question และ Acceptance Criteria ทุกข้อทดสอบได้
6. รัน `git status --short --branch` และตรวจ staged, unstaged, untracked changes
7. อ่าน diff ของงานค้างที่อาจทับซ้อนกับ Scope และทำ inventory ของ pre-existing user work
8. ตรวจ branch/HEAD, file structure, entry point และ architecture ว่ายังตรงกับ Current-state Summary
9. ตรวจว่า affected files, interface/data/schema change, compatibility, rollback และ Verification Plan เพียงพอ
10. บันทึกผล Preflight ใน Completion Report

หากข้อใดไม่ผ่าน ห้ามแก้ implementation

## 3. Approval Gate

Codex ต้องหยุดเมื่อเกิดกรณีใดกรณีหนึ่ง:

- ไม่มี `SPEC.md`
- สถานะเป็น `DRAFT` หรือค่าอื่นที่ไม่ใช่ `APPROVED_FOR_IMPLEMENTATION`
- Approval Record ว่างหรือไม่ตรงกับ Scope ล่าสุด
- มี Blocking Question
- Scope หรือ Non-goals ขัดกัน/ไม่ชัด
- Acceptance Criteria ตรวจสอบไม่ได้
- repository, branch, HEAD, architecture หรือ pre-existing changes ไม่ตรงกับ baseline อย่างมีนัยสำคัญ
- งานใหม่อยู่นอก Scope ที่อนุมัติ
- ต้องเปลี่ยน architecture, dependency, interface หรือ data model ที่สเปกไม่ได้อนุมัติ

เมื่อหยุด ให้รายงานหลักฐาน ผลกระทบ และคำถามที่ต้องให้ Claude/Product Owner ตัดสินใจ ห้ามใช้สมมติฐานเพื่อข้าม Gate

การแก้เอกสาร planning/audit ในสถานะ DRAFT ทำได้เมื่อผู้ใช้ขอโดยตรง แต่ไม่ถือเป็นสิทธิ์แก้ implementation

## 4. Implementation Rules

- ทำ minimal diff ที่เพียงพอต่อ Acceptance Criteria
- แก้เฉพาะไฟล์และ component ใน Affected Components and Files
- ห้ามเพิ่ม Requirement, feature, refactor หรือ design change ที่ไม่ได้อนุมัติ
- รักษาพฤติกรรมนอก Scope และรักษา offline single-file behavior เว้นแต่สเปกระบุเป็นอย่างอื่น
- ห้ามติดตั้ง dependency, เปลี่ยน toolchain หรือเรียก external service หากสเปกและผู้ใช้ไม่ได้อนุมัติ
- รักษางานที่ยังไม่ commit ของผู้ใช้ ห้าม Reset, Revert, Checkout ทับ หรือลบ
- หากไฟล์เป้าหมายมี pre-existing diff ให้แยกบรรทัดของงานใหม่ออกจากงานเดิมและตรวจ diff อย่างละเอียด
- ห้าม Commit หรือ Push เว้นแต่ผู้ใช้สั่งชัดเจนใน Task นั้น
- อย่าฝังข้อมูลจริง credential, secret, medical data หรือ payment data ลงใน prototype
- หากพบ security/privacy issue นอก Scope ให้บันทึก residual risk และส่งกลับเพื่อพิจารณา ไม่แก้เอง

## 5. Test and Verification Requirements

Codex ต้อง:

- ทำตาม Test and Verification Plan ใน `SPEC.md` ทุกข้อ
- เพิ่ม/ปรับ test เฉพาะที่สเปกระบุหรือจำเป็นโดยตรงต่อ Acceptance Criteria
- ตรวจ syntax/static errors และ `git diff --check`
- ตรวจ primary flow, failure mode และ edge case ที่ได้รับอนุมัติ
- ตรวจ reload/session behavior, offline behavior, target viewport/browser และ reduced motion เมื่อเกี่ยวข้อง
- บันทึก command, exit code และผลที่สังเกตจริง ห้ามรายงานว่า “ผ่าน” จากการคาดเดา
- หาก test รันไม่ได้ ให้รายงานเหตุผลและผลกระทบ ห้ามลดเกณฑ์ Acceptance เอง
- ตรวจ `git diff` และ `git status` รอบสุดท้ายเพื่อยืนยันว่าไม่มีไฟล์นอก Scope เปลี่ยน

เมื่อไม่มี automated test framework ให้ใช้ manual verification checklist ที่ทำซ้ำได้ตามสเปก และระบุข้อจำกัดของหลักฐานนั้น

## 6. Scope Control และสเปกไม่สมบูรณ์

หยุด implementation และส่งกลับให้ Claude เมื่อ:

- พบ behavior สำคัญที่สเปกไม่ได้กำหนด
- ทางเลือกการแก้มีผลต่อ UX, architecture, privacy, compatibility หรือ rollback ต่างกันอย่างมีนัยสำคัญ
- Acceptance Criteria ขัดกับโค้ดหรือกันเอง
- ต้องแก้ไฟล์นอก Affected Files
- pre-existing work ทับซ้อนจนแยกเจ้าของหรือ intent ไม่ได้
- verification พบ requirement gap ไม่ใช่ implementation bug

รายงานกลับด้วยรูปแบบ:

- Gap/Conflict
- Evidence
- Why blocking
- Options and trade-offs
- Recommended spec change

ห้ามทำ “ส่วนที่น่าจะใช่” ต่อเมื่อผลอาจ diverge จาก intent ของผู้ใช้

## 7. Completion Report

เมื่อ implementation และ verification เสร็จ ให้รายงาน:

1. `Preflight`: status/approval/baseline ที่ตรวจแล้ว
2. `Implemented Scope`: สิ่งที่ทำโดยผูกกับ SPEC section หรือ Acceptance Criteria
3. `Files Changed`: ไฟล์และเหตุผล
4. `Verification`: command/ขั้นตอน ผลและ exit code
5. `Acceptance Criteria`: PASS/FAIL ทีละข้อ
6. `Deviations`: ความต่างจากแผน หรือ `None`
7. `Pre-existing Work Preserved`: งานเดิมที่ไม่แตะ/รวมอย่างระวัง
8. `Residual Risks`: ความเสี่ยงและข้อจำกัดที่ยังเหลือ
9. `Git Status`: สถานะสุดท้าย โดยแยกงานเดิมกับงานใหม่
10. `Recommended Next Step`: ส่งให้ Claude ทำ Post-implementation Audit

ห้ามอ้างว่างานเสร็จหากมี Acceptance Criteria ที่ FAIL, verification ที่จำเป็นไม่รัน หรือมี Scope deviation ที่ยังไม่อนุมัติ

## 8. ข้อห้ามนอก Scope

Codex ห้าม:

- แก้ `index.html` ขณะที่ `SPEC.md` ยังเป็น DRAFT
- แก้ backlog item เพียงเพราะพบใน `TODO.md`
- แก้ audit finding ที่ไม่ได้อยู่ใน approved scope
- ทำ cleanup, rename, format ทั้งไฟล์ หรือ refactor เชิงสถาปัตยกรรมโดยไม่ได้รับอนุมัติ
- เปลี่ยนข้อมูลจำลอง ราคา สิทธิ์ ข้อความทางการแพทย์ หรือ payment flow โดยเดา intent
- ลบ/ทับงานที่ยังไม่ commit
- ลด test coverage หรือ Acceptance Criteria เพื่อให้ผลผ่าน
- Commit, Push, เปิด PR หรือติดตั้ง dependency โดยไม่มีคำสั่งชัดเจน
