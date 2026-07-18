Status: APPROVED_FOR_IMPLEMENTATION

# ORNSIRIN PROJECT — Workflow Baseline and Draft Implementation Specification

## Document Control

| Field | Value |
|---|---|
| Product Owner / Approver | ผู้ใช้ |
| Planning and Audit Lead | Claude |
| Implementation and Verification Lead | Codex |
| Repository | `/Users/dome/Desktop/ORNSIRIN PROJECT` |
| Baseline branch | `main` tracking `origin/main` |
| Baseline HEAD | `818249b` — `Color-coded hero headers bring the secondary screens to life` |
| Baseline audit date | 2026-07-18 (Asia/Bangkok) |
| Phase 1 architecture audit date | 2026-07-18 (Asia/Bangkok, late evening) |
| Specification version | 0.3 |
| Blocking Questions | NONE blocking for INC-F (BQ-001/BQ-002 resolved 2026-07-18; ดู §18) |
| Implementation authorization | GRANTED — เฉพาะ INC-F ตาม §5.3 (อนุมัติ 2026-07-19) |

> Approval Gate: **อนุมัติแล้ว (2026-07-19)** — ขอบเขตที่อนุมัติคือ **INC-F ตาม §5.3 เท่านั้น** · Codex ต้องทำ Mandatory Preflight ตาม `AGENTS.md` ก่อนเริ่ม และต้องยืนยันว่า **precondition ผ่านแล้ว: งานค้าง celebration (47+/2− ใน `index.html`) ถูกผู้ใช้ commit เรียบร้อย** — ณ เวลาอนุมัติ งานนี้ยังไม่ถูก commit (ดู Approval Record) หากยังไม่ commit ห้ามแตะ `index.html` · งานใดนอก §5.3 ยังไม่ได้รับอนุญาต · Claude ไม่มีสิทธิ์แก้ implementation ตามคำสั่งผู้ใช้

## 1. Objective

Phase 0 มีเป้าหมายสร้างระบบทำงานร่วมกันระหว่าง Claude, Codex และผู้ใช้ พร้อมบันทึก baseline ของ ORNSIRIN PROJECT ที่ตรวจสอบย้อนกลับได้ เพื่อให้การเปลี่ยน application ครั้งถัดไปมี Scope, Acceptance Criteria และ Verification Plan ที่ผู้ใช้อนุมัติก่อนลงมือ

เอกสารฉบับนี้ยังไม่เลือก feature หรือ defect ใดให้ Codex implement

## 2. User-visible Outcome

Phase 0 ไม่มีการเปลี่ยนแปลงที่ผู้ใช้เดโมมองเห็น พฤติกรรมและหน้าตาของ `index.html` ต้องคงเดิมทุกประการ

ผลลัพธ์ที่ผู้ใช้ได้รับใน Phase 0 คือ:

- กติกา Claude ใน `CLAUDE.md`
- กติกา Codex ใน `AGENTS.md`
- Draft specification, audit baseline และรายการคำถามใน `SPEC.md`
- การรักษางานค้างเดิมใน `index.html` โดยไม่แก้ ทับ ลบ reset หรือ revert

## 3. Current-state Summary

### 3.1 Repository structure at Phase 0 start

ไฟล์ application/documentation ที่ `rg --files` พบก่อนสร้างเอกสาร Phase 0:

```text
README.md
TODO.md
index.html
```

ไฟล์กำกับเพิ่มเติม:

```text
.gitattributes
.gitignore
.claude/settings.local.json  # local, ignored
```

ไม่พบ `package.json`, build configuration, automated test suite หรือ asset directory แยกต่างหาก

### 3.2 Application baseline

- `README.md` ระบุว่าเป็น Ornsirin Health Link prototype สำหรับนำเสนอ ไม่ใช่ระบบจริง และใช้ข้อมูลจำลอง
- Application ทั้งหมดอยู่ใน `index.html` แบบ single-file offline bundle
- `index.html` ณ baseline มี 2,231 บรรทัด, 1,831,354 bytes
- SHA-256 ณ ก่อนสร้างเอกสาร: `f471e803cb581ecd85bbeca17a06c373ab85f09c4cde19bd8ef81c3e2a00b976`
- Tailwind runtime, Lucide, Noto fonts, logo และ QR ถูกฝังในไฟล์
- ไม่มี external `<script src>`, `<link>` หรือ HTTP(S) asset reference ที่ application markup เรียกใช้โดยตรงจาก static scan
- State ของเดโมอยู่ใน memory และ persist ไป `sessionStorage` ภายใต้ key `ornsirin-demo-state-v1`
- ผู้ใช้ตัวอย่าง ราคา สิทธิ์ รายการบริการ แพทย์ และ provider เป็นข้อมูล hardcoded

### 3.3 Git baseline and protected pre-existing work

สถานะก่อนสร้างเอกสาร:

```text
## main...origin/main
 M index.html
```

Uncommitted diff ใน `index.html` มี `47 insertions(+), 2 deletions(-)` และ `git diff --check -- index.html` ผ่าน งานค้างนี้เพิ่ม success celebration/count-up behavior บริเวณ animation CSS, `successModal()` และ `render()`

ข้อกำหนดการรักษางานเดิม:

- ถือ diff นี้เป็น pre-existing user work
- Phase 0 ห้ามแก้หรือจัดรูปแบบ `index.html`
- ห้าม Reset, Revert, Checkout ทับ, stage, commit หรือ push งานนี้
- Implementation รอบถัดไปต้องตรวจ baseline ใหม่และถามผู้ใช้ว่างานนี้เป็น accepted baseline หรือยัง

### 3.4 Phase 1 baseline re-verification (2026-07-18)

Claude รัน survey ตาม `CLAUDE.md` §3 ซ้ำใน Phase 1 และยืนยันว่า baseline ของ Phase 0 ยังเป็นปัจจุบันทุกค่า:

| Check | Command | Result |
|---|---|---|
| Working directory | `pwd` | `/Users/dome/Desktop/ORNSIRIN PROJECT` |
| Branch/HEAD | `git status --short --branch` | `main...origin/main` (sync), HEAD `818249b` |
| Staged changes | `git diff --cached --stat` | ว่าง |
| Unstaged changes | `git diff --stat` | `index.html` เท่านั้น: 47 insertions, 2 deletions |
| Untracked | `git status --short` | `AGENTS.md`, `CLAUDE.md`, `SPEC.md` |
| Whitespace hygiene | `git diff --check` | exit 0 |
| Inventory | `rg --files --hidden -g '!.git/**'` | 8 ไฟล์ ตรงตาม §3.1 บวกเอกสาร workflow 3 ไฟล์ |
| Integrity | `shasum -a 256 index.html` | `f471e803…a00b976` ตรง baseline |
| Size | `wc -l index.html` | 2,231 บรรทัด |

Evidence anchors ของ Findings เดิมถูก spot-check แล้วยังชี้ตำแหน่งถูกต้อง: AUD-001 (`userBar()` logged-out branch ~822-839), AUD-002 (`addPetSave` handler ~2100-2123, pet card template ~1451-1462), AUD-007 (`persistState()/hydrateState()` ~645-655), AUD-008 (login inputs ~826-834)

ไม่มีการแตะ `index.html`, ไม่มี commit/push/reset/revert/checkout และไม่ติดตั้ง dependency ใดใน Phase 1

### 3.5 Existing backlog

`TODO.md` ระบุว่า P0 เสร็จทั้งหมด และยังมีรายการที่ไม่เสร็จ ได้แก่:

- P1: ตัวควบคุมคิวสำหรับผู้พรีเซนต์
- P2: กระดิ่งแจ้งเตือน
- P2: หน้าผลตรวจแล็บ
- P2: ซ่อนข้อมูลส่วนตัวก่อนล็อกอิน
- P2: โหมดผู้สูงอายุของคุณยายบุญมี
- P2: หน้าสถิติฝั่งผู้ประกอบการ

รายการ backlog เป็น candidate เท่านั้น ไม่ถือว่าได้รับอนุมัติให้ implement

## 4. Architecture and Execution Flow

### 4.1 Static architecture

| Layer | Location | Responsibility |
|---|---|---|
| Embedded dependencies | `index.html:8-345` | Tailwind runtime, Lucide และ embedded font data |
| Application CSS | `index.html:346-403` | Theme, layout, animation, reduced-motion behavior |
| Shell DOM | `index.html:405-423` | Phone frame, status bar, user bar, app root, modal root |
| Hardcoded domain data | `index.html:433-640` | Users, prices, services, pets, doctors, provider config |
| State/persistence helpers | `index.html:642-658` | Save/hydrate state through `sessionStorage` |
| Screen/template functions | `index.html:768-1758` | Home, consult, queue, call, lab, meds, lifestyle and overlays |
| Render/event layer | `index.html:1759-2204` | Route-to-template, `innerHTML`, animations and event wiring |
| Bootstrap/runtime helpers | `index.html:2206-2228` | Hydrate, initial render, frame fitting and clock interval |

### 4.2 Control flow

```text
Browser loads index.html
  -> embedded libraries and CSS initialize
  -> hydrateState() merges sessionStorage into default state
  -> render() chooses a screen template from state.screen
  -> template string is assigned to #app / #modal-root via innerHTML
  -> render() attaches event listeners to the newly-created DOM
  -> user action or timer changes state
  -> setState() persists state and calls render() again
```

Special timer paths:

- Queue screen starts/stops `queueTimer` based on screen/overlay state
- VDO call screen starts/stops connection and call-duration timers
- Clock updates every 15 seconds
- Payment confirmation uses a delayed verification state before success

### 4.3 Data flow

```text
Hardcoded data + user form input + timer output
  -> mutable global state object
  -> sessionStorage JSON snapshot
  -> screen/modal template functions
  -> innerHTML rendering
  -> user-visible prototype
```

ไม่มี backend, database, real authentication, real payment verification หรือ real VDO service ใน repository ปัจจุบัน

## 5. Scope

### 5.1 In scope for Phase 0

- ตรวจ path, README, TODO, Git status, structure และ uncommitted changes
- บันทึก Architecture, Control Flow และ Data Flow ระดับ baseline
- บันทึก Audit Findings พร้อม evidence/severity/state
- สร้าง `CLAUDE.md`, `AGENTS.md` และ `SPEC.md`
- กำหนด Approval Gate, roles, Acceptance Criteria และ Verification Plan สำหรับ workflow
- รวบรวม Blocking Questions สำหรับผู้ใช้

### 5.2 Candidate implementation increments (Phase 1 menu — ยังไม่มีข้อใดถูกอนุมัติ)

ทุกรายการด้านล่างสืบทอดจาก backlog ใน `TODO.md`, Audit Findings หรือคำขอของผู้ใช้ก่อนหน้า ไม่มีการขยาย scope ใหม่โดย Claude · ผู้ใช้เลือกได้ **ครั้งละหนึ่ง increment** (ตอบใน BQ-002) แล้ว Claude จะขยาย increment ที่เลือกเป็น full Scope / Acceptance Criteria / Verification Plan ก่อนขออนุมัติจริง

| ID | Increment | ที่มา | ขนาดโดยประมาณ | บริเวณที่กระทบ (function/section) |
|---|---|---|---|---|
| INC-0 | Verify + accept งานค้าง success celebration เป็น baseline (แล้วผู้ใช้ commit เอง) | AUD-005 / BQ-001 | S (verification เท่านั้น) | ไม่แก้โค้ด — รัน checklist ใน §14.2 |
| INC-A | ตัวควบคุมคิวสำหรับผู้พรีเซนต์ (แบนเนอร์กลับเข้าคิว + คีย์ลัดเลื่อนคิว) | TODO P1 / AUD-004 | M | `homeScreen()`, `queueScreen()`, `startQueueTimer()`, keydown wiring ใน bootstrap |
| INC-B | ซ่อนข้อมูลส่วนตัวก่อนล็อกอิน (login เป็น privacy gate จริง) | TODO P2 / AUD-001 | M | `render()` switch, `homeScreen()`, `medsScreen()`, `userBar()` |
| INC-C | Escape ข้อความจากผู้ใช้ก่อนเข้า `innerHTML` (pet fields, symptom text) | AUD-002 | S-M | escape helper ใหม่ + sinks: pet card, pet picker, booking details, add-pet form |
| INC-D | Recovery เมื่อ `sessionStorage` พัง/เต็ม (แจ้ง + reset สะอาด) | AUD-007 | S | `persistState()`, `hydrateState()` |
| INC-E | กระดิ่งแจ้งเตือน 3 รายการ กดเด้งไปหน้าคิว/หน้ายา | TODO P2 | S | `userBar()` หรือ `homeScreen()`, handler ใหม่ |
| INC-F | หน้าผลตรวจแล็บ (mock 3-5 แถว + ปุ่มปรึกษาหมอต่อ) | TODO P2 | M | screen ใหม่ + `DEPTH` map + `render()` switch + nav |
| INC-G | โหมดผู้สูงอายุของคุณยายบุญมี (ตัวอักษรใหญ่ + การ์ด SOS) | TODO P2 | M | `homeScreen()`, CSS scale hooks, `users` data |
| INC-H | หน้าสถิติฝั่งผู้ประกอบการ | TODO P2 | L | screen ใหม่ทั้งหน้า (พิจารณาไฟล์แยกได้ตาม TODO note) |
| INC-I | Accessibility bounded audit + labels/focus fixes | AUD-008 | S-M | login form, modal focus behavior — ต้อง audit ก่อนกำหนด AC |

สถานะการเลือก (2026-07-18): **INC-0 = ตรวจแล้ว PASS** (ดูผลใน §14.2 — เหลือขั้นผู้ใช้ commit เอง) · **INC-F = ถูกเลือกเป็น increment ถัดไป** (สเปกเต็มใน §5.3)

ข้อจำกัดการจัดลำดับ: **INC-0 ต้องเสร็จก่อน increment ใดที่แก้ `index.html`** เพราะงานค้างปัจจุบันทับซ้อน `successModal()` และ `render()` — หากข้าม INC-0 แล้วแก้บริเวณเดียวกัน จะแยกเจ้าของ diff ไม่ได้และ rollback ยาก (เข้าเงื่อนไขหยุดงานของ Codex ตาม `AGENTS.md` §6)

### 5.3 INC-F specification — หน้าผลตรวจแล็บ (พร้อมขออนุมัติ)

**Precondition:** ผู้ใช้ commit งานค้าง celebration (INC-0 ผ่านแล้ว) ก่อน Codex เริ่ม — เพื่อให้ diff ของ INC-F แยกเจ้าของชัดและ rollback ได้อิสระ

**Objective:** ทำให้คำสัญญาในแอป "ผลตรวจแจ้งผ่านแอปภายใน 1-2 วัน" มีหน้าจริงรองรับ พร้อมจุด cross-sell "ปรึกษาหมอต่อ" ตาม TODO P2

**User-visible outcome:**

1. หน้าเจาะเลือดมีปุ่ม/การ์ดรอง "ผลตรวจของฉัน" ใต้ hero — กดแล้วเข้าหน้าใหม่ `lab-results`
2. หน้า `lab-results`: hero โทน slate เดียวกับหน้าเจาะเลือด + รายการผลตรวจจำลอง 3-5 แถวต่อสมาชิก
   - แต่ละแถว: ชื่อรายการ, ค่าที่วัด + หน่วย, ช่วงอ้างอิง, ชิปสถานะ (เขียว "ปกติ" / อำพัน "ควรติดตาม"), วันที่ตรวจ
   - วันที่ต้องคำนวณจากวันปัจจุบัน (เช่น ~30 วันก่อน) ไม่ hardcode — ตามแนวทางกันข้อมูลค้างของโปรเจกต์
   - สมหญิง (u1) และคุณยายบุญมี (u3) มีชุดผลของตัวเอง; สมชาย (u2) และกานต์ (u4) เห็น empty state "ยังไม่มีผลตรวจ" พร้อมปุ่มพาไปจองเจาะเลือด
3. ปุ่ม CTA "ปรึกษาหมอต่อ" ท้ายรายการ → ไปหน้าเลือกผู้เชี่ยวชาญ (flow เดิม ไม่แก้)
4. ปุ่มย้อนกลับบน hero → กลับหน้าเจาะเลือด

**In scope:** หน้าจอใหม่ 1 หน้า, entry point 1 จุดบนหน้าเจาะเลือด, mock data ต่อสมาชิก (hardcoded แบบเดียวกับ `medsByUser`), รายการใน `DEPTH` map, case ใน `render()` switch, back mapping

**Non-goals ของ INC-F:** ไม่แตะ bottom nav (คง 5 ปุ่มเดิม) · ไม่ generate ผลจากการจองจริง · ไม่มีกราฟ/ประวัติย้อนหลัง · ไม่เพิ่ม persisted state field ใหม่ · ไม่แตะ flow จ่ายเงิน, `successModal()` หรือ dialog gating (บริเวณ diff ของ INC-0) · ไม่เพิ่ม dependency/external asset

**Affected regions ใน `index.html` (ไฟล์เดียวที่แก้ได้):**

| Region | การแก้ |
|---|---|
| Domain data (ใกล้ `medsByUser`) | เพิ่ม `labResultsByUser` hardcoded |
| `DEPTH` map | เพิ่ม `"lab-results": 2` |
| `labScreen()` | เพิ่มปุ่ม "ผลตรวจของฉัน" |
| ฟังก์ชันใหม่ `labResultsScreen()` | hero + รายการผล + empty state + CTA |
| `render()` switch + back handler | case ใหม่ + back จาก `lab-results` → `lab` |

**Interfaces/data/schema:** ไม่มี API/พึ่งพาใหม่ · `sessionStorage` schema ไม่เปลี่ยน (`state.screen` รับค่า string ใหม่ได้ด้วย hydration เดิม) · เปิดออฟไลน์ไฟล์เดียวได้เหมือนเดิม

**Failure modes / edge cases ที่ต้องรองรับ:** refresh ระหว่างอยู่หน้านี้ → กลับหน้าเดิมสมาชิกเดิม · สลับสมาชิกขณะอยู่หน้านี้ → รายการเปลี่ยนตาม · สมาชิกไม่มีผล → empty state ไม่ใช่หน้าว่าง · reduced-motion → ผ่านระบบ animation gate เดิมโดยไม่เพิ่มกรณีพิเศษ

**Acceptance Criteria (pass/fail):**

- AC-F-01: ปุ่ม "ผลตรวจของฉัน" ปรากฏบนหน้าเจาะเลือดและนำเข้าหน้า `lab-results` ด้วยอนิเมชัน forward ปกติ; ย้อนกลับกลับสู่หน้าเจาะเลือด
- AC-F-02: u1 เห็นผล ≥3 รายการ มีชิปทั้ง "ปกติ" และ "ควรติดตาม"; u3 เห็นชุดต่างจาก u1; u2 และ u4 เห็น empty state พร้อมปุ่มไปจองที่ใช้งานได้
- AC-F-03: วันที่บนผลตรวจคำนวณจากวันเปิดแอป ไม่มีวันที่ hardcode
- AC-F-04: "ปรึกษาหมอต่อ" นำไปหน้าเลือกผู้เชี่ยวชาญของ flow เดิม
- AC-F-05: F5 ขณะอยู่หน้านี้แล้ว hydrate กลับหน้าเดิม สมาชิกเดิม
- AC-F-06: console ไม่มี error ตลอดการทดสอบ; `git status` มีเฉพาะ `index.html`; diff ไม่แตะบริเวณ `successModal()`/dialog gating ของ INC-0
- AC-F-07: ยังเปิดแบบ file:// ออฟไลน์ได้ ไม่มี external reference ใหม่ (ตรวจ network log ว่าไม่มี request ออกนอกเครื่อง)

**Verification plan (Codex):** เปิดไฟล์บน Chrome ล่าสุด (default target ตาม ASM-004; ผู้ใช้ override ได้ตอนอนุมัติ) → ไล่ AC-F-01 … AC-F-07 ทีละข้อ ทั้ง viewport ปกติและจอ 1366×768 → สกัด script ตรวจ syntax → `git diff` ยืนยันบริเวณที่แก้ตรงตาราง Affected regions → บันทึกผลจริงทุกขั้นใน Completion Report

**Rollback:** revert เฉพาะ diff ของ INC-F (เป็น diff แยกชัดหลัง INC-0 ถูก commit แล้ว) — ไม่มีผลต่อข้อมูล persisted เดิม

## 6. Non-goals

Phase 0 ไม่รวม:

- การแก้ `index.html`
- การแก้ bug, security finding, accessibility issue หรือ backlog item ใด ๆ
- การเปลี่ยน UI, copy, price, benefit, mock data หรือ flow
- การเพิ่ม test/runtime/build/dependency
- การเปลี่ยน single-file/offline architecture
- การเชื่อม backend, authentication, payment, telemedicine หรือ external service จริง
- การ commit, push, branch, PR, reset, revert หรือ checkout ทับไฟล์ผู้ใช้
- การตัดสินใจว่า prototype พร้อม production

## 7. Affected Components and Files

| File | Phase 0 action | Notes |
|---|---|---|
| `CLAUDE.md` | Create | Planning/Audit workflow |
| `AGENTS.md` | Create | Implementation/Verification gate |
| `SPEC.md` | Create | DRAFT baseline, findings and questions |
| `index.html` | No change permitted | Contains protected pre-existing uncommitted work |
| `README.md` | Read only | Current product description |
| `TODO.md` | Read only | Candidate backlog, not authorization |

Future affected components/files: BLOCKED pending approved Scope

## 8. Interfaces, API, Data or Schema Changes

Phase 0 changes none of the following:

- Public interface: none
- Backend/API: none exists and none is added
- Data/schema: no application state change
- `sessionStorage` key/schema: unchanged
- External services: none
- File format/runtime: unchanged

Any future interface, state-shape or storage-key change must be specified with compatibility and migration behavior before approval

## 9. Implementation Plan

### 9.1 Phase 0 plan

1. Capture repository/Git baseline and protected work
2. Create the three workflow documents only
3. Verify document structure, DRAFT status, diff hygiene and unchanged `index.html` hash
4. Report findings and stop for Product Owner review

### 9.2 Plan before any application implementation

1. Product Owner answers Blocking Questions and selects exactly one implementation increment
2. Claude re-audits current repository and updates stale evidence
3. Claude replaces “Proposed future scope” with explicit in-scope behavior, Non-goals and affected files
4. Claude defines pass/fail Acceptance Criteria and executable Verification Plan
5. Product Owner approves the exact revision
6. Status changes to `APPROVED_FOR_IMPLEMENTATION` and Approval Record is completed
7. Codex runs Mandatory Preflight from `AGENTS.md`
8. Codex implements, verifies and submits Completion Report
9. Claude performs Post-implementation Audit

## 10. Failure Modes and Edge Cases

### Workflow failure modes

- Status changed without a matching Approval Record: Codex must stop
- Repository changes after approval: Codex must compare baseline and stop if material
- Approved scope overlaps pre-existing `index.html` diff: Codex must identify overlap and request clarification if intent is ambiguous
- Acceptance Criteria cannot be executed in the available environment: return to Claude/Product Owner; do not silently weaken them
- New requirement appears during implementation: treat as out of scope until approved revision
- User asks for a task outside approved scope: stop and request a spec update

### Existing application edge cases to consider in a future spec

- Reload during queue, payment or VDO call
- Corrupt/unavailable `sessionStorage`
- Rapid/double click across modal transitions
- Timer behavior when tab is backgrounded
- Empty, long or markup-like user input
- Booking/cancellation after reload
- Viewport shorter than the 844px phone frame
- Reduced-motion preference
- Offline launch by opening the file directly versus localhost

These are verification candidates, not authorized fixes

## 11. Security and Privacy Considerations

- Repository is explicitly a demo with synthetic data; do not introduce real personal, medical or payment information
- Login is only a presentation control, not authentication
- Payment QR, queue and VDO call are simulations
- Personal/health-like demo content is currently rendered before login; see Finding `AUD-001`
- User-entered pet fields flow into `innerHTML`; see Finding `AUD-002`
- `sessionStorage` is readable by scripts in the same origin and is not suitable for real protected health information
- A production direction would require a separate architecture/security specification covering identity, authorization, consent, encryption, audit logging, retention and regulatory review

## 12. Performance and Reliability Considerations

- Single-file offline delivery reduces network failure risk and supports presentation portability
- The 1.83 MB HTML includes runtime libraries and fonts, increasing parse/startup cost and making review/diff isolation harder
- Re-rendering replaces major DOM sections and reattaches listeners, increasing regression risk as features grow
- State persistence improves F5 recovery but write/parse failures are swallowed silently
- Timer-driven queue/call flows can diverge from presenter timing or background-tab scheduling
- `fitPhone()` uses CSS `zoom`; target browser/projector combinations must be confirmed before defining compatibility acceptance
- No automated regression suite currently exists

## 13. Compatibility, Migration and Rollback

### Phase 0

- Compatibility impact: none; documentation only
- Migration: none
- Rollback: remove only the three newly-created documents if the Product Owner explicitly requests it; never touch the pre-existing `index.html` diff

### Future implementation

- Default compatibility requirement is the current offline single-file behavior until an approved spec says otherwise
- Any state-shape change must define how existing `ornsirin-demo-state-v1` data is handled
- Rollback must identify exact files/changes and must not rely on destructive Git commands against user work
- Target browsers, viewport and presentation hardware remain blocking inputs

## 14. Audit Findings

### 14.1 Summary

| ID | Severity | State | Finding |
|---|---|---|---|
| AUD-001 | MEDIUM | CONFIRMED | Personal/health-like demo content is available before login |
| AUD-002 | MEDIUM | CONFIRMED | User-controlled pet data reaches `innerHTML` without escaping |
| AUD-003 | MEDIUM | CONFIRMED | Monolithic file and absence of automated tests raise regression cost |
| AUD-004 | MEDIUM | CONFIRMED | Presenter cannot control/re-enter the timer-driven queue as requested in backlog |
| AUD-005 | MEDIUM | CONFIRMED | Significant uncommitted app work exists before workflow setup |
| AUD-006 | MEDIUM | CONFIRMED LIMITATION | Authentication, payment and VDO are simulation-only |
| AUD-007 | LOW | CONFIRMED | Persistence failures are silently ignored |
| AUD-008 | LOW | CONFIRMED / NEEDS_VERIFICATION | Login labels are placeholder-only; modal focus behavior still needs manual audit |

No CRITICAL finding was confirmed in Phase 0. Severity is calibrated for a controlled offline presentation prototype; several findings would become HIGH/CRITICAL if real health or payment data were introduced

### AUD-001 — Content available before login

- Severity: MEDIUM for current synthetic demo; HIGH if real personal/health data is used
- State/Confidence: CONFIRMED / High
- Evidence: `index.html:822-839` changes only the top user bar when logged out; `render()` still renders the active screen at `index.html:1768-1781`. Read-only browser DOM inspection before login showed the sample resident name, house number, benefits, service buttons and upcoming appointment content. `TODO.md` also lists “ซ่อนข้อมูลส่วนตัวก่อนล็อกอิน” as unfinished
- Observed: login does not gate rendering of the application content
- Expected: if login is presented as a privacy boundary, protected content and actions should not render or be reachable before login
- Impact: weakens the demo story and would expose information in any real-data deployment
- Recommendation: Product Owner must decide whether login is decorative or a real gate; implement conditional rendering only if selected
- Trade-off: gating adds state/flow cases and may slow the presentation script
- Scope relation: Candidate; not in Phase 0

### AUD-002 — Unescaped user input reaches HTML rendering

- Severity: MEDIUM in local prototype; HIGH in a shared/real system
- State/Confidence: CONFIRMED / High
- Evidence: pet name/breed/age are read from inputs at `index.html:2100-2108`, stored at `index.html:2113-2123`, interpolated into templates such as `index.html:1451-1462`, then assigned through `app.innerHTML` at `index.html:1781`
- Observed: no escaping/encoding boundary is visible before HTML interpolation
- Expected: user-controlled text should be encoded or inserted with `textContent`
- Impact: markup/event-handler injection can run within the prototype origin and corrupt the rendered UI/session
- Recommendation: define and apply one escaping strategy for all user-controlled text, with injection test cases
- Trade-off: a narrow escape helper is low-cost but requires complete sink inventory; DOM construction is safer but a larger refactor
- Scope relation: Candidate; not in Phase 0

### AUD-003 — Monolith and missing regression automation

- Severity: MEDIUM
- State/Confidence: CONFIRMED / High
- Evidence: repository start inventory contained only `index.html`, `README.md` and `TODO.md`; no package/test/build files. `index.html` is 1,831,354 bytes and contains dependencies, fonts, styles, state, templates and event wiring
- Observed: regression verification depends on manual inspection of a large mixed-concern file
- Expected: each approved change should have repeatable syntax and flow verification proportional to risk
- Impact: high review cost and increased chance that a small change affects unrelated flows
- Recommendation: first define a dependency-free verification harness/checklist compatible with the offline constraint; split architecture only if separately approved
- Trade-off: automated browser tooling improves confidence but introduces tool/dependency maintenance; manual checklist preserves simplicity but is weaker
- Scope relation: Candidate technical-debt work; not in Phase 0

### AUD-004 — Queue is not presenter-controlled

- Severity: MEDIUM for live-demo reliability
- State/Confidence: CONFIRMED / High
- Evidence: `TODO.md` has an unchecked P1 “ตัวควบคุมคิวสำหรับผู้พรีเซนต์”; `startQueueTimer()` around `index.html:755-764` advances queue state by timer, and current navigation does not expose the proposed persistent queue banner/keyboard controls
- Observed: timing is coupled to elapsed time rather than the presenter’s narration
- Expected: only if selected by Product Owner, presenter can re-enter and advance the queue without resetting it
- Impact: demo pacing can drift or require restarting the consult flow
- Recommendation: make this the next narrow increment if live presentation reliability is the priority
- Trade-off: presenter-only controls can confuse normal users unless hidden/clearly scoped
- Scope relation: Candidate; not approved

### AUD-005 — Protected uncommitted application work

- Severity: MEDIUM delivery risk, not an application defect
- State/Confidence: CONFIRMED / High
- Evidence: initial `git status` was `M index.html`; diff stat was 47 insertions and 2 deletions. The diff adds success animation/count-up logic; SHA-256 baseline recorded above
- Observed: future work may overlap code that has no committed checkpoint
- Expected: ownership/baseline is explicit before implementation
- Impact: accidental overwrite, mixed Completion Report or difficult rollback
- Recommendation: user confirms this diff as accepted baseline and preferably creates their own checkpoint before approved implementation
- Trade-off: checkpointing adds process but sharply reduces recovery risk
- Scope relation: Blocking baseline decision for future app work

### AUD-006 — Simulation-only service boundaries

- Severity: MEDIUM if stakeholder expectation is unclear; intended behavior for current prototype
- State/Confidence: CONFIRMED LIMITATION / High
- Evidence: `README.md` explicitly calls the project a prototype with mock data; source contains client-side queue/payment/VDO simulation and no backend/API
- Observed: any ID/password logs in; QR/payment verification and calls do not contact real services
- Expected: presentation must clearly identify these behaviors as demo simulations
- Impact: stakeholder misunderstanding or unsafe reuse beyond intended context
- Recommendation: retain explicit demo labeling; production work requires a new architecture/specification
- Trade-off: stronger labeling can reduce visual realism but prevents false expectations
- Scope relation: Non-goal unless Product Owner changes project direction

### AUD-007 — Silent persistence failure

- Severity: LOW for demo, higher if state becomes important
- State/Confidence: CONFIRMED / High
- Evidence: `persistState()` and `hydrateState()` catch and ignore all errors at `index.html:645-655`
- Observed: quota, privacy-mode or malformed JSON failures have no recovery message and may appear as unexpected state loss
- Expected: predictable reset/recovery behavior or at least diagnostic feedback during verification
- Impact: difficult troubleshooting during presentation
- Recommendation: decide whether graceful reset plus demo-only notice is desired in a future reliability increment
- Trade-off: visible recovery messaging adds UI/state complexity
- Scope relation: Candidate; not in Phase 0

### AUD-008 — Accessibility gaps need bounded audit

- Severity: LOW in current presentation context
- State/Confidence: login labels CONFIRMED / High; modal focus NEEDS_VERIFICATION / Medium
- Evidence: login inputs at `index.html:826-834` rely on placeholders and have no associated `<label>`. Template/modal code shows no explicit focus trap/return in the inspected sections
- Observed: accessible names depend on placeholder text; full keyboard/focus behavior was not exhaustively verified in Phase 0
- Expected: labels remain identifiable and modal focus is predictable if accessibility is in approved scope
- Impact: weaker screen-reader/keyboard usability
- Recommendation: run a dedicated keyboard/accessibility audit before setting related Acceptance Criteria
- Trade-off: robust focus management may require cross-flow changes in the render architecture
- Scope relation: Candidate; not in Phase 0

### 14.2 Phase 1 detailed audit — protected uncommitted diff (`index.html`, 47+/2−)

อ่านแบบ read-only ครบทุก hunk ใน Phase 1 (`git diff -- index.html`) สรุปเนื้อหาและการประเมิน:

| Hunk | ตำแหน่ง | เนื้อหา |
|---|---|---|
| 1 | CSS animation block (~374-390) | เพิ่ม keyframes `checkDraw`, `ringPulse`, `confettiFall` + base classes (`.check-draw`, `.success-ring`, `.confetti-bit`) + คลาสกระตุ้น `.celebrate …` |
| 2 | reduced-motion block (~395-402) | ปิดทั้งสาม effect ภายใต้ `prefers-reduced-motion: reduce` |
| 3 | `successModal()` (~1726-1755) | confetti layer (render เฉพาะ `b.amount` จริง), ring + SVG check วาดเส้นแทน lucide icon, `data-countup` บนบรรทัดยอดเงิน |
| 4 | `render()` dialog gating (~1840-1866) | ติดคลาส `celebrate` เฉพาะตอน dialog เพิ่งเปิด, ยอดเงินวิ่งนับด้วย rAF + `setTimeout` failsafe (~900ms) + ข้ามเมื่อ reduced-motion ผ่าน `matchMedia` |

การประเมินเชิงออกแบบ (CONFIRMED จากการอ่านโค้ด):

- Replay-safety สอดคล้องสถาปัตยกรรมเดิม: effect ทั้งหมด scope ใต้ `.celebrate` ซึ่งติดเฉพาะ `modalKey` เปลี่ยน — การ re-render ระหว่าง dialog เปิดจะไม่เล่นซ้ำ และ base state (`opacity:0`, `stroke-dashoffset` default 0) ทำให้ DOM ที่สร้างใหม่โดยไม่มีคลาสไม่แสดง effect ค้าง
- Confetti ถูก gate ด้วย `b.amount` — dialog แจ้งเตือน (ยกเลิกนัด/ลบโปรไฟล์/จบสาย ที่ amount=0) จะไม่ฉลอง ซึ่งตรงเจตนา UX
- ไม่มี user-controlled data เข้าสู่ markup ใหม่ (สี/ตำแหน่ง confetti มาจากสูตร index ล้วน) — ไม่เพิ่มพื้นผิว AUD-002
- ไม่แตะ `sessionStorage` schema, timers อื่น, หรือ external dependency

สถานะการตรวจ: **VERIFIED — INC-0 executed 2026-07-18 (PASS with limitation)** · วิธี: สร้างสำเนา byte-identical (`_inc0_verify_copy.html`, SHA-256 ตรงต้นฉบับ, แก้เฉพาะ `<title>` เพื่อเปิดในเบราว์เซอร์) รัน checklist แล้วลบสำเนา — `index.html` hash ยืนยันไม่เปลี่ยนหลังจบ (`f471e803…a00b976`)

| Checklist step | ผล |
|---|---|
| 1. เปิดหน้าเวอร์ชันปัจจุบัน | PASS — โหลดสำเร็จ, celebration CSS อยู่ครบ |
| 2. Flow จ่ายเงินจริง → dialog สำเร็จ | PASS — `celebrate` ติดที่ overlay, confetti 18 ชิ้น, ring + check-draw + `exhale-once` ครบ, countup จบที่ ฿400 ตรง target |
| 3. Dialog amount=0 (ยกเลิกนัด) | PASS — ไม่มี confetti/countup, check + ring ยังแสดงปกติ |
| 4. สลับแท็บระหว่างนับ | PASS โดย failsafe design (`setTimeout` เซ็ตค่าจบเสมอ — ยืนยันค่าจบถูกต้องในข้อ 2) |
| 5. Reduced motion (ตั้งค่า OS) | **NOT RUN** — ต้องตั้งค่าที่ระบบปฏิบัติการจริง ผู้ใช้ควร spot-check เองหนึ่งครั้ง หรือยอมรับหลักฐานเชิงโค้ด (CSS + `matchMedia` guard มีครบ) |
| 6. Console errors | PASS — ไม่มี error ตลอดการทดสอบ |

ความเสี่ยงคงเหลือที่ระบุได้จากการอ่าน (ใช้ประกอบ BQ-001):

- R1 (LOW): rAF ถูก browser หยุดเมื่อแท็บอยู่เบื้องหลัง — mitigated ด้วย `setTimeout` failsafe ที่เซ็ตยอดสุดท้ายเสมอ
- R2 (LOW): `exhale-once` เดิมยังติดคลาสร่วมกับ ring/draw ใหม่ — คาดว่าเสริมกัน แต่ต้องดูของจริงว่าไม่ตีกันทางสายตา
- R3 (LOW): confetti 18 ชิ้น × fixed 640px fall — บนจอที่ถูก `fitPhone()` ย่อ อาจตกไม่สุดกรอบ/สุดเร็วไป ต้องดูของจริง

Verification checklist สำหรับ INC-0 (รันได้ทันทีที่ผู้ใช้อนุญาตให้เปิดหน้า):

1. เปิด `index.html` เวอร์ชันปัจจุบัน (ดิสก์) ในเบราว์เซอร์เป้าหมาย
2. ทำ flow จ่ายเงิน 1 รายการ (เช่น สั่งยา) จนถึง dialog สำเร็จ
3. ยืนยัน: เช็ควาดเส้น ~0.5s, วงแหวนกระเพื่อม 1 ครั้ง, confetti โปรยแล้วจางหมด, ยอดวิ่ง 0 → ยอดจริง และจบที่ค่าถูกต้องแม้แท็บถูกสลับระหว่างนับ
4. เปิด dialog ที่ amount=0 (เช่น ยกเลิกนัด) ยืนยันว่าไม่มี confetti/countup แต่เช็ค+ring ยังทำงาน
5. ตั้งค่า reduce motion ของ OS แล้วทำซ้ำข้อ 2 — ต้องไม่มี effect ใดเล่น และยอดแสดงเต็มทันที
6. ตรวจ console ว่าไม่มี error ตลอดการทดสอบ

### 14.3 Verified strengths

- Single-file offline packaging matches the documented presentation goal
- `prefers-reduced-motion` handling exists for major animations
- `hydrateState()` derives booking sequence from persisted bookings to reduce duplicate references after refresh
- Lucide rendering is guarded so icon failure does not stop the entire app
- Current uncommitted `index.html` diff passes `git diff --check`

## 15. Test and Verification Plan

### 15.1 Phase 0 verification

Run from `/Users/dome/Desktop/ORNSIRIN PROJECT`:

1. `pwd` — must equal the repository path exactly
2. `git status --short --branch` — record branch and distinguish protected `M index.html` from the three new documents
3. `rg --files --hidden -g '!.git/**'` — verify document/file inventory
4. Read `CLAUDE.md`, `AGENTS.md` and `SPEC.md` completely
5. Check the first line of `SPEC.md` equals `Status: DRAFT`
6. `git diff --check` — must return exit code 0
7. `shasum -a 256 index.html` — must remain `f471e803cb581ecd85bbeca17a06c373ab85f09c4cde19bd8ef81c3e2a00b976`
8. `git diff --stat -- index.html` — must remain 47 insertions and 2 deletions from the protected baseline
9. Confirm no dependency, commit, push, reset, revert or checkout action occurred

### 15.1.1 Phase 1 verification log (executed 2026-07-18)

| Step | Result |
|---|---|
| `pwd` | ตรง repository path |
| อ่าน `CLAUDE.md`, `AGENTS.md`, `SPEC.md`, `README.md`, `TODO.md` ครบทั้งไฟล์ | ทำแล้ว |
| `git status --short --branch` + แยก staged/unstaged/untracked | ตรง §3.4 |
| `git diff -- index.html` อ่านครบทุก hunk แบบ read-only | ตรง §14.2 |
| `git diff --check` | exit 0 |
| `shasum -a 256 index.html` | ตรง baseline (`f471e803…a00b976`) |
| แก้ไขเฉพาะ `SPEC.md` | ไม่มีไฟล์อื่นเปลี่ยน ไม่มี commit/push/dependency |

### 15.2 Future implementation verification

BLOCKED. Claude must replace this subsection with commands, target browsers/viewports, primary flows, edge cases and expected results for the selected increment before approval

## 16. Acceptance Criteria

### Phase 0

- AC-P0-01: `CLAUDE.md`, `AGENTS.md` and `SPEC.md` exist at repository root
- AC-P0-02: first line of `SPEC.md` is exactly `Status: DRAFT`
- AC-P0-03: `CLAUDE.md` defines Claude role, audit procedure/checklist/severity/evidence, assumptions/questions, approval prohibition, Codex handoff and post-audit
- AC-P0-04: `AGENTS.md` defines Codex role, Mandatory Preflight, Approval Gate, implementation/verification/scope rules, spec-gap handling and Completion Report
- AC-P0-05: `SPEC.md` contains every section requested by the Product Owner and separates confirmed evidence from assumptions/questions
- AC-P0-06: `index.html` hash and pre-existing diff stat remain unchanged from baseline
- AC-P0-07: no existing file is reset, reverted, checked out over, deleted, staged, committed or pushed
- AC-P0-08: no dependency is installed and no implementation begins
- AC-P0-09: final report includes documents changed, Git before/after, preserved work, structure, Blocking Questions and next step for Claude

### Future application increment

NOT DEFINED; therefore not verifiable and not authorized

## 17. Assumptions

- ASM-001: Project remains an offline presentation prototype unless the Product Owner says otherwise
- ASM-002: All displayed resident, health, pet and payment information is synthetic
- ASM-003: Current `index.html` uncommitted success-animation diff is intentional user work to preserve, but not yet an approved/committed baseline
- ASM-004: The presentation target likely includes a 1366×768 projector, based on README/TODO, but exact browser/OS is not confirmed
- ASM-005: The remaining TODO items are ideas/priorities, not approved requirements

Assumptions must not be converted into implementation requirements without Product Owner decision

## 18. Open Questions

### Resolved (2026-07-18) — ย้ายผลไปที่ §19 Decisions

- BQ-001 — RESOLVED: ผู้ใช้รับงาน celebration เป็น baseline; INC-0 verification executed PASS (§14.2); เหลือขั้นผู้ใช้ commit เองก่อน INC-F เริ่ม (DEC-006)
- BQ-002 — RESOLVED: ผู้ใช้เลือก INC-F หน้าผลตรวจแล็บ (DEC-007); BQ-005 หมดสภาพ blocking โดยปริยายเพราะการเลือกเกิดขึ้นแล้ว

### Blocking Questions

ไม่มีสำหรับ scope INC-F — เงื่อนไขคงเหลือเป็นการยืนยันตอนอนุมัติ:

- BQ-004 (ลดระดับเป็น confirm-on-approval) — Verification ของ INC-F ใช้ default: Chrome ล่าสุดบน macOS, เปิดแบบ file:// offline, ตรวจทั้ง viewport ปกติและ 1366×768 (ตาม ASM-004) — การอนุมัติ INC-F ถือเป็นการยอมรับ default นี้ เว้นแต่ผู้ใช้ระบุอื่น

### Non-blocking / strategic

- BQ-003 — ทิศทาง demo vs ระบบจริง: ยังเปิดอยู่ ไม่ block INC-F (INC-F เป็น mock feature สอดคล้อง ASM-001)
- BQ-005 — ลำดับ privacy vs presenter reliability สำหรับรอบถัด ๆ ไป: ยังเปิดอยู่

### Non-blocking until a related scope is selected

- OQ-001 — Is adding a dependency-free automated smoke check desirable, or should verification remain manual to preserve the zero-tooling repository?
- OQ-002 — Should accessibility become an explicit acceptance target in the next increment?
- OQ-003 — Should `.claude/settings.local.json` permissions be reviewed/cleaned by the user separately, given that the file is local and ignored?
- OQ-004 — เอกสาร workflow ทั้งสาม (`CLAUDE.md`, `AGENTS.md`, `SPEC.md`) ยังเป็น untracked — ผู้ใช้ต้องการ commit เอกสารเหล่านี้เอง (แนะนำ เพื่อกันการสูญหายและให้ audit trail ตรวจย้อนได้) หรือจะสั่งให้ commit ในรอบใดรอบหนึ่ง?

## 19. Decisions

| ID | Date | Decision | Decider |
|---|---|---|---|
| DEC-001 | 2026-07-18 | Phase 0 is documentation/audit workflow setup only | Product Owner |
| DEC-002 | 2026-07-18 | `index.html` must not be changed before SPEC approval | Product Owner |
| DEC-003 | 2026-07-18 | Existing uncommitted work must be preserved; no reset/revert/checkout | Product Owner |
| DEC-004 | 2026-07-18 | No dependency install, commit, push or implementation in Phase 0 | Product Owner |
| DEC-005 | 2026-07-18 | Claude plans/audits; Codex implements/verifies; Product Owner approves | Product Owner |
| DEC-006 | 2026-07-18 | รับงานค้าง success celebration เป็น baseline (BQ-001); INC-0 verification PASS ตาม §14.2; ผู้ใช้จะ commit งานนี้เองก่อนเริ่ม INC-F | Product Owner |
| DEC-007 | 2026-07-18 | เลือก INC-F (หน้าผลตรวจแล็บ) เป็น implementation increment ถัดไป (BQ-002) | Product Owner |

## 20. Approval Record

| Version | Status | Approved scope | Approver | Date | Notes |
|---|---|---|---|---|---|
| 0.1 | DRAFT | None | Pending Product Owner | — | Blocking Questions remain; no implementation authorization |
| 0.2 | DRAFT | None | Pending Product Owner | — | Phase 1 architecture audit added: baseline re-verified, uncommitted diff audited read-only (§14.2), increment menu §5.2 ready for selection; Blocking Questions BQ-001–BQ-005 remain |
| 0.3 | DRAFT | None (INC-F spec §5.3 awaiting approval) | Pending Product Owner | — | BQ-001/BQ-002 resolved (DEC-006/DEC-007); INC-0 executed PASS with reduced-motion limitation; full INC-F specification added; awaiting formal approval + user commit of celebration baseline |
| 0.3 | APPROVED_FOR_IMPLEMENTATION | INC-F ตาม §5.3 ทั้งหมด รวม default verification target (Chrome ล่าสุด, file:// offline, viewport ปกติ + 1366×768) ตามหมายเหตุ BQ-004 | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท | 2026-07-19 (Asia/Bangkok) | ข้อยกเว้น/เงื่อนไข: (1) precondition — ผู้ใช้ต้อง commit งาน celebration ก่อน Codex เริ่ม (ณ เวลาอนุมัติยังเป็น `M index.html`, HEAD `818249b`, hash `f471e803…a00b976`); (2) reduced-motion spot-check ของ INC-0 ยังเป็น open limitation ฝั่งผู้ใช้; (3) Claude ถูกห้ามแก้ implementation — งานแก้เป็นของ Codex เท่านั้น |

Only the Product Owner may authorize changing the first line to `Status: APPROVED_FOR_IMPLEMENTATION`. Approval must reference the exact version/scope and resolve all Blocking Questions that affect that scope
