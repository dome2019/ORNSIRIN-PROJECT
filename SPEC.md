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
| Baseline HEAD | `6f1e5d3` — INC-O committed + pushed โดย Product Owner · SHA-1 `6d225e06` · ก่อนหน้า: `11edac9` (approve v0.11), `3a41dfc` |
| Baseline audit date | 2026-07-18 (Asia/Bangkok) |
| Phase 1 architecture audit date | 2026-07-18 (Asia/Bangkok, late evening) |
| Specification version | 0.12 |
| Blocking Questions | NONE |
| Implementation authorization | **GRANTED — INC-P สมุดสุขภาพ + กราฟค่าสุขภาพ (§5.27, Option A MVP) อนุมัติ v0.12 เมื่อ 2026-07-24** · รอบก่อนหน้า: INC-O (`6f1e5d3`), INC-N/INC-L+M (`3a41dfc`) — COMPLETED |

> Approval Gate: **อนุมัติแล้ว (v0.12, 2026-07-24)** — ขอบเขต: **INC-P ตาม §5.27 เท่านั้น** (สมุดสุขภาพ read-only + กราฟค่าสุขภาพ 3 ตัว inline-SVG + ลิงก์ผลแล็บ/ยา/บัตรสุขภาพ + การ์ดเข้าหน้าแรก + empty state) · baseline `6f1e5d3` (SHA-1 `6d225e06`, working tree สะอาด) · Mandatory Preflight ก่อนเริ่ม · **additive เท่านั้น** — ห้ามแตะ protected (timer/payment/tracker/celebration/escaping/privacy gate/INC-J/K/storage) · SVG invariants §5.27 บังคับ (viewBox+width:100% ไม่มี px, สีใน style attr, ไม่ crispEdges, static/reduced-motion-safe, ตัวเลขเป็น HTML) · read-only ไม่มี persisted key ใหม่ · จำลอง 100% (ASM-002) · **ประวัติย้อนหลังลูกค้า = deferred** (ผู้ใช้สั่งยังไม่ทำ, DEC-023) · Completion Report: inventory + cosmetic + hash SHA-1 · Claude ไม่แก้ implementation
>
> **หมายเหตุ hash (PIA-007):** ค่า hash ของ `index.html` ที่บันทึกในเอกสารนี้เป็น **SHA-1** (จากคำสั่ง `shasum` ค่าเริ่มต้น) — เช่น baseline `a948687` = SHA-1 `41323998…`, ก่อนหน้า `ca68ddb` = SHA-1 `62d25830…` (SHA-256 = `0faf0c33…`) · รอบ v0.8 Codex ใช้ SHA-256 จึงรายงานว่า "ไม่ตรง" — เป็นคนละอัลกอริทึม ไม่ใช่ drift

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

### 5.2 Candidate implementation increments (Phase 1 menu)

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
| INC-J | บัตรสุขภาพฉุกเฉิน (กรุ๊ปเลือด/แพ้ยา/โรคประจำตัว/ผู้ติดต่อฉุกเฉิน) — เด้งใน SOS | คำขอผู้ใช้ (แนวสุขภาพ) | S-M | `users`/data ใหม่ + `sosModal()` + healthCard viewer + u3 home |
| INC-K | ผู้ดูแล / แจ้งลูกหลาน (care circle) — ครอบครัวได้รับแจ้งเมื่อ SOS/นัด | คำขอผู้ใช้ (แนวสุขภาพ) | M | caregiver data + การ์ดผู้ดูแลบน u3 home + SOS notify line |

สถานะการเลือก (อัปเดตหลังปิด batch 2026-07-19): **INC-0 = เสร็จ** (commit `2bbb500`) · **INC-F = เสร็จ + ตรวจรับแล้ว** (§5.3.1, commit `22f413c`) · **INC-A + INC-E + INC-B = เสร็จ + ตรวจรับแล้ว** (audit PASS §5.8, commits `2a8bd34`/`418ef45`, DEC-010) · **ยังเปิดให้เลือก:** INC-C (escape user input), INC-D (storage recovery), INC-G (โหมดผู้สูงอายุ), INC-H (สถิติผู้ประกอบการ), INC-I (accessibility) — เมื่อเลือกแล้ว Claude จะเขียนสเปกเต็มใน §5.x ก่อนขออนุมัติ · **อัปเดตหลังปิด v0.6 (2026-07-19):** INC-C + INC-G + PIA-001 + README = เสร็จ + ตรวจรับแล้ว (§5.13, `ca68ddb`, DEC-012/013) · **ยังเปิดให้เลือก:** INC-D (storage recovery), INC-H (สถิติผู้ประกอบการ), INC-I (accessibility) — INC-C/G ปิดครบแล้ว · **อัปเดต 2026-07-19:** ผู้ใช้เลือก **INC-D + INC-J + INC-K** เป็นรอบถัดไป (DEC-014) · **ปิดรอบแล้ว:** INC-D+J+K (`a948687`), INC-L+M (design + SOS ambulance) + INC-N (hardening) = **เสร็จ + deployed** (`3a41dfc`, §5.22/§5.24) · **คำขอใหม่ 2026-07-19:** ผู้ใช้ขอระบบ **real-time tracking + SOP workflow status แบบแกร็บ** → Claude สำรวจโค้ด 4 มุมมอง (grounded design) ผู้ใช้เลือก **Option B** แล้ว**ขยายเพิ่ม** (2026-07-19): + แม่บ้าน/นวด/Pet care + หน้า "สถานะการบริการ" แบบเสร็จ (ตามภาพ) + รูปแบบเงินมัดจำ/จ่ายหน้างาน (DEC-020) → **INC-O** (§5.25) รออนุมัติ v0.11 · **ยังเปิด:** INC-H (สถิติผู้ประกอบการ — ต่อยอดจาก tracker ได้), INC-I (accessibility)

ข้อจำกัดการจัดลำดับ (ปิดแล้ว — INC-0 เสร็จตั้งแต่ `2bbb500` และถูกปฏิบัติครบใน INC-F/batch): เดิมกำหนดว่า **INC-0 ต้องเสร็จก่อน increment ใดที่แก้ `index.html`** เพราะงานค้างปัจจุบันทับซ้อน `successModal()` และ `render()` — หากข้าม INC-0 แล้วแก้บริเวณเดียวกัน จะแยกเจ้าของ diff ไม่ได้และ rollback ยาก (เข้าเงื่อนไขหยุดงานของ Codex ตาม `AGENTS.md` §6)

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

### 5.3.1 INC-F Post-implementation Audit result (2026-07-19)

- Verdict: **PASS** — Acceptance Criteria AC-F-01 … AC-F-07 ผ่านครบจากการตรวจ runtime จริง (วิธีสำเนา byte-identical เช่นเดียวกับ INC-0)
- Diff 88+/1− ตรงตาราง Affected regions ทั้ง 5 จุด ไม่มี scope creep, ไม่แตะบริเวณ celebration, ไม่มี external reference ใหม่, console สะอาด, ไอคอนใหม่ทั้งสาม render จาก lucide ฝังในไฟล์
- Findings ระดับ LOW: **PIA-001** แถบเมนูล่างไม่มีแท็บ active ขณะอยู่หน้า lab-results (ไม่ใช่ deviation — สเปกไม่ได้กำหนด; เป็น candidate polish) · **PIA-002** ไม่พบ Completion Report ของ Codex ใน repo (แนะนำแนบในรอบถัดไป)
- Product Owner ตรวจรับโดยการ commit `22f413c` (2026-07-19 00:27 +0700) และ push แล้ว — INC-F ปิดงาน

### 5.4 INC-A specification — ตัวควบคุมคิวสำหรับผู้พรีเซนต์

**Objective:** ให้ผู้พรีเซนต์ควบคุมจังหวะคิวได้ตามการเล่าเรื่อง และกลับเข้าหน้าคิวได้โดยไม่รีเซ็ต (ปิด AUD-004 / TODO P1 ตัวสุดท้าย)

**User-visible outcome:**
1. เมื่อมีคิวค้าง (`state.queue` ไม่ใช่ null) และล็อกอินแล้ว หน้าแรกแสดงแบนเนอร์ใต้ปุ่มบริการ: ไอคอนหูฟังแพทย์ + "คิวปรึกษาหมอ #N · แตะเพื่อกลับเข้าคิว" (ถ้าถึงคิว: "ถึงคิวแล้ว — แพทย์พร้อม") — แตะแล้วไป `screen:"queue"` โดยตำแหน่งคิว/สถานะชำระเงินคงเดิมทุกประการ
2. คีย์ลัดผู้พรีเซนต์ (ไม่มี UI บอกบนจอ): ขณะอยู่หน้าคิวและไม่มี overlay เปิด — `ArrowRight` ลดคิว 1 ขั้น (ต่ำสุด 0 = ถึงคิว), `ArrowLeft` เพิ่มคิว 1 ขั้น (สูงสุด 3) · แถบ progress, ตัวเลข และข้อความอัปเดตผ่านกลไก render เดิม (`wait` ตั้งเป็น `position * 6`)

**Non-goals:** ไม่หยุด/เปลี่ยนความเร็ว timer อัตโนมัติเดิม · ไม่เพิ่ม persisted field ใหม่ (`queue`/`queuePaid` มีอยู่แล้ว) · ไม่แสดงคีย์ลัดใน UI

**Affected regions:** `homeScreen()` (แบนเนอร์คิว) · global `keydown` listener ใหม่ในส่วน bootstrap (ทำงานเฉพาะ `state.screen === "queue" && !state.modal && !state.bookingSuccess`)

**Edge cases:** refresh ระหว่างมีคิว → แบนเนอร์คงอยู่ (hydrate เดิม) · จบสาย VDO (`queue: null`) → แบนเนอร์หาย · คีย์ลัดไม่ทำงานนอกหน้าคิว/ตอน overlay เปิด · กดถึง 0 แล้วปุ่มเข้าห้อง/ล็อกจ่ายเงินแสดงตามเงื่อนไข `queuePaid` เดิม

**Acceptance Criteria:**
- AC-A-01: มีคิวค้าง → แบนเนอร์แสดงเลขคิวจริงบนหน้าแรก; ไม่มีคิว → ไม่แสดง
- AC-A-02: แตะแบนเนอร์ → เข้าหน้าคิว ตำแหน่ง/`queuePaid` ไม่รีเซ็ต
- AC-A-03: บนหน้าคิว `ArrowRight`/`ArrowLeft` ขยับคิวลง/ขึ้นในช่วง [0,3] พร้อม UI อัปเดต; ที่ 0 แสดงสถานะถึงคิวตามเงื่อนไขจ่ายเงินเดิม
- AC-A-04: คีย์ลัดเงียบสนิทเมื่ออยู่หน้าอื่นหรือมี overlay เปิด
- AC-A-05: refresh ระหว่างมีคิว → แบนเนอร์และสถานะคิวคงเดิม

### 5.5 INC-E specification — กระดิ่งแจ้งเตือน

**Objective:** ทำให้คำสัญญา "เราจะแจ้งเตือนคุณ" มีของจริง และสาธิต re-engagement (TODO P2)

**User-visible outcome:**
1. `userBar()` สถานะล็อกอิน: ไอคอนกระดิ่ง + badge ตัวเลขจำนวนแจ้งเตือน (ซ้ายของปุ่มสมาชิก)
2. แตะกระดิ่ง → sheet "การแจ้งเตือน" (kind ใหม่ `notifications`, ใช้ `.sheet-body` เข้าอนิเมชันชุดเดิม) รายการ:
   - (เงื่อนไข — แสดงบนสุดเมื่อ `state.queue` มีค่า) "ใกล้ถึงคิวปรึกษาหมอแล้ว (#N)" → ไปหน้า `queue` ไม่รีเซ็ต
   - "ยาประจำใกล้หมด — สั่งเติมล่วงหน้าได้เลย" → `meds`
   - "ผลตรวจสุขภาพพร้อมดูแล้ว" → `lab-results`
   - "วัคซีนสัตว์เลี้ยงใกล้ครบกำหนด" → `lifestyle` พร้อม `lifestyleTab:"petcare"`
   แตะรายการ → ปิด sheet + นำทาง · badge = จำนวนรายการที่แสดงจริง (3 หรือ 4)

**Non-goals:** ไม่มีระบบอ่านแล้ว/ลบแจ้งเตือน · ไม่ผูกเนื้อหากับข้อมูลจริงรายคน (ข้อความกลางแบบ demo — ปลายทางมี empty state รองรับทุกสมาชิกอยู่แล้ว) · ไม่มี push จริง

**Affected regions:** `userBar()` (กระดิ่ง+badge) · ฟังก์ชัน `notificationsModal()` ใหม่ + branch ใน render modal section + handlers (เปิด/แตะรายการ)

**Acceptance Criteria:**
- AC-E-01: ล็อกอินเห็นกระดิ่ง+badge; ยังไม่ล็อกอินไม่เห็น (สอดคล้อง INC-B)
- AC-E-02: ไม่มีคิว → 3 รายการ badge 3; มีคิวค้าง → 4 รายการ มีรายการคิวบนสุด badge 4
- AC-E-03: แตะรายการนำทางถูกปลายทางทั้ง 4 แบบ (queue ไม่รีเซ็ตคิว, lifestyle เปิดแท็บ petcare) และ sheet ปิด
- AC-E-04: sheet เปิด/ปิดผ่านระบบ modal gating เดิม ไม่มีอนิเมชันเล่นซ้ำผี (แตะภายในไม่ re-slide)

### 5.6 INC-B specification — ซ่อนข้อมูลส่วนตัวก่อนล็อกอิน

**Objective:** เปลี่ยน login จากฉากประกอบเป็น privacy gate จริง (ปิด AUD-001)

**User-visible outcome:**
1. ยังไม่ล็อกอิน — หน้าแรกแบบ generic: hero ต้อนรับ (โลโก้/ข้อความ "ยินดีต้อนรับสู่ Ornsirin Health Link" + ชวนล็อกอินที่แถบบน) **ไม่มี** ชื่อสมาชิก, ป้ายลูกบ้าน/บ้านเลขที่, สิทธิ์ส่วนลด, นัดหมาย, การ์ดแนะนำเจาะเลือด · ปุ่มบริการ 4 ปุ่มยังแสดง (โชว์ศักยภาพแอป)
2. ยังไม่ล็อกอินแล้วพยายามเข้า screen อื่นใด (จาก nav/ปุ่ม) → แสดงหน้า `loginGateScreen()`: การ์ดไอคอนกุญแจ + "เข้าสู่ระบบก่อนใช้งาน" + คำอธิบายชี้ไปแถบบน — ไม่มีข้อมูลส่วนบุคคลใดหลุดออกมา (ยา, ผลตรวจ, นัด, สัตว์เลี้ยง)
3. ล็อกอินแล้ว — ทุกอย่างเหมือนปัจจุบันครบ (รวมแบนเนอร์คิว INC-A และกระดิ่ง INC-E) · logout จากหน้าใดก็ตาม → กลับ home generic

**Non-goals:** ไม่แก้กลไก login (ไอดี/รหัสอะไรก็ได้ตามเดิม — ยังเป็น demo) · ไม่เพิ่ม session timeout · ไม่เปลี่ยน `sessionStorage` schema (`loggedIn` มีอยู่แล้ว)

**Affected regions:** `render()` — gate ก่อน switch (`!state.loggedIn && screen !== "home"` → `loginGateScreen()`) · `homeScreen()` — เพิ่ม branch generic · ฟังก์ชัน `loginGateScreen()` ใหม่

**Edge cases:** refresh ขณะ logged-out ที่หน้าลึก → เห็น gate ไม่ใช่ข้อมูล · hydrate `loggedIn:true` → ปกติ · bottom nav ใช้ได้ตลอด (นำไป gate ไม่ใช่ dead end)

**Acceptance Criteria:**
- AC-B-01: ก่อนล็อกอิน หน้าแรกไม่มีชื่อ/บ้านเลขที่/สิทธิ์/นัดหมาย/การ์ดแนะนำ และมีข้อความชวนล็อกอิน
- AC-B-02: ก่อนล็อกอิน ทุก screen อื่น (meds, lab, lab-results, lifestyle, consult-specialty) แสดง gate — ไม่มีข้อมูลยา/ผลตรวจ/สัตว์เลี้ยงหลุด
- AC-B-03: ล็อกอินแล้วเนื้อหากลับครบ; logout จากหน้าลึกกลับ home generic
- AC-B-04: refresh คงพฤติกรรมถูกต้องทั้งสองโหมด

### 5.7 Batch execution terms (INC-A + INC-E + INC-B)

- ทำเป็น **หนึ่งรอบ implementation เดียว** โดย Codex (จุดทับซ้อน `homeScreen()`/`userBar()` จัดการภายในรอบ) · diff เดียว ไฟล์เดียว (`index.html`)
- Precondition: baseline = `22f413c` — **ผ่านแล้ว** (working tree สะอาด ณ เวลาเขียน v0.4)
- Verification รวม: ไล่ AC ทั้ง 13 ข้อ (A-01…05, E-01…04, B-01…04) + syntax check + console + `git status` ไฟล์เดียว + offline (ไม่มี external ref ใหม่) + ตรวจว่าไม่แตะบริเวณ celebration/INC-F
- ลำดับแนะนำภายในรอบ: INC-B ก่อน (โครง gate) → INC-A → INC-E (พึ่งเงื่อนไข loggedIn)
- Rollback: revert batch diff เดียว — ไม่กระทบ commit `22f413c` และก่อนหน้า

### 5.8 Batch INC-A+E+B Post-implementation Audit result (2026-07-19)

**ผล: PASS — 13/13 Acceptance Criteria ผ่านจากการตรวจอิสระของ Claude** (diff review ทุก hunk + runtime ทดสอบเองครบทุก AC ไม่พึ่งรายงาน Codex อย่างเดียว) · Product Owner ตรวจรับแล้ว (DEC-010)

| หัวข้อ | ผลตรวจ |
|---|---|
| Diff review | อ่านครบทุก hunk: `1859c36..7403c7a`, 124+/18−, `index.html` ไฟล์เดียว — ทุก hunk อยู่ในบริเวณที่ §5.4–§5.6 กำหนด ไม่มี hidden scope |
| Runtime AC | 13/13 PASS ผ่านสำเนา byte-identical (ลบหลังตรวจ; hash `index.html` ก่อน/หลัง audit = `1c09a93d…` ไม่เปลี่ยน — Claude ไม่ได้แตะ implementation) |
| Console / offline | 0 error · 0 network request เมื่อเปิดจาก file:// (การโหลดสำเร็จยืนยัน syntax ในตัว — เครื่อง audit ไม่มี node ให้รัน `node --check` ซ้ำ) |
| งานเก่า | ไม่มี hunk แตะ `labResultsScreen()` (INC-F), `successModal()`/celebration, payment flow · `git diff --check` exit 0 |
| กลไกใหม่ | queue object `{position, wait}` ถูกแทนที่แบบ lossless · keydown listener ลงทะเบียนครั้งเดียวนอก `render()` ไม่สะสมซ้ำ · ไอคอน Bell/Lock/PawPrint มีจริงในบันเดิล lucide ที่ฝัง · แถบบนวัด overflow = 0px |
| Completion Report | `COMPLETION_REPORT.md` ครบ 10 หัวข้อตาม `AGENTS.md` §7 — Codex ส่งในรูปไฟล์ใน repo ตามที่ขอ (ปิด PIA-002) |

การตรวจรับ: Product Owner commit `2a8bd34` (implementation) + `418ef45` (report), push ขึ้น `origin/main` และยืนยัน "approve" ในแชท 2026-07-19

**Finding ใหม่จากรอบนี้:**

#### PIA-003 — Cosmetic deviation ใน `userBar()` ไม่ถูกรายงานใน Completion Report

- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: diff hunk `userBar()` — โลโก้ 20→18px, "ORNSIRIN" 10.5→9.5px, avatar สมาชิก 22→20px, padding ปุ่มออกลดลง — แต่ `COMPLETION_REPORT.md` §6 ระบุ "Implementation deviations: None"
- Observed: การบีบ layout เพื่อให้กระดิ่งลงพอดีเป็นการตัดสินใจสมเหตุสมผล ผลลัพธ์ดี (วัด overflow = 0px, ดู screenshot ระหว่าง audit) แต่ไม่ถูก disclose
- Expected: การปรับ visual ต่อบริเวณเดิมควรถูกระบุใน Deviations แม้ทำเพื่อรองรับ scope ที่อนุมัติ
- Impact: จำกัดอยู่ที่ความครบของ audit trail — ไม่กระทบพฤติกรรม
- Recommendation: รอบถัดไป Codex ต้องระบุ cosmetic adjustment ทุกจุดใน Completion Report §6
- Scope relation: Process improvement — ไม่ต้องแก้โค้ด

#### PIA-004 — Queue state คงอยู่ข้าม logout (expected behavior)

- Severity: LOW (observation) · State: CONFIRMED · Confidence: High
- Evidence: handler logout ล้างเฉพาะ `modal`/`bookingSuccess` — `state.queue`/`queuePaid` คงอยู่ แต่ถูกซ่อนสนิทตอน logged-out (แบนเนอร์/คีย์ลัด/timer/กระดิ่ง gate ด้วย `loggedIn` ทั้งหมด — ตรวจ runtime แล้ว) และกลับมาเมื่อล็อกอินใหม่
- Observed: queue object ไม่มีข้อมูลส่วนบุคคล (`{position, wait}`) — ไม่ใช่ privacy leak
- Impact: เป็นข้อดีต่อสคริปต์เดโม — สาธิต logout/privacy gate ได้โดยไม่เสียคิว
- Recommendation: คงพฤติกรรมนี้ไว้ บันทึกเป็น expected behavior
- Scope relation: No action

### 5.9 INC-C specification — Escape ข้อความจากผู้ใช้ก่อนเข้า `innerHTML` (ปิด AUD-002)

**Objective:** เดโมสดพิมพ์อะไรก็ไม่ทำหน้าแตก — ปิดช่อง markup injection จากข้อความที่ผู้ใช้พิมพ์ ก่อนเข้า template ทุกจุด

**Design บังคับ:** เก็บ **raw** ใน state/sessionStorage เหมือนเดิม — escape **ณ จุด render เท่านั้น** (ห้าม escape ตอนเก็บ กัน double-escape สะสมข้าม refresh) · helper เดียว `esc(s)` แทนที่ `&` ก่อนแล้วตาม `<` `>` `"` `'` — ครอบทั้ง text content และ attribute value

**Sink inventory ขั้นต่ำ** (Codex ต้อง sweep เพิ่มให้ครบระหว่าง implement และรายงาน inventory สุดท้ายใน Completion Report):

| แหล่ง user input | จุด render ที่พบ (line โดยประมาณ ณ `e2a50ec`) |
|---|---|
| `state.symptom` (พิมพ์อาการ) | textarea ใน `consultForm()` (~1091) และทุกจุดที่ข้อความอาการแสดงซ้ำ |
| `pet.name` / `pet.breed` / `pet.age` (ฟอร์มเพิ่ม/แก้ไขสัตว์เลี้ยง) | การ์ดโปรไฟล์ (~1576–1577) · ชิปวัคซีน (~1587) · **ฟอร์มแก้ไข `value="${…}"` — attribute context** (~1827–1838) · dialog ลบโปรไฟล์ (~2316) · booking details (~2347) · pet picker ใน `detailModal()` |

ข้อมูล mock คงที่ (users, doctors, services, ราคา) **ไม่ escape** — ไม่ใช่ user input

**Non-goals:** ไม่เปลี่ยน schema/state · ไม่เพิ่ม validation/จำกัดอักขระ · ไม่แตะ flow อื่น

**Acceptance Criteria:**
- AC-C-01: มี escape helper เดียว ใช้ซ้ำทุก sink ของ user input — ไม่มี double-escape
- AC-C-02: ตั้งชื่อสัตว์ `<img src=x onerror="window.__xss=1">` → ชื่อแสดงเป็นข้อความตรงตัวทุกจุด (การ์ด, ชิปวัคซีน, ฟอร์มแก้ไข, picker ตอนจอง, รายละเอียดนัด, dialog ลบ) และ `window.__xss` ไม่ถูกตั้งค่า
- AC-C-03: พันธุ์/อายุที่มี `"` → ไม่หลุดออกนอก attribute (ค่าเต็มอยู่ใน input เดียว ไม่เกิด attribute ใหม่)
- AC-C-04: พิมพ์อาการ `</textarea><b>x</b>` → textarea แสดงข้อความตรงตัว ไม่มี element ใหม่เกิด และ refresh แล้วข้อความเดิมยังครบถูกต้อง
- AC-C-05: ข้อความ mock เดิม spot-check หน้า home/meds/lifestyle/lab-results แสดงเหมือนก่อนแก้ทุกจุด — ไม่มี `&amp;` หลุดให้เห็น

### 5.10 INC-G specification — โหมดผู้สูงอายุของคุณยายบุญมี

**Objective:** สลับเป็นคุณยายบุญมี (u3) แล้วอ่านง่ายขึ้นทันที + การ์ด SOS — จุดขายสำหรับผู้ฟังกลุ่มหมู่บ้านผู้เกษียณ (TODO P2 ตัวรองสุดท้าย)

**User-visible outcome:**
1. ล็อกอิน + `currentUser === "u3"` → โหมดผู้สูงอายุ**อัตโนมัติ** (ไม่มีปุ่ม toggle): เนื้อหาขยาย ~15% ทั้งหน้าจอหลักและ sheet/dialog · สลับกลับ u1/u2/u4 → ขนาดปกติทันที
2. การ์ด **SOS เฉพาะ u3** บนหน้าแรก ตำแหน่งเด่น (แนะนำ: ใต้ hero เหนือปุ่มบริการ) โทนแดง/clay ไอคอนโทรศัพท์ ตัวหนังสือใหญ่ "SOS แจ้งเหตุฉุกเฉิน · แตะเพื่อเรียกทีมดูแล"
3. แตะ SOS → dialog จำลอง (modal kind ใหม่ `sos` — ใช้ dialog/backdrop animation ชุดเดิม และ**ต้องเคารพ `overlayAllowed`/`loggedIn` gating เหมือน modal อื่น**): "ทีมดูแลอรสิรินรับเรื่องแล้ว กำลังโทรกลับภายใน 1 นาที (จำลอง)" + ปุ่มปิด — ไม่มีทางตัน ไม่โทรจริง

**ข้อควรระวังทางเทคนิค (สำคัญ):** font-size ในแอปเป็น **inline style แทบทั้งหมด** — CSS class ธรรมดา override ไม่ได้ · แนวทางแนะนำ: `zoom` (หรือกลไกเทียบเท่า) บนคอนเทนเนอร์ `#app` + `#modal-root` ผ่านคลาสเดียว (เช่น `elder-mode`) — compose กับ `fitPhone()` zoom บน `#phone` ได้เพราะคนละ element · Codex เลือกกลไกอื่นได้ถ้าผ่าน AC ครบ

**Non-goals:** ไม่มีปุ่มเปิด/ปิดแยก (ผูกกับ persona) · ไม่เปลี่ยน copy/ราคา/สิทธิ์/ข้อมูล u3 · ไม่ redesign รายหน้า · ไม่มีการโทรหรือส่งข้อมูลจริง

**Acceptance Criteria:**
- AC-G-01: สลับเป็น u3 → เนื้อหาขยายเห็นชัด (~1.1–1.2×) ทุกหน้า; สลับกลับ → ขนาดเดิมเป๊ะ
- AC-G-02: การ์ด SOS แสดงเฉพาะ u3 ที่ล็อกอินแล้วบนหน้าแรก — ไม่แสดงก่อนล็อกอิน/สมาชิกอื่น/หน้าอื่น
- AC-G-03: แตะ SOS → dialog จำลองพร้อมปุ่มปิด ปิดแล้วกลับหน้าเดิมโดยสถานะอื่นไม่กระทบ (queue/bookings/pets เดิมครบ)
- AC-G-04: elder mode ไม่ทำ layout แตก — ไม่มี horizontal overflow ในหน้า home/consult/lab/meds/lifestyle + sheet จอง และใช้กับจอ 1366×768 ได้ (`fitPhone()` ยังทำงาน)
- AC-G-05: refresh ระหว่างเป็น u3 → โหมดคงอยู่ · logout → หน้า generic ขนาดปกติ · ฟีเจอร์เดิมของ u3 (ผลแล็บ, กระดิ่ง, แบนเนอร์คิว, สัตว์เลี้ยง 2 ตัว) ทำงานปกติในโหมดนี้

### 5.11 PIA-001 fix — bottom nav active บนหน้า lab-results

**Objective:** หน้า "ผลตรวจของฉัน" ให้แท็บ "เจาะเลือด" ติด active (ปัจจุบันไม่มีแท็บใด active)

**Evidence:** `bottomNav()` — `const isActive = (id) => id === state.screen || (id === "consult-specialty" && state.screen === "consult-form");` — ไม่มี mapping ของ `"lab-results"`

**แนวทาง:** เพิ่มเงื่อนไขเดียว `|| (id === "lab" && state.screen === "lab-results")`

**Acceptance Criteria:**
- AC-P1-01: อยู่หน้า lab-results → แท็บ "เจาะเลือด" active (สี/พื้นหลังเหมือนตอนอยู่หน้า lab) และแท็บอื่นไม่ active
- AC-P1-02: ทุกหน้าอื่น highlight เหมือนเดิมเป๊ะ (home, consult-specialty, consult-form→ปรึกษาหมอ, lab, meds, lifestyle)

### 5.12 Round execution terms (INC-C + INC-G + PIA-001 + README)

- **งานโค้ด 3 รายการ = หนึ่งรอบ Codex** · diff เดียว ไฟล์เดียว (`index.html`) · ลำดับแนะนำ: INC-C ก่อน (แตะหลาย template — ทำตอน tree สะอาด) → INC-G → PIA-001
- **งานเอกสาร README = Claude** ทำหลัง audit ของ batch นี้ PASS และผู้ใช้ตรวจรับ (เขียนครั้งเดียวรวม INC-G) — เนื้อหาครอบคลุม: หน้าผลแล็บ (INC-F), กระดิ่งแจ้งเตือน (INC-E), privacy gate ก่อนล็อกอิน (INC-B), ตัวควบคุมคิวผู้พรีเซนต์ (INC-A — เพิ่มคีย์ลัดในหมวดเคล็ดลับเดโม), โหมดผู้สูงอายุ + SOS (INC-G), ปรับตาราง persona (คุณยายบุญมี = โหมดผู้สูงอายุ)
- Precondition: `index.html` working tree สะอาด ณ เวลาอนุมัติ (baseline `e2a50ec` — ผู้ใช้ commit v0.5 แล้ว) — doc diff ของ `SPEC.md` v0.6 ที่ค้างไม่ block
- Verification รวม: AC 12 ข้อ (C-01…05, G-01…05, P1-01…02) + มาตรฐานเดิม (syntax check, console 0 error, offline ไม่มี external ref ใหม่, `git status` ไฟล์เดียว, ไม่แตะบริเวณ INC-F/celebration/batch INC-A+E+B) + **Completion Report ต้องระบุ cosmetic deviation ทุกจุด** (บทเรียนจาก PIA-003)
- Rollback: revert diff เดียว — ไม่กระทบ `e2a50ec` และก่อนหน้า

### 5.13 Batch INC-C+G+PIA-001 Post-implementation Audit result (2026-07-19)

**ผล: PASS — 12/12 Acceptance Criteria** · ยืนยันสองชั้น: (1) Claude ตรวจ runtime ด้วยตนเองครบทุก AC ผ่านสำเนา byte-identical; (2) ผู้ตรวจอิสระ 4 ราย (Opus 4.8) ลงมติ PASS ตรงกันทั้งหมด — รวมตัวที่ได้รับมอบหมายให้ "หักล้างผล PASS" ซึ่งขับ browser ทดสอบซ้ำเองแล้วก็ยังไม่พบข้อบกพร่อง

| หัวข้อ | ผลตรวจ |
|---|---|
| Diff review | 16 hunk (53+/11− ณ commit `ca68ddb`) ทุก hunk map เข้า INC-C / INC-G / PIA-001 — ไม่มี hidden scope |
| Runtime AC | 12/12 PASS (Claude ตรวจเอง + ผู้ตรวจอิสระยืนยันซ้ำ) ผ่านสำเนา byte-identical (hash `62d25830…` ก่อน/หลัง audit ไม่เปลี่ยน — Claude ไม่แตะ implementation) |
| INC-C completeness | ไล่ทุก user-input sink (อาการ + ชื่อ/พันธุ์/อายุ/น้ำหนักสัตว์) — escape ครบทุกจุด รวม attribute context และ detail ของ dialog ลบ; `esc()` แทน `&` ก่อน ไม่เกิด double-escape; เก็บ raw ตอน persist |
| INC-G | elder mode gate = `loggedIn && currentUser==="u3"` (zoom 1.15 บน `#app`+`#modal-root`) · SOS card เฉพาะ u3 บนหน้าแรก · dialog gated ด้วย overlayAllowed · ไม่มี horizontal overflow ที่ 1366×768 (วัดจริง over=0), `fitPhone` ไม่ถูกรบกวน (`#phone` สูงคงที่ 844px) |
| PIA-001 | เพิ่ม OR-clause เดียวใน `bottomNav` — lab-results → แท็บ lab active เดี่ยว ทุกหน้าอื่น mapping เดิมเป๊ะ |
| Console / offline | 0 error · ไม่มี external ref ใหม่ใน diff · เปิด file:// ไม่มี network request (เหลือแค่ data: URI ของไอคอน) |
| งานเก่า | `labResultsScreen()` (INC-F) byte-identical · celebration block เหมือนเดิม · `successModal` ต่างแค่บรรทัด `esc(b.detail)` ตามคาด · batch INC-A+E+B ไม่ถูก refactor |
| Completion Report | ถูกต้องตามข้อเท็จจริง (diff stat, baseline blob, cosmetic disclosure 4 จุด ตรงกับ diff) — ปฏิบัติตามกติกาใหม่ PIA-003 (เปิดเผย cosmetic ครบ) |

การตรวจรับ: Product Owner commit `ca68ddb` + push และยืนยัน "approve" ในแชท 2026-07-19

**Finding ใหม่จากรอบนี้:**

#### PIA-005 — esc() ครอบ `pet.weight` (mock) ด้วย — inert, disclosed

- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: `lifestyleScreen()` — `[pet.breed, pet.age, pet.weight].filter(Boolean).map(esc).join(" · ")` · `pet.weight` เป็น mock (ไม่มี input เก็บค่านี้) แต่ถูก escape ร่วมใน map เดียวกับพันธุ์/อายุที่เป็น user input
- Observed: ค่า mock ทั้งสาม ("28 กก." ฯลฯ) ไม่มีอักขระพิเศษ จึงแสดงผลเหมือนเดิมทุกประการ (AC-C-05 ยังผ่าน) และ Codex เปิดเผยใน Completion Report §6 แล้ว
- Impact: ไม่มี — เป็นการเผื่อเหนียวที่ปลอดภัย
- Recommendation: ไม่ต้องแก้
- Scope relation: No action

#### PIA-006 — SOS เข้าถึงได้จากหน้าแรกเท่านั้น (ตรงสเปก)

- Severity: LOW (observation) · State: CONFIRMED · Confidence: High
- Evidence: ปุ่ม `data-sos-open` อยู่ใน `homeScreen()` เท่านั้น
- Observed: ตรงสเปก §5.10 ทุกประการ · ถ้าต้องการให้เข้าถึงได้ทุกหน้าในโหมดผู้สูงอายุค่อยพิจารณา increment แยกในอนาคต
- Recommendation: ไม่ต้องแก้ในรอบนี้
- Scope relation: Candidate (future)

### 5.14 INC-D specification — Storage recovery เมื่อ `sessionStorage` พัง/เต็ม (ปิด AUD-007)

**Objective:** เดโมไม่พังเป็นจอขาวหรือค้างสภาพเสีย ถ้า `sessionStorage` มีข้อมูลเน่าหรือเขียนไม่ได้ (quota เต็ม) — กู้กลับสู่เดโมสดเริ่มต้นอย่างสะอาด

**Current behavior (evidence ณ `ca68ddb`):**
- `persistState()` (index.html:649-651): `try { sessionStorage.setItem(...) } catch(e){}` — quota เต็มถูกกลืนเงียบ (แอปยังทำงานจาก in-memory ได้ — ยอมรับได้)
- `hydrateState()` (index.html:652-660): ถ้า `JSON.parse` ล้ม → catch แล้ว `return` เฉย ๆ **แต่ไม่ล้าง key ที่เน่า** → เปิดหน้าใหม่ก็ล้มซ้ำทุกครั้ง; และไม่มีการตรวจว่า parsed เป็น object จริงก่อน `Object.assign`

**Required behavior:**
1. `hydrateState()`: ถ้า parse ล้ม **หรือ** ค่าที่ได้ไม่ใช่ plain object → ล้าง `sessionStorage[STATE_KEY]` แล้วบูตด้วย state ตั้งต้น (screen "home", loggedIn false) โดยไม่ throw ไม่จอขาว
2. `persistState()`: คง try/catch กันแอปพัง — quota เต็ม แอปเดินต่อจาก in-memory ได้ (ไม่ต้องมี toast เพื่อลดสิ่งรบกวนกลางเดโม)
3. ไม่เปลี่ยน `STATE_KEY`, schema หรือพฤติกรรม F5 ปกติ

**Non-goals:** ไม่ทำ migration ข้าม schema version · ไม่เพิ่ม UI ตั้งค่า · ไม่ย้ายไป localStorage

**Acceptance Criteria:**
- AC-D-01: ใส่ JSON เน่า (เช่น `"{oops"`) ลง `sessionStorage[STATE_KEY]` แล้ว reload → บูตหน้า home สภาพสด ไม่มี error ค้าง console ไม่จอขาว และ key ที่เน่าถูกล้าง
- AC-D-02: ใส่ JSON ที่ valid แต่ไม่ใช่ object (`"123"`, `"\"x\""`, `"null"`) → บูตสะอาด `state.screen === "home"` ไม่ crash
- AC-D-03: จำลอง quota เต็ม (stub `setItem` ให้ throw) แล้วทำ action ที่เรียก `setState` → render สำเร็จ แอปยังกดต่อได้ ไม่มี uncaught error
- AC-D-04: กรณีปกติ — F5 กลางเดโมยังกู้ state ครบเป๊ะ (regression guard: bookings/queue/loggedIn/currentUser/symptom)

### 5.15 INC-J specification — บัตรสุขภาพฉุกเฉิน (Emergency Health Profile)

**Objective:** ให้ข้อมูลฉุกเฉินสำคัญ (กรุ๊ปเลือด แพ้ยา โรคประจำตัว ผู้ติดต่อฉุกเฉิน) พร้อมส่งให้ทีมดูแลทันทีที่กด SOS — ต่อยอด SOS + โหมดผู้สูงอายุให้เป็นชุดดูแลผู้สูงอายุที่สมบูรณ์

**Data (จำลองทั้งหมด ตาม ASM-002):** เพิ่มโปรไฟล์ฉุกเฉินของ **u3 (คุณยายบุญมี)** เป็นอย่างน้อย — สอดคล้องผลแล็บเดิม (INC-F: FBS/LDL "ควรติดตาม"):
- กรุ๊ปเลือด: O
- แพ้ยา: เพนิซิลลิน
- โรคประจำตัว: ความดันโลหิตสูง, เบาหวานชนิดที่ 2
- ผู้ติดต่อฉุกเฉิน: สมหญิง สุขใจ (บุตรสาว) · เบอร์จำลอง (ห้ามใช้เบอร์จริง)
(u1/u2 ใส่ได้ถ้าไม่เพิ่มงานมาก — ไม่บังคับ)

**User-visible outcome:**
1. บนหน้าแรกของ u3 บริเวณเดียวกับปุ่ม SOS: ปุ่ม/ลิงก์ "บัตรสุขภาพฉุกเฉิน" เปิด modal ใหม่ (`kind: "healthCard"`, ใช้ animation ชุด dialog เดิม, เคารพ `overlayAllowed`) แสดงบัตรครบทุกช่อง
2. เมื่อ u3 กด SOS → dialog ยืนยัน (`sosModal`) แสดง**สรุปบัตรสุขภาพฉุกเฉิน**เพิ่ม (กรุ๊ปเลือด/แพ้ยา/โรคประจำตัว) พร้อมข้อความ "ข้อมูลนี้ถูกส่งให้ทีมดูแลแล้ว" — ทีมเห็นข้อมูลทันที

**Non-goals:** ไม่มีฟอร์มแก้ไขบัตร (ข้อมูล seed) · ไม่เชื่อมข้อมูลจริง/ไม่โทรจริง · ไม่บังคับทำให้ครบทุก user

**Affected regions:** data ใหม่ (`emergencyProfileByUser` หรือ field ใน `users`) · `sosModal()` · ฟังก์ชัน `healthCardModal()` ใหม่ + branch/handler · u3 home region

**Acceptance Criteria:**
- AC-J-01: u3 มีโปรไฟล์ฉุกเฉินครบ 4 หมวด (กรุ๊ปเลือด/แพ้ยา/โรคประจำตัว/ผู้ติดต่อฉุกเฉิน) เป็นข้อมูลจำลอง
- AC-J-02: u3 กด SOS → dialog แสดงสรุปบัตร (กรุ๊ปเลือด/แพ้ยา/โรคประจำตัว) + ข้อความว่าส่งให้ทีมดูแลแล้ว
- AC-J-03: เปิด "บัตรสุขภาพฉุกเฉิน" → modal แสดงบัตรครบ ปิดกลับหน้าเดิมโดยสถานะอื่นไม่กระทบ
- AC-J-04: ค่าทุกช่อง render ปลอดภัย (ผ่าน esc ถ้าเป็น text) และไม่ล้นกรอบใน elder mode + 1366×768 (dialog เลื่อนได้ถ้าจำเป็น)
- AC-J-05: ก่อนล็อกอิน/ผู้ใช้อื่นที่ไม่มีข้อมูล → ไม่มีบัตร/ข้อมูลฉุกเฉินหลุด

### 5.16 INC-K specification — ผู้ดูแล / แจ้งลูกหลาน (Care Circle)

**Objective:** สาธิต "ลูกหลานอยู่ไกลก็อุ่นใจ" — ครอบครัวที่กำหนดเป็นผู้ดูแลได้รับแจ้ง (จำลอง) เมื่อผู้สูงอายุกด SOS · จุดขายตรงกลุ่มหมู่บ้านผู้เกษียณ

**Data (จำลอง):** เพิ่มความสัมพันธ์ผู้ดูแลให้ u3 — ผู้ดูแล = สมหญิง สุขใจ (บุตรสาว) · ใช้ชื่อจาก `users` เดิมเพื่อความต่อเนื่อง (ตระกูล "สุขใจ")

**User-visible outcome:**
1. บนหน้าแรกของ u3: การ์ด/แถบ "ผู้ดูแล" แสดงว่าใครกำลังดูแล — "ผู้ดูแล: สมหญิง สุขใจ (บุตรสาว) · ได้รับแจ้งอัตโนมัติเมื่อกด SOS"
2. เมื่อ u3 กด SOS → dialog ยืนยันแสดงบรรทัดเพิ่ม "✓ แจ้งผู้ดูแล (สมหญิง) แล้ว" (จำลอง — ไม่ส่งข้อความจริง)

**Non-goals:** ไม่มีระบบเชิญ/แก้ไขผู้ดูแล · ไม่ส่ง SMS/push จริง · ไม่ทำ two-way chat · จำกัดที่ u3 (ผู้ใช้อื่นไม่ต้องมีผู้ดูแลในรอบนี้)

**Affected regions:** caregiver data (field ใน u3 หรือ map) · `sosModal()` (บรรทัดแจ้งผู้ดูแล) · u3 home region (การ์ดผู้ดูแล) — **ทับซ้อนกับ INC-J ที่ SOS dialog + u3 home จึงทำในรอบเดียว**

**Acceptance Criteria:**
- AC-K-01: u3 มีข้อมูลผู้ดูแล (ชื่อ + ความสัมพันธ์) เป็นข้อมูลจำลอง
- AC-K-02: หน้าแรก u3 แสดงการ์ด/แถบผู้ดูแลระบุชื่อผู้ดูแล — ไม่แสดงก่อนล็อกอิน/ผู้ใช้อื่น/หน้าอื่น
- AC-K-03: u3 กด SOS → dialog มีบรรทัด "แจ้งผู้ดูแล (ชื่อ) แล้ว"
- AC-K-04: ข้อมูลผู้ดูแลจำลองล้วน ไม่มีการส่งข้อความ/นำทางออกจริง และไม่หลุดก่อนล็อกอิน

### 5.17 Batch execution terms (INC-D + INC-J + INC-K)

- ทำเป็น **หนึ่งรอบ Codex** · diff เดียว ไฟล์เดียว (`index.html`) · ลำดับแนะนำ: **INC-D ก่อน** (แยกส่วน persistence, ไม่ชนใคร) → **INC-J** (data + healthCard + SOS) → **INC-K** (caregiver + SOS + u3 home) เพราะ J/K ทับซ้อนที่ `sosModal()` และหน้าแรก u3
- Precondition: application blob = `ca68ddb` (`index.html` สะอาด ณ เวลาอนุมัติ) — doc diff v0.7 ที่ค้างไม่ block
- Verification รวม: AC 13 ข้อ (D-01…04, J-01…05, K-01…04) + มาตรฐานเดิม (syntax check, console 0 error, offline ไม่มี external ref ใหม่, `git status` ไฟล์เดียว) + ไม่แตะบริเวณ INC-F/celebration/batch ก่อนหน้า + คง escaping (INC-C), elder mode/SOS เดิม (INC-G) ทำงานปกติ + **Completion Report ระบุ cosmetic deviation ทุกจุด**
- Rollback: revert diff เดียว — ไม่กระทบ `ca68ddb` และก่อนหน้า

### 5.18 Batch INC-D+J+K Post-implementation Audit result (2026-07-19)

**ผล: PASS — 13/13 Acceptance Criteria** (Claude ตรวจ runtime ด้วยตนเองครบทุก AC ผ่านสำเนา byte-identical) · Product Owner ตรวจรับแล้ว (DEC-015)

| หัวข้อ | ผลตรวจ |
|---|---|
| Diff review | 127+/5− (`index.html` ไฟล์เดียว) ทุก hunk map เข้า INC-D / INC-J / INC-K — ไม่มี hidden scope |
| INC-D | JSON เน่า → บูตสด + ล้าง key ที่เสีย (แก้ปัญหาเดิมที่ล้มซ้ำ) · non-object (`123`/`"x"`/`null`/`[]`) → ปฏิเสธ+ล้าง · quota เต็ม (stub throw) → `setState` เดินต่อ ไม่มี uncaught error · round-trip ปกติกู้ครบ (bookingSeq 42) |
| INC-J | บัตรสุขภาพครบ 4 หมวด · SOS แสดงสรุป + "ส่งให้ทีมดูแลแล้ว" · เปิด/ปิดบัตรสถานะอื่นครบ · **inject XSS เข้า data จริง → esc() กันได้ทุก sink** · 1366×768 + elder: dialog ในกรอบ overflow 0 เลื่อนได้ |
| INC-K | ผู้ดูแล = สมหญิง (บุตรสาว) จาก users เดิม · การ์ดผู้ดูแลบนหน้าแรก u3 · SOS "แจ้งผู้ดูแล (สมหญิง) แล้ว · จำลอง" · ไม่มี link/form/network |
| Privacy | ก่อนล็อกอิน / u1-u2-u4 / u3 หน้าอื่น — ไม่มี SOS/บัตร/ผู้ดูแล/กรุ๊ปเลือดหลุด · เปิด modal healthCard ตอน logged-out ถูก `overlayAllowed` บล็อก |
| Regression | `esc`, `bottomNav`, `notificationsModal`, `labResultsScreen`, `successModal`, `persistState` byte-identical กับ `ca68ddb` · เปลี่ยนแค่ `hydrateState` (ตาม INC-D) |
| Console / offline | 0 error · ไม่มี external ref ใหม่ · network เหลือแค่ data: URI ไอคอน |
| Completion Report | ถูกต้องตามข้อเท็จจริง (diff stat 127+/5−, baseline blob) · เปิดเผย cosmetic ครบตามกติกา PIA-003 |

การตรวจรับ: Product Owner commit `a948687` + push และยืนยัน "เรียบร้อย" ในแชท 2026-07-19

**Finding ใหม่จากรอบนี้:**

#### PIA-007 — hash algorithm ในเอกสารไม่ตรงกันระหว่าง Claude (SHA-1) กับ Codex (SHA-256)

- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: SPEC บันทึก `62d25830…` (SHA-1 จาก `shasum`) แต่ Completion Report v0.8 §1/§6 ตีความว่าเป็น SHA-256 แล้วรายงานว่า "ไม่ตรง" · ตรวจแล้ว blob `ca68ddb:index.html` = SHA-1 `62d25830…` = SHA-256 `0faf0c33…` (ไฟล์เดียวกันเป๊ะ)
- Observed: ไม่ใช่ application drift — เป็นความสับสนของอัลกอริทึม hash
- Impact: จำกัดที่ audit trail — ไม่กระทบโค้ด
- Recommendation: ระบุใน Document Control ว่า hash เป็น SHA-1 (ทำแล้วในหมายเหตุ Approval Gate v0.9) · รอบหน้าใช้อัลกอริทึมเดียวกันทั้งสองฝ่าย
- Scope relation: Process — ไม่ต้องแก้โค้ด

#### PIA-008 — quota degradation ไม่ persist action หลังเต็ม (expected, non-goal)

- Severity: LOW (observation) · State: CONFIRMED · Confidence: High
- Evidence: INC-D คง persist แบบ silent try/catch — quota เต็มแล้ว action ถัดไปทำงานใน in-memory ได้ แต่ไม่ถูกเก็บ (หายเมื่อ reload)
- Observed: เป็น silent degradation ที่สเปก §5.14 อนุมัติไว้ (ไม่ทำ toast กันรบกวนเดโม)
- Recommendation: ไม่ต้องแก้ · หากรอบอนาคตต้องการเตือนผู้ใช้ค่อยเพิ่ม
- Scope relation: No action

### 5.19 INC-L specification — Design polish pack (user-friendly + น่าดึงดูด)

**Objective:** ยกระดับความสวย/อ่านง่าย/ลื่นไหลทั้งแอปจากผล design audit อิสระ 4 มุมมอง (visual consistency, UX friction, elder readability, delight) — โดย**ห้ามแตะ**จุดแข็งที่บันทึกไว้ (ระบบ motion replay-safe, service color coding, screenHero pattern, การ์ด/ปุ่มมาตรฐานเดิม, payment celebration)

**ที่มา:** design audit 2026-07-19 (ผู้ตรวจอิสระ 4 ราย, 49 findings + 33 strengths, evidence ต่อรายการเป็น template+line ณ `a948687`)

**งานแบ่ง 5 กลุ่ม (ทุกข้อเป็น template/inline-style/copy edit — ไม่มี architecture change):**

**L-A ขายตั้งแต่วินาทีแรก:**
1. Hero ก่อนล็อกอิน: copy ขายแทนสั่ง ("ดูแลสุขภาพและไลฟ์สไตล์ของทุกคนในบ้าน — หมอ พยาบาล และทีมดูแล ถึงหน้าบ้านคุณ") + แถวชิป benefit 3 ชิ้นสไตล์ pill เดิมของ hero (ลูกบ้านลด 15% · ส่งถึงบ้านฟรี · ดูแลทั้งครอบครัวรวมสัตว์เลี้ยง) + คำสั่งล็อกอินลดเป็น hint เล็ก (~line 925-933)
2. ราคาปรึกษา ฿400 (+฿100 ใบรับรอง) แสดงบนฟอร์มก่อนกด "ส่งคำขอปรึกษา"
3. `consultForm` เปลี่ยนจาก `topBar` เป็น `screenHero` โทนเขียวเดียวกับ specialty (~line 1126) — ปรับการ์ดจิตแพทย์ใต้ hero ไม่ให้เขียวซ้อนเขียว

**L-B อ่านง่าย (ผู้สูงอายุ/โปรเจกเตอร์):**
4. เพิ่ม token `--ink-soft:#5B6350` แทน #8A9080 (×33) และ #7A8271 (×14) ทั้งหมด
5. Bottom nav label 10→11.5px + w500 + inactive ใช้ #5B6350
6. เวลารอคิวแยกบรรทัด 13px #5B6350 เด่น (~line 1226)
7. ชิปผลแล็บ 9.5→11px หนา สีเข้มขึ้น (#7A5115/#F8E8C7, #3D5636/#DDE9D7) + icon alert-circle นำหน้า "ควรติดตาม"
8. บรรทัดสิทธิ์ลูกบ้าน hero 11→12px #E7EFDD w500 · ค่าแพ้ยา healthCard 12.5→15px w600 · label ใน SOS summary 9.5→11px
9. แถบบน: avatar 20→26px (12px), กระดิ่ง 25→30px, ปุ่มออก py-1.5 · ชื่อผู้ใช้ max-width 88→64px ชดเชย
10. ชิปอาการ py-2.5 + 13px · ปุ่มปิด overlay ทุกตัว w-10 + aria-label="ปิด" · caption ช่องเวลา 8.5→10px #6D7766

**L-C ความสม่ำเสมอ (mechanical):**
11. หัว modal ทุกตัว 17px (bookingDetail 15.5, detail/addPet/success 16 → 17)
12. chevron-right มาตรฐาน 16px บนการ์ดเต็มแถว (คง 13px เฉพาะแถวสถานะ pet)
13. เงา card ครอบ `#app .rounded-3xl` ด้วย (lab-results empty + login gate)
14. หัว section หน้า service เป็น serif-th 14px semibold (~6 จุด)
15. token `--danger:#B0543F` + แก้ #C0503F 2 จุด (ปุ่มวางสาย, validation border)
16. ฟอร์มล็อกอินใช้ palette (แทน text-black/text-gray-400) · icon ยาใส่ color:var(--clay) · gradient hero ใช้ var(--primary/-dark)
17. Empty state หน้ายา ยกระดับเป็นแบบหน้าผลแล็บ (icon circle + serif heading + hint)

**L-D ลด friction:**
18. หน้าคิวยังไม่จ่าย: ปุ่มหลัก = "ชำระเงินเพื่อยืนยันคิว" · "กลับหน้าแรก" เป็น secondary
19. ปิด payment sheet ของ lifestyle กลางคัน → กลับ detail sheet โดย selection ครบ (เดิมทิ้งหมด)
20. Payment คิวปรึกษามี line items (ค่าปรึกษา ฿400 · ใบรับรอง +฿100)
21. Disabled CTA มี helper caption + aria-disabled: "ถัดไป" หน้า specialty ("เลือกแผนกก่อน") และ "ส่งคำขอปรึกษา" ("เลือกอาการหรือพิมพ์อย่างน้อย 5 ตัวอักษร")
22. หมอไม่ว่างมี label "ไม่ว่าง" (pattern จาก detailModal) + opacity 0.6
23. การ์ด carousel ไลฟ์สไตล์: กดได้ทั้งใบ + icon circle สีตาม service + chevron แทน "→" + ชิป "ลูกบ้าน −15%" (resident) / ราคาเริ่มต้น (non-resident)
24. Day picker: ถ้า slot วันนี้ผ่านหมด default "พรุ่งนี้" (เดโมตอนเย็น)
25. แสดงหน่วยราคา (/ชม., /ครั้ง) ตาม data เดิม · แก้ "เป็นเวลา 00:23 นาที" ให้ format ถูก

**L-E จังหวะมีชีวิต (reuse motion เดิม + reduced-motion ครบ):**
26. จบสาย VDO: การ์ดหมอ (ชื่อ+staffAvatar) + ระยะเวลา + "✓ สรุปคำแนะนำและใบสั่งยา ส่งเข้าแอปใน 15 นาที" + title "ขอบคุณที่ดูแลตัวเองวันนี้"
27. ผลแล็บ stagger reveal (CSS scoped ใต้ .screen-enter เท่านั้น — re-render ภายในนิ่ง)
28. สายเชื่อมต่อแล้ว: วง avatar หายใจต่อ (class listen-ring เดิมให้วง static ~1289)
29. ถึงคิว #0: ปุ่ม CTA ได้ exhale-once (ใน guard prevQueuePos เดิม)
30. แจ้งเตือนคิว: chip clay + ป้าย "ด่วน" · badge กระดิ่ง exhale เมื่อจำนวนเปลี่ยน (prevNotifCount pattern เดิม) · icon healthCard exhale ตอนเปิด

**Non-goals (candidate รอบหน้า):** แจ้งเตือน per-user จริง · dialog ยืนยันก่อนยกเลิกนัด · พฤติกรรม SOS (อยู่ใน INC-M) · redesign นอกรายการ

**Acceptance Criteria (กลุ่มละหนึ่ง pass/fail):**
- AC-L-01: กลุ่ม A ครบ 3 — hero ขาย+ชิป, ราคาบนฟอร์ม, consultForm screenHero ไม่เขียวซ้อน
- AC-L-02: กลุ่ม B — ไม่เหลือ #8A9080/#7A8271 ในไฟล์, ขนาด/สีตามระบุ, contrast จุดที่แก้ ≥4.5:1
- AC-L-03: กลุ่ม C — หัว modal 17px ทุกตัว, chevron มาตรฐาน, เงา 3xl, section serif, ไม่เหลือ #C0503F/text-gray-400/text-black, meds empty ใหม่
- AC-L-04: กลุ่ม D ครบ 8 (18–25) — flow จริง: ปิด payment lifestyle แล้ว selection อยู่, default พรุ่งนี้เมื่อ slot หมด, hierarchy ปุ่มคิวใหม่
- AC-L-05: กลุ่ม E ครบ 5 (26–30) — เล่นครั้งเดียวตอนเหตุการณ์จริง, ไม่ replay ตอน re-render, ปิดสนิทใต้ reduced-motion
- AC-L-06: Guard — จุดแข็งไม่ถูกแตะ (motion gating, service colors, celebration), ไม่มี horizontal overflow (ปกติ/1366×768/elder), console 0, offline เดิม

### 5.20 INC-M specification — SOS เรียกรถพยาบาล (จำลองเต็มรูปแบบ)

**Objective:** เปลี่ยน SOS จาก "โทรกลับใน 1 นาที" เป็น **flow เรียกรถพยาบาลแบบ ride-hailing** — ไคลแมกซ์ใหม่ของเรื่องเล่าผู้สูงอายุ · **จำลอง 100%** (ASM-002): ไม่มี tel:, ไม่มี network, ป้าย "จำลอง" ชัดเจนถาวร

**Flow ใหม่ (แทน sosModal เดิม — และปิด design finding "แตะพลาดครั้งเดียว = แจ้งเหตุทันที"):**

1. **Stage ยืนยัน** — แตะการ์ด SOS → dialog ปุ่มใหญ่ (เต็มความกว้าง py-3.5+):
   - "เรียกรถพยาบาลทันที" (primary, var(--danger), icon ambulance/truck ที่มีใน lucide)
   - "ให้ทีมดูแลโทรกลับ" (secondary) → เนื้อหา/พฤติกรรมเดิมของ v0.8 ครบ (สรุปบัตรสุขภาพ + แจ้งผู้ดูแล)
   - "ยกเลิก" → ปิดเฉย ๆ
2. **Stage รถกำลังมา (dispatch tracker):**
   - หัว "รถพยาบาลกำลังมา" + วง pulse (reuse success-ring/celebrate)
   - **ETA เริ่ม 8 นาที ถอยหลัง ~5 วินาที/นาที** (direct-DOM timer แบบ call-timer — ไม่ re-render) + progress bar → จบที่ **"รถถึงหน้าบ้านแล้ว"** (หัวเปลี่ยน+เขียว+exhale)
   - ข้อมูลจำลอง: "รพ.คู่สัญญาเชียงใหม่ · รถ กข 1234 · พยาบาลวิชาชีพ 1 ท่าน"
   - ✓ ส่งบัตรสุขภาพฉุกเฉิน (ค่าจาก INC-J) ให้ทีมรถแล้ว · ✓ แจ้งผู้ดูแล (สมหญิง · บุตรสาว) แล้ว
   - ป้ายถาวร: "การจำลองสำหรับเดโม — ไม่มีการเรียกรถจริง"
   - ปุ่ม "ยกเลิกการเรียก (จำลอง)" + ปุ่มปิด
3. **State:** `modal:{kind:"sos", stage:"confirm"|"dispatch", ...}` — persist ผ่านกลไกเดิม → F5 กลาง dispatch กลับมาที่ tracker
4. **Presenter shortcut:** ArrowRight เร่ง ETA ลง 1 นาที เฉพาะตอน tracker เปิด (ขยาย listener เดิม — ไม่ชนคีย์ลัดคิวเพราะเดิม gate ด้วย !state.modal)
5. คงกติกาเดิม: เฉพาะ u3+ล็อกอิน, `overlayAllowed`, esc() ทุก field, elder zoom, timer หยุดสะอาดเมื่อปิด

**Non-goals:** ไม่มีแผนที่/GPS จริง · ไม่มีการโทร/ข้อความจริง · INC-J/K data คงเดิม · ไม่เพิ่ม persisted key ใหม่

**Acceptance Criteria:**
- AC-M-01: แตะ SOS ครั้งเดียว**ไม่** dispatch — เจอ stage ยืนยัน 3 ปุ่มใหญ่ · "ยกเลิก" ปิดโดย state ครบ
- AC-M-02: "เรียกรถพยาบาล" → tracker: ETA 8 นาทีถอยหลังจนถึง "รถถึงหน้าบ้านแล้ว" + progress + ข้อมูลรถ mock ครบ
- AC-M-03: tracker มี ✓ บัตรสุขภาพ (ค่าตรง INC-J) + ✓ ผู้ดูแล + ป้ายจำลองถาวร · ไม่มี a[href]/tel:/form/network ใหม่
- AC-M-04: "ให้ทีมดูแลโทรกลับ" → เนื้อหา/พฤติกรรม v0.8 เดิมครบ (regression)
- AC-M-05: ยกเลิกการเรียกได้ · ปิดแล้ว timer หยุด ไม่มี interval รั่ว (เปิด-ปิดหลายรอบไม่ซ้อน) · F5 ระหว่าง dispatch → กลับมาที่ tracker
- AC-M-06: elder mode + 1366×768 ไม่ล้น เลื่อนได้ · gating u3/ล็อกอินเดิม · ArrowRight เร่ง ETA เฉพาะ tracker เปิด ไม่รบกวนคีย์ลัดคิว

### 5.21 Round execution terms (INC-L + INC-M)

- **หนึ่งรอบ Codex** ลำดับ L (mechanical ก่อน) → M · diff เดียว `index.html` — **รอบใหญ่สุดที่ผ่านมา**: ถ้า Codex ประเมินว่าเสี่ยง แจ้งขอแบ่ง L ก่อน M ได้ตาม AGENTS.md §6
- Precondition: application baseline = `a948687` (SHA-1 `41323998…`, `index.html` สะอาด) · doc diff ของ SPEC/README/TODO ไม่ block
- Verification: AC 12 ข้อ (L-01…06, M-01…06) + มาตรฐานเดิม (syntax, console 0, offline, `git status` ไฟล์เดียว, ไม่แตะบริเวณ protected: celebration/INC-F/escaping/queue controls/notifications/privacy gate/INC-D/J/K) · **Completion Report ต้องมี inventory รายข้อ L 1–30: ทำ/ข้าม + เหตุผล** + cosmetic deviation ทุกจุด + hash ระบุ SHA-1
- Rollback: revert diff เดียว — ไม่กระทบ `a948687` และก่อนหน้า

### 5.22 INC-L+M Post-implementation Audit result (2026-07-19)

**ผล: PASS_WITH_RESIDUAL — 12/12 Acceptance Criteria** · ยืนยันสองชั้น: Claude ทดสอบ runtime เอง (SOS 3 stage, timer hygiene, ETA→arrival, F5, ค่าต้องห้าม=0, 1366×768+elder ไม่ล้น, console 0) + ผู้ตรวจอิสระ 4 ราย (inventory L 30 ข้อ / เจาะ SOS timer / regression+facts / refuter ขับ browser จริง) — verdict 3×PASS_WITH_RESIDUAL + 1×PASS

| หัวข้อ | ผลตรวจ |
|---|---|
| INC-L 30 ข้อ | ครบทุกข้อ · ค่าต้องห้าม #8A9080/#7A8271/#C0503F/text-gray-400/text-black = 0 · token --ink-soft/--danger เพิ่มแล้ว · contrast 8 คู่ที่แก้ ≥4.5:1 (คำนวณเอง) |
| INC-M | แตะครั้งเดียวไม่ dispatch (3 ปุ่มยืนยัน) · tracker ETA 8→arrival + ข้อมูลรถ mock + ✓บัตรสุขภาพ + ✓ผู้ดูแล + ป้ายจำลองถาวร · timer สะอาด (ไม่ leak/stack/negative, self-stop, F5 กลับ tracker) · โทรกลับ = v0.8 เดิม · ไม่มี tel:/network ใหม่ (`tel:` ใน lucide เป็น substring เดิม) |
| Regression | esc/persist/hydrate/clearPersisted/celebration/count-up + data INC-J/K byte-identical · healthCardModal/labResultsScreen เปลี่ยนเฉพาะ L7/L8/L27 ที่อนุมัติ · diff 406+/192−, hash `36fcb910` ตรงรายงาน |
| Docs | Completion Report ถูกต้องตามข้อเท็จจริง (diff stat, fingerprints, doc-diff plausibility) |

การตรวจรับ: Product Owner สั่ง "แก้ไขเลย" (2026-07-19) = ยอมรับ v0.9 + สั่งทำ INC-N ต่อ (DEC-017) · **การ commit v0.9 ยังค้าง ณ เวลาเขียน** — แนะนำ commit ก่อนรัน Codex INC-N

**Findings (LOW ทั้งหมด — ไม่บล็อกตรวจรับ, ถูกจัดเข้า INC-N ที่แก้ได้):**

#### PIA-009 — SOS overlay ค้างถ้ามีบัตรแต่ไม่มีผู้ดูแล (latent)
- Severity: LOW (ปัจจุบันแตะไม่ถึง) → HIGH หากเพิ่ม persona ที่มีบัตรแต่ไม่มีผู้ดูแล · State: CONFIRMED · Confidence: High
- Evidence: `sosModal()` (index.html:2117) `if (!profile || !caregiver) return "";` แต่ render gate (2337) เช็คแค่ `emergencyProfileFor()` → ถ้าคืน "" modalRoot ได้ `pointerEvents:"auto"` + innerHTML ว่าง = overlay ล่องหนล็อกทั้งจอ + ค้างข้าม F5 (persist)
- ปัจจุบัน: u3 เท่านั้นที่มี profile และมี caregiver ครบ จึงไม่เกิด — แต่เป็นกับดักถ้าเพิ่มข้อมูล
- Recommendation: แก้ที่ INC-N (§5.23 N-1)

#### PIA-010 — cosmetic/behavior deviation ไม่ถูก disclose ครบ (process)
- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: (ก) ข้อความการ์ด SOS "แตะเพื่อเรียกทีมดูแล"→"แตะเพื่อขอความช่วยเหลือ" (ถูกต้องตาม flow ใหม่แต่ไม่อยู่ใน L1-30/§6) · (ข) L24 ขยายให้เปลี่ยนวันเมื่อ slot เต็มด้วย (disclose ใน §2 แต่ §6 เขียน deviation=None)
- Observed: พฤติกรรมถูกต้อง/ดีขึ้น — เป็นช่องว่างการรายงาน
- Recommendation: บันทึกไว้เท่านั้น ไม่ต้องแก้โค้ด · รอบหน้า Codex ต้อง disclose ให้ครบ

#### PIA-011 — token สีตกค้าง 1 จุด (debt)
- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: `#5B6350` literal (index.html:1720, แท็บ lifestyle inactive) ยังไม่ใช้ `var(--ink-soft)` (ค่าเท่ากัน) · L4 กำหนดแค่กำจัด #8A9080/#7A8271 (=0 แล้ว)
- Recommendation: แก้ที่ INC-N (§5.23 N-3) — cosmetic ไม่กระทบสายตา

#### PIA-012 — fitPhone เขียน zoom:0 (pre-existing, ไม่ใช่ v0.9)
- Severity: LOW · State: CONFIRMED · Confidence: Medium
- Evidence: `fitPhone()` (index.html:2785-2786) `if (scale < 1) phone.style.zoom = scale;` — ถ้า `window.innerHeight` อ่านได้ 0 ชั่วขณะ zoom=0 (Chrome fallback เป็น 1 อยู่แล้ว จึงกระทบแค่ cosmetic) · โค้ดเดิม ไม่ได้แตะในรอบ v0.9
- Recommendation: แก้ที่ INC-N (§5.23 N-2) — clamp `scale > 0`

### 5.23 INC-N specification — Hardening mini-pack (แก้ finding จาก audit v0.9)

**Objective:** ปิด finding ที่แก้ได้จาก audit INC-L+M — งานเล็กมาก (~3-5 บรรทัด) ไม่เปลี่ยนพฤติกรรมที่ผู้ใช้เห็นในกรณีปกติ

**รายการ (3 fix):**

**N-1 (PIA-009) — กัน SOS overlay ค้าง:** ให้ `sosModal()` **ไม่คืน `""` เมื่อมี `emergencyProfileFor()`** — เปลี่ยนเงื่อนไข guard จาก `if (!profile || !caregiver) return "";` เป็น `if (!profile) return "";` แล้วทำให้ทุกบรรทัดที่พึ่ง `caregiver` แสดงแบบมีเงื่อนไข (`caregiver ? ... : ""`) เหมือนสไตล์ v0.8 · ครอบทั้ง 3 stage (confirm/callback/dispatch) — stage ใดที่อ้าง caregiver ต้อง null-safe · (ทางเลือกเทียบเท่า: เพิ่ม `&& caregiverFor()` ที่ render gate 2337 + timer gate 2341 + key gate 2767 — แต่ **แนะนำวิธีแรก** เพราะกันได้แม้ข้อมูลเปลี่ยน และไม่ทำให้ SOS หายทั้งปุ่ม)
- ยึด: esc() ทุก field เดิม · u3 ปัจจุบันแสดงผลเหมือนเดิมทุกประการ (มี caregiver อยู่แล้ว)

**N-2 (PIA-012) — fitPhone กัน zoom:0:** เปลี่ยน `if (scale < 1)` (index.html:2786) เป็น `if (scale > 0 && scale < 1)` — กันกรณี `innerHeight` เป็น 0 ชั่วขณะ

**N-3 (PIA-011) — token สีตกค้าง:** เปลี่ยน `#5B6350` (index.html:1720, สีตัวอักษรแท็บ lifestyle ที่ไม่ active) เป็น `var(--ink-soft)` · ค่าเท่ากัน ผลลัพธ์เหมือนเดิม

**Non-goals:** ไม่แตะ flow/copy/พฤติกรรม SOS อื่น (PIA-010 ปล่อยตามเดิม) · ไม่ refactor · ไม่เปลี่ยน state/schema

**Acceptance Criteria:**
- AC-N-01: จำลองผู้ใช้ที่มี `emergencyProfileFor()` แต่ `caregiverFor()` เป็น null (เช่นแก้ข้อมูลชั่วคราวตอนทดสอบ) → เปิด SOS ทุก stage แสดงผลได้ ไม่คืน overlay ว่าง ไม่ล็อกจอ · บรรทัดผู้ดูแลหายไปเฉย ๆ อย่างปลอดภัย
- AC-N-02: u3 ปกติ (มี caregiver) — SOS ทั้ง 3 stage เหมือน v0.9 เป๊ะ (confirm 3 ปุ่ม, callback, dispatch tracker + "แจ้งผู้ดูแล (สมหญิง)")
- AC-N-03: fitPhone — ที่ viewport ปกติ/1366×768 ย่อเฟรมเหมือนเดิม; ไม่มีทางตั้ง `zoom:0` (ตรวจว่า guard ครอบ)
- AC-N-04: ไม่เหลือ `#5B6350` literal นอก `:root` (เหลือแค่บรรทัดนิยาม token) · แท็บ lifestyle แสดงเหมือนเดิม
- AC-N-05: Guard — console 0 error, offline เดิม, ไม่มี horizontal overflow, ไม่แตะบริเวณ protected/พฤติกรรม SOS อื่น · diff เล็ก (~5 บรรทัด)

**Round terms:** Codex รอบเดียว · baseline application state = `index.html` SHA-1 `36fcb910…` (v0.9 applied) · **แนะนำ commit v0.9 ก่อน** เพื่อ diff สะอาด — ถ้าไม่ ให้ preserve v0.9 diff เป็น pre-existing work (AGENTS.md §7) และแยก ~5 บรรทัด INC-N ใน Completion Report · Rollback: revert เฉพาะ ~5 บรรทัด

### 5.24 INC-N closure (landed 2026-07-19)

INC-N (hardening mini-pack §5.23) ถูก implement โดย Codex และ **commit รวมกับ INC-L+M ใน `3a41dfc`** (ผู้ใช้ commit ผ่าน GitHub Desktop หลายครั้งด้วยข้อความเดียวกัน) — ไม่ได้แยก commit ตามที่แนะนำ แต่ผลถูกต้อง

**Claude static verification (2026-07-19) ต่อ `HEAD:index.html`:**

| Fix | ผลตรวจ |
|---|---|
| N-1 (PIA-009 กัน SOS overlay ค้าง) | ✅ pattern บั๊ก `if (!profile \|\| !caregiver) return ""` = 0 · pattern แก้ `if (!profile) return ""` = 1 |
| N-2 (PIA-012 fitPhone clamp) | ✅ `scale > 0 && scale < 1` = 1 |
| N-3 (PIA-011 token สี) | ✅ `#5B6350` เหลือ 1 (บรรทัดนิยาม `--ink-soft` ใน `:root` เท่านั้น — literal ตกค้างถูก tokenize แล้ว) |

สรุป: **INC-N landed ครบ 3 fix ถูกต้อง** · ข้อสังเกต process: Codex ควร commit แยกตาม scope และส่ง Completion Report ของ INC-N (ไม่ได้ส่งรอบนี้เพราะ fold เข้า INC-L+M) — บันทึกเป็น debt เชิงกระบวนการ ไม่กระทบโค้ด (DEC-018)

### 5.25 INC-O specification — SOP live tracker + หน้าสถานะการบริการ (ปรับขยาย 2026-07-19)

**Objective:** ต่อยอด SOS ambulance tracker (INC-M) เป็นระบบ **ติดตามสถานะสด + SOP workflow แบบแกร็บ** ครอบบริการถึงบ้าน + หน้า **"สถานะการบริการ" แบบเสร็จสมบูรณ์** (timeline ทุกขั้น "สำเร็จ" + เวลา + สรุปยอด + ปุ่มชำระส่วนที่เหลือ) ตามภาพอ้างอิงที่ Product Owner ให้ · **จำลอง 100%** (ASM-002)

**ที่มา:** design exploration 4 มุมมอง (§5.25 เดิม) + ภาพอ้างอิง "สถานะการบริการ" + DEC-019/DEC-020 (Product Owner เลือกขยายบริการ + รูปแบบเงิน "มัดจำ + จ่ายหน้างาน")

**ขอบเขตบริการ (ปรับขยายจาก Option B):**
- **เจาะเลือดถึงบ้าน** (`kind:"lab"`) — home visit
- **ส่งยาต่อเนื่อง** (`kind:"meds"`) — delivery
- **บริการไลฟ์สไตล์: แม่บ้าน / นวด / Ice bath / กายภาพบำบัด / Pet care** (`kind:"lifestyle"`, ใช้ `category` แยกชนิด) — home visit
- **ไม่รวม** ปรึกษาหมอ (มีคิว+VDO อยู่แล้ว — ไม่ใช่ booking)

**SOP steps (แต่ละขั้นมี label + คำอธิบาย + เวลา preset แบบสมจริง เพื่อให้หน้า "สำเร็จ" อ่านเหมือนตารางจริงในภาพ):**

*เจาะเลือดถึงบ้าน / ไลฟ์สไตล์ (home visit) — 5 ขั้น:*
1. ผู้ให้บริการรับงาน (เผยการ์ดผู้ให้บริการ)
2. กำลังเดินทางมาหาคุณ — **ETA** (นับถอยหลัง + progress)
3. ถึงบ้านแล้ว
4. กำลังให้บริการ (เจาะเลือด / ทำความสะอาด / นวด / ดูแลสัตว์ ฯลฯ ตามชนิด)
5. เสร็จสิ้น *(terminal)* — เจาะเลือดมี CTA **"ดูผลตรวจของฉัน"** → `data-nav="lab-results"`

*ส่งยาต่อเนื่อง (delivery) — 4 ขั้น:*
1. เภสัชกรจัดยา
2. ไรเดอร์รับพัสดุ (เผยการ์ดไรเดอร์)
3. กำลังจัดส่ง — **ETA**
4. ส่งถึงแล้ว *(terminal)*

**หน้า "สถานะการบริการ" (ตามภาพ):** timeline แนวตั้ง — ขั้นที่ผ่าน = ไอคอน + ชื่อ + คำอธิบาย + **"สำเร็จ" (เขียว) + เวลา `HH:MM น.`** · ขั้นปัจจุบัน = ไฮไลต์ + ETA/หมุน · ขั้นถัดไป = จาง/รอดำเนินการ · ล่างสุด = การ์ดสรุปยอด (ยอดรวมทั้งสิ้น / ยอดที่ชำระแล้ว / ยอดคงเหลือ) + ปุ่ม **"ชำระเงิน"** (เต็มความกว้าง) · เมื่อจบครบทุกขั้น = ทุกบรรทัด "สำเร็จ" เหมือนภาพ

**รูปแบบเงิน — มัดจำ + จ่ายหน้างาน (DEC-020):**
- booking แต่ละรายการมี `total` (ราคาเต็มหลังส่วนลดลูกบ้าน), `deposit` (จ่ายตอนจอง), `remaining` (= total − deposit)
- **บริการถึงบ้าน (lab + lifestyle):** ตอนจอง จ่าย **มัดจำ ~30%** (`deposit = round(total*0.3)`); หน้า tracker แสดง "คงเหลือชำระหน้างาน" = `remaining`; เมื่อถึงขั้นสุดท้าย ปุ่ม **"ชำระเงิน"** เปิด payment sheet จ่าย `remaining` (จำลอง + celebration เดิม) → เปลี่ยนเป็น "ชำระครบแล้ว ✓"
- **ส่งยา (meds):** เป็นการซื้อของ — **จ่ายเต็มตอนจอง** (`deposit = total`, `remaining = 0`); หน้าสถานะแสดง "ชำระครบแล้ว ✓" ไม่มีปุ่มค้าง
- **หมายเหตุ scope:** รูปแบบมัดจำ **แตะโซน payment** (payment sheet + booking amount) ที่เคยเป็น protected — รอบนี้ payment-confirm/paymentModal **อยู่ใน scope INC-O** และต้อง re-verify (AC-O-11) · ยอดต้องบวกกันลงตัว (`deposit + remaining = total`) ห้ามคิดเงินซ้ำ

**Design invariants (บังคับ — กัน regression, จากผลสำรวจ):**
1. Tracker/status = **modal kind ใหม่ `tracker`** · timer แบบ SOS (**direct-DOM + persist ทุก tick, ห้าม setState per tick**), single handle `trackTimer`, start/stop ใน `render()` หลัง DOM เขียน, `updateTrackerDom()` null-check, self-stop, ไม่ stack (คัดจาก `startSosTimer` 864-883)
2. Gate = `state.loggedIn` + booking รองรับ track · **ไม่ใช่ `emergencyProfileFor()`** (u3-only) · **ห้ามคืน `""` ขณะ gate จริง** (บทเรียน PIA-009 — guard builder+gate เงื่อนไขเดียว, data ว่าง = placeholder)
3. เก็บ `b.track = { step, eta }` บน booking **key ด้วย `ref`** (ไม่ใช่ index — กัน drift) · persist ผ่าน sessionStorage เดิม F5-safe · booking เก่าไม่มี track → "ยืนยันแล้ว" ปกติ ไม่ error
4. `kind`/`category` discriminator เลือก SOP template · เก็บ provider id บน booking → avatar ผ่าน `staffAvatar()` (เพิ่ม mock พยาบาล/ไรเดอร์/ช่างบริการ)
5. **`modalKey` คงที่ข้าม step** (ห้าม fold step — ไม่งั้น replay animation)
6. ArrowRight: branch เดียวใน keydown listener เดิม, gate `modal.kind==="tracker"`, เลื่อน step, `preventDefault()+return` ก่อน queue branch (ไม่ชนคิว/SOS)
7. เวลา timestamp: preset realistic schedule ต่อ SOP (ไม่ใช่ wall-clock จริงตอนกดเร็ว) เพื่อให้หน้า "สำเร็จ" อ่านเหมือนภาพ
8. reduced-motion: เพิ่ม id progress ใหม่เข้า media block (405-411) · elder + 1366×768: dialog `max-height`+`overflow-y:auto` (timeline ยาว)
9. **จำลอง 100%**: ไม่มีแผนที่/GPS/`tel:`/network/form ใหม่ · ป้าย "จำลอง" ถาวร · `esc()` ทุก dynamic field
10. timer **foreground-only** · home chip = last-known step
11. payment ส่วนที่เหลือ: **reuse** payment sheet + celebration เดิม (ไม่สร้าง flow ใหม่) — แค่ส่ง amount = `remaining`; อัปเดต booking เป็นชำระครบ ห้ามแตะ booking อื่น

**Home card chip:** นัด track active → แสดง label สถานะปัจจุบัน (แทน "ยืนยันแล้ว") + progress เส้นเล็ก · ไม่มี track → เดิม

**Non-goals (เก็บ Option C เต็ม/รอบหน้า):** ไม่ทำ consult tracker · ไม่มีแผนที่/route จริง/แชท/tip/rating · ไม่เดินสถานะเบื้องหลัง · ไม่มีแจ้งเตือน step-change (คู่ INC-E ทีหลัง) · ไม่มีสถิติผู้ประกอบการ (INC-H) · ไม่เพิ่มบริการใหม่ที่แอปยังไม่มี (เช่น รับ-ส่งผู้สูงอายุในภาพ — เป็น style reference; เพิ่มได้ถ้า PO สั่งภายหลัง) · ไม่เพิ่ม persisted key ใหม่

**Acceptance Criteria:**
- AC-O-01: นัด lab/meds/lifestyle ที่ยืนยันแล้ว มีปุ่ม "ติดตามสถานะ/สถานะการบริการ" (home card + detail sheet) · แตะ → เปิด tracker modal
- AC-O-02: tracker home-visit (lab/lifestyle) แสดง SOP 5 ขั้นถูกลำดับ + การ์ดผู้ให้บริการ + ETA ขั้นเดินทาง + progress · lab ขั้นจบมี CTA เด้ง "ผลตรวจของฉัน"
- AC-O-03: tracker meds แสดง SOP 4 ขั้น + การ์ดไรเดอร์ + ETA ขั้นจัดส่ง · ขั้นจบ "ส่งถึงแล้ว"
- AC-O-04: ทั้ง 5 บริการไลฟ์สไตล์ (แม่บ้าน/นวด/ice bath/กายภาพ/petcare) เข้า tracker ได้ ขั้น 4 ปรับ label ตามชนิดบริการ
- AC-O-05: หน้าสถานะแบบเสร็จ — เมื่อจบครบทุกขั้น ทุกบรรทัดขึ้น "สำเร็จ" + เวลา `HH:MM น.` (timeline เหมือนภาพ)
- AC-O-06: ETA เดินถอยหลังจริง (direct-DOM) · ArrowRight เลื่อนขั้นเฉพาะตอน tracker เปิด · ไม่รบกวนคีย์ลัดคิว/SOS
- AC-O-07: timer สะอาด — เปิด/ปิด ×5 ไม่ leak/stack, ETA ไม่ติดลบ, self-stop เมื่อปิด/logout/สลับผู้ใช้/จบ · F5 กลาง tracker กลับมาต่อขั้นเดิม
- AC-O-08: home chip แสดงสถานะสด (last-known) เมื่อ active · booking เก่าไม่มี track = "ยืนยันแล้ว" ไม่ error หลัง F5
- AC-O-09: gate ถูก — ทุกผู้ใช้ที่ล็อกอิน (ไม่ใช่แค่ u3) · ไม่มี empty-overlay lock · ก่อนล็อกอินไม่โผล่
- AC-O-10: จำลอง — ไม่มี `a[href]`/`tel:`/form/network/map ใหม่ · ป้าย "จำลอง" ถาวร · `esc()` ทุก field
- AC-O-11: **รูปแบบเงิน** — home-visit จ่ายมัดจำ ~30% ตอนจอง, tracker แสดงคงเหลือ, ปุ่ม "ชำระเงิน" จ่ายส่วนที่เหลือ (จำลอง+celebration) แล้วเป็น "ชำระครบแล้ว ✓" · meds จ่ายเต็ม แสดง "ชำระครบแล้ว" · ยอด `deposit + remaining = total` ทุกกรณี ไม่คิดเงินซ้ำ · F5 ระหว่างค้างจ่าย/จ่ายแล้ว สถานะเงินคงถูกต้อง
- AC-O-12: Guard — `modalKey` คงที่ (ไม่ replay), reduced-motion ครอบ progress ใหม่, elder+1366×768 timeline เลื่อนได้ไม่ล้น, console 0, offline เดิม, ไม่แตะ protected นอกเหนือ payment ที่อยู่ใน scope (celebration/queue/SOS/call timers, escaping, privacy gate, INC-J/K, storage recovery ต้องคงเดิม)

**Round terms:** ขนาด **L** (ขยายจาก Option B: +3 บริการไลฟ์สไตล์ +completed timeline +deposit/payment model) — **Codex อาจแจ้งขอแบ่งเป็น O-1 (tracker+บริการ+หน้าสถานะ) → O-2 (มัดจำ/จ่ายหน้างาน) ตาม AGENTS.md §6** ถ้าประเมินว่าเสี่ยง · baseline `3a41dfc` (SHA-1 `3974d870`) · Verification: AC 12 ข้อ + มาตรฐานเดิม + re-verify payment · Completion Report: inventory + cosmetic + hash SHA-1 · Rollback: revert diff เดียว

### 5.26 INC-O Post-implementation Audit result (2026-07-24)

**ผล: PASS_WITH_RESIDUAL — 12/12 Acceptance Criteria** · ยืนยันสองชั้น: Claude ทดสอบ runtime เองครบวง (money flow มัดจำ→จ่ายหน้างาน, tracker, F5, gate, PIA-009, 1366×768, protected byte-identical) + ผู้ตรวจอิสระ 4 ราย (payment / timer-motion / scope-regression / refuter ขับ browser 7 การโจมตี) — verdict PASS_WITH_RESIDUAL + PASS×3

| หัวข้อ | ผลตรวจ |
|---|---|
| เงินมัดจำ/จ่ายหน้างาน | แม่นยำทุกบาท — `deposit=round(total*0.3)`, `remaining=total-deposit`, บวกกัน=total (brute-force 0–300k ไม่มี drift) · จ่ายส่วนที่เหลือ mutate booking เดียวด้วย ref ไม่สร้างซ้ำ ไม่คิดเงินซ้ำ (idempotent guard) · meds จ่ายเต็ม · resident discount ใช้ครั้งเดียวก่อนแบ่ง · F5 คงถูกต้อง |
| Tracker/SOP | lab/lifestyle 5 ขั้น, meds 4 ขั้น, ไลฟ์สไตล์ 5 บริการ label ครบ · หน้า "สถานะการบริการ" timeline "สำเร็จ"+เวลา ตรงภาพอ้างอิง |
| Timer hygiene | single handle, direct-DOM+persist (ไม่ setState/tick), self-stop, ไม่ stack, ETA ไม่ติดลบ, modalKey คงที่ (ไม่ replay), timers mutually exclusive |
| Gate/security | เจ้าของ (`userId===currentUser`) ไม่ใช่ u3-only · ไม่มี PIA-009 lock · esc() ครบ (XSS pet name ปลอดภัย) · จำลอง 100% ไม่มี map/tel:/network |
| Regression | โค้ดโปรเทกต์ byte-identical (esc/persist/hydrate/queue+SOS/call timers/celebration/privacy gate/INC-J/K) · แตะเฉพาะ payment (ใน scope) + reduced-motion block (เพิ่ม id ใหม่) · diff 456+/58−, hash `6d225e06` ตรง |

การตรวจรับ: Product Owner commit `6f1e5d3` + push และสั่ง "ปิด v0.11 ได้เลย" 2026-07-24

**Findings (LOW ทั้งหมด — ไม่บล็อก):**

#### PIA-013 — หน้ารายละเอียดนัดส่งยาโชว์แถว "มัดจำ" ทั้งที่จ่ายเต็ม (cosmetic)
- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: `bookingDetailModal` ส่งยา (meds: deposit=total, remaining=0) ใช้ layout เดียวกับบริการมัดจำ → พิมพ์แถว "มัดจำ ฿[เต็มจำนวน]" · Codex เปิดเผยใน Completion Report §8
- Impact: ป้ายกำกับแปลกเท่านั้น — badge/tracker/ยอดถูกหมด (paid=total, outstanding=0, "ชำระครบแล้ว ✓")
- Recommendation: polish เล็ก — `kind==='meds'` (หรือ remaining===0) โชว์ "ชำระแล้ว" บรรทัดเดียว · เสนอพ่วง v0.12

#### PIA-014 — payment timeout race fix เป็น hardening นอกข้อกำหนดมัดจำ (process, in-scope)
- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: Codex ผูก callback ของ payment delay กับ payment object ที่ถูกต้อง (`m !== pendingPayment` guard) · แตะ payment-confirm
- Observed: อยู่ใน scope (§5.25 เปิด payment เข้า INC-O, AC-O-11) · ตรวจแล้วไม่ regression (happy path จ่ายผ่านปกติ)
- Recommendation: รับทราบ ไม่ต้องแก้

#### PIA-015 — trackerModal มี fallback "ไม่พบสถานะบริการนี้" ที่เข้าไม่ถึง (dead code, harmless)
- Severity: LOW · State: CONFIRMED · Confidence: High
- Evidence: render เรียก `trackerModal()` เฉพาะเมื่อ `trackerAllowed` จริง; กรณี false ตกไป else ที่ตั้ง pointerEvents:none
- Recommendation: ไม่ต้องแก้ (belt-and-suspenders กัน PIA-009)

**ข้อสังเกต (pre-existing, ไม่ใช่ INC-O):** `state.bookings` เป็น array รวม ไม่ filter รายคนบนหน้าแรก — นัดของ u1 โผล่ให้ u3 เห็น (แต่ track ไม่ได้) · เป็นของเดิมก่อน INC-O (AUD-006, login เป็น presentation control) · INC-O gate ตัว tracker ถูกต้องแล้ว

### 5.27 INC-P specification — สมุดสุขภาพ (Health Record Hub) + กราฟค่าสุขภาพ (Option A MVP, read-only)

**Objective:** เพิ่มหน้า **"สมุดสุขภาพ"** หน้าเดียวที่รวมข้อมูลสุขภาพของสมาชิก (บัตรสุขภาพ INC-J + ผลตรวจ INC-F + ยา) ไว้ที่เดียว **พร้อมกราฟแนวโน้มค่าสุขภาพ 3 ตัว** (ความดัน / น้ำตาลในเลือด / น้ำหนัก) วาดด้วย inline SVG เอง (ออฟไลน์ ไม่มีไลบรารีกราฟ) — เปลี่ยนภาพเป็น **health companion** · **อ่านอย่างเดียว, ข้อมูลจำลอง 100%** (ASM-002)

**ที่มา:** grounded design 4 มุมมอง 2026-07-24 (data-map / chart-tech / hub-UX / risk-scope) · ผู้ใช้เลือกทิศ health companion (DEC-022) · Option A MVP (เสี่ยงต่ำ, additive เกือบล้วน)

**ขอบเขต (Option A MVP):** หน้า `health-record` อ่านอย่างเดียว + กราฟ sparkline 3 ตัว + ลิงก์ไปฟีเจอร์เดิม (ผลแล็บ/ยา/บัตรสุขภาพ) + การ์ดเข้าจากหน้าแรก + empty state · **ไม่มีฟอร์ม "บันทึกค่าใหม่"** (defer)

**Data ใหม่ (synthetic `vitalsByUser` วางข้าง `labResultsByUser` ~1686):**
- shape `vitalsByUser[uid] = { bp:[{daysAgo,sys,dia}], sugar:[{daysAgo,value}], weight:[{daysAgo,value}] }` + band `{lo,hi}` numeric ต่อ vital
- **u3 = พระเอก** (ความดันสูง+เบาหวาน ตาม emergencyProfile): BP ~158/96 → ก้ำกึ่ง (มีจุดควรติดตาม), น้ำตาลสูง→ก้ำกึ่ง, น้ำหนักคงที่ · สอดคล้อง lab เดิม (FBS 96, LDL 142 follow)
- u1 = ส่วนใหญ่ปกติ, น้ำตาลล่าสุด ~118 ล้อ lab FBS 118 · u2/u4 = ไม่มี → empty state · reuse `labResultDate()` ทำแกน x · มี disclaimer จำลอง

**SVG sparkline helper (invariants บังคับ):**
1. inline SVG ใน template string แบบ `QR_SVG` (~738) inject ผ่าน render เดิม
2. **`viewBox` + `style="width:100%;height:auto;display:block"` — ห้าม fixed px** (รอด elder zoom 1.15 + fitPhone)
3. **สีใส่ `style` attribute เท่านั้น** — ห้าม presentation attr (`stroke="var(--x)"` fail เงียบ)
4. **ห้าม `crispEdges`** (เส้นทแยงหยัก) · สี: เส้น `--primary`, จุดนอกช่วง `--clay`, band/แกน `--line`/`--slate`
5. guard คณิต: `Number()` coerce · `span=(dMax-dMin)||1` · จุดเดียว/ค่าเท่ากัน → วางกลาง (กัน NaN)
6. **ตัวเลข/วันที่/label เป็น HTML sibling นอก SVG** (ไม่ใช้ `<text>` — กัน font ไทยเพี้ยน + esc + elder scale)
7. **STATIC ไม่มี draw animation** (กัน motion regression) — ถ้า animate ต้อง scope `.screen-enter` + เพิ่ม reduced-motion block + fallback `stroke-dashoffset:0`
8. accessibility: `role="img"` + `aria-label` (esc) + **caption ไทยมองเห็น** (ค่าล่าสุด + ทิศทาง + จำนวนจุดควรติดตาม)

**โครงหน้า (reuse ของเดิม):** (1) screenHero "สมุดสุขภาพ" icon heart-pulse เขียว · (2) การ์ดตัวตน (มี emergencyProfile) กรุ๊ปเลือด/แพ้ยา/โรคประจำตัว + disclaimer · (3) กราฟ 3 การ์ด: ความดัน(mmHg sys/dia)/น้ำตาล(mg/dL)/น้ำหนัก(กก.) — sparkline + ค่าล่าสุด + ชิปสถานะ + delta ▲/▼ (unicode muted) + วันที่ · (4) สรุปผลแล็บ + ปุ่ม "ดูผลตรวจทั้งหมด" `data-nav="lab-results"` · (5) สรุปยา + ปุ่ม "ดูยาต่อเนื่อง" `data-nav="meds"` · (6) CTA "ปรึกษาหมอต่อ" เมื่อมี ≥1 ควรติดตาม · (7) empty state (u2/u4)

**Thresholds/chips (reuse ปกติ/ควรติดตาม):** BP ควรติดตาม sys≥140 หรือ dia≥90 · FBS ควรติดตาม ≥126 (ก้ำกึ่ง 100–125) · น้ำหนัก = label กลาง (คงที่/ลดลง/เพิ่มขึ้น)

**Entry:** การ์ด full-width "สมุดสุขภาพ" หน้าแรก `data-nav="health-record"` (auto-wired, 0 JS ใหม่) — **ไม่แตะ bottom nav / ไม่ขยาย grid บริการ** · ไม่วางใน block เฉพาะ u3

**Wiring (แก้น้อยมาก):** `DEPTH['health-record']=1` (บังคับ กัน slide ผิดทิศ) · render switch +1 else-if · การ์ดเข้า 1 อัน · back default กลับ home (ไม่ต้องแก้)

**Non-goals:** ไม่มีฟอร์มบันทึกค่า (persistence — defer) · ไม่มี interactive tooltip (Option B) · ไม่มี persisted key ใหม่ · ไม่เพิ่ม nav tab · ไม่แตะ timer/payment/tracker/celebration/privacy gate · PIA-013 แยกต่างหาก · ไม่เกี่ยว INC-H

**Acceptance Criteria:**
- AC-P-01: การ์ด "สมุดสุขภาพ" หน้าแรก (ทุกผู้ใช้ล็อกอิน) → แตะไปหน้า `health-record` · ก่อนล็อกอินเข้าไม่ได้ (privacy gate เดิม)
- AC-P-02: กราฟ 3 ตัว เป็น inline SVG · สีเส้น/จุด resolve var() ได้ (ไม่ดำ/หาย) · จุดนอกช่วง `--clay`
- AC-P-03: แต่ละกราฟมีค่าล่าสุด + ชิปสถานะ + วันที่ + caption ไทย · role="img" + aria-label
- AC-P-04: u3 ข้อมูลครบสอดคล้อง (มีจุดควรติดตาม, ตรง lab/โรคประจำตัว) · u1 ส่วนใหญ่ปกติ · **u2/u4 empty state** ไม่มีกราฟเปล่า/NaN
- AC-P-05: สรุปแล็บ+ยา+ลิงก์ทำงาน · การ์ดตัวตน u3 โชว์บัตรสุขภาพย่อ · CTA ปรึกษาหมอต่อเมื่อมีค่าควรติดตาม
- AC-P-06: SVG width:100%+viewBox ไม่มี fixed px · **ไม่มี horizontal overflow** ปกติ/1366×768/**elder (u3)** · เลื่อนครบ
- AC-P-07: กราฟ static หรือ draw-in ต้อง scope `.screen-enter` + ปิดใต้ reduced-motion (fallback เต็มเส้น) · re-render สลับสมาชิกไม่ replay ผี
- AC-P-08: `esc()` ทุก dynamic string · guard คณิต (ไม่ NaN) · มี disclaimer จำลอง · ไม่มี network/external ref ใหม่
- AC-P-09: Guard — DEPTH ถูก (slide/back ถูก) · console 0 · ไม่แตะ protected (timer/payment/tracker/celebration/escaping/privacy gate/INC-J/K/storage) · ไม่มี persisted key ใหม่

**Round terms:** ขนาด **S–M** (additive: const + ฟังก์ชันหน้า + SVG helper + 3 บรรทัด wiring) · baseline `6f1e5d3` (SHA-1 `6d225e06`) · Verification: AC 9 + มาตรฐานเดิม · Completion Report: inventory + cosmetic + hash SHA-1 · Rollback: revert diff เดียว

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

สถานะปัจจุบัน (หลังปิด batch, 2026-07-19 — ตาราง Phase 0 เดิมถูกแทนที่เพราะหมดสภาพ):

| File | สถานะ | Notes |
|---|---|---|
| `index.html` | แก้ได้เฉพาะใน increment ที่อนุมัติ | ล่าสุด: batch INC-A+E+B (`2a8bd34`) · ไม่มี scope ค้าง |
| `SPEC.md` | Claude ปรับปรุงตามรอบ planning/audit | v0.5 = ปิดรอบ batch |
| `CLAUDE.md` / `AGENTS.md` | คงที่ | เอกสาร governance — แก้เมื่อผู้ใช้ขอเท่านั้น |
| `TODO.md` | อัปเดตติ๊กงานเสร็จตามรอบ audit | backlog ไม่ใช่ authorization |
| `README.md` | Read only | มี doc drift เล็กน้อย (ฟีเจอร์ใหม่ยังไม่ถูกกล่าวถึง) — รอผู้ใช้สั่ง |
| `COMPLETION_REPORT.md` | Codex เขียนต่อรอบ implementation | เพิ่มครั้งแรกในรอบ batch (`418ef45`) ตาม PIA-002 |

Increment ถัดไป: Affected files ต้องระบุในสเปก §5.x ของ increment นั้นก่อนขออนุมัติ

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
| AUD-001 | MEDIUM | **RESOLVED** (INC-B, `2a8bd34`) | Personal/health-like demo content is available before login — ปิดโดย privacy gate (§5.8) |
| AUD-002 | MEDIUM | **RESOLVED** (INC-C, `ca68ddb`) | User-controlled input reaches `innerHTML` without escaping — ปิดโดย `esc()` render-time (§5.13) |
| AUD-003 | MEDIUM | CONFIRMED | Monolithic file and absence of automated tests raise regression cost |
| AUD-004 | MEDIUM | **RESOLVED** (INC-A, `2a8bd34`) | Presenter cannot control/re-enter the timer-driven queue — ปิดโดยแบนเนอร์+คีย์ลัด (§5.8) |
| AUD-005 | MEDIUM | **RESOLVED** (INC-0, `2bbb500`) | Significant uncommitted app work exists before workflow setup — ผู้ใช้ commit เป็น baseline แล้ว |
| AUD-006 | MEDIUM | CONFIRMED LIMITATION | Authentication, payment and VDO are simulation-only |
| AUD-007 | LOW | **RESOLVED** (INC-D, `a948687`) | Persistence failures are silently ignored — hydrate เน่า/quota กู้สะอาดแล้ว (§5.18) |
| AUD-008 | LOW | CONFIRMED / NEEDS_VERIFICATION | Login labels are placeholder-only; modal focus behavior still needs manual audit |
| PIA-001 | LOW | OPEN (candidate) | หน้า lab-results ไม่มีแท็บ active ของตัวเองใน bottom nav (§5.3.1) — polish เล็ก รอผู้ใช้เลือก |
| PIA-002 | LOW | **RESOLVED** (`418ef45`) | ขอให้ Codex ส่ง Completion Report เป็นไฟล์ใน repo — ทำแล้วในรอบ batch |
| PIA-003 | LOW | CONFIRMED (process) | Cosmetic deviation ใน `userBar()` ไม่ถูกรายงานใน Completion Report (§5.8) |
| PIA-004 | LOW | CONFIRMED (observation) | Queue state คงอยู่ข้าม logout โดยซ่อนสนิท — expected behavior (§5.8) |

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
- **Resolution (2026-07-19):** ปิดโดย INC-B (`loginGateScreen()` + generic home + overlay/timer gating) — commit `2a8bd34`, verified §5.8 (AC-B-01…04)

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
- **Resolution (2026-07-19):** ปิดโดย INC-A (แบนเนอร์กลับเข้าคิว + คีย์ลัดลูกศรเฉพาะหน้าคิว ไม่มี UI บอก) — commit `2a8bd34`, verified §5.8 (AC-A-01…05)

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

### 15.2 Implementation verification per increment

Verification plan เป็นส่วนหนึ่งของสเปกแต่ละ increment (ดู §5.3 และ §5.4–§5.7 เป็นแบบ) และผลตรวจถูกบันทึกเป็น audit log ต่อรอบ: §5.3.1 (INC-F), §5.8 (batch INC-A+E+B) · increment ถัดไปต้องมี verification plan ของตัวเองใน §5.x ก่อนขออนุมัติ · default target (ตาม BQ-004 ที่ resolved): Chrome ล่าสุดบน macOS, เปิด file:// offline, viewport ปกติ + โปรเจกเตอร์ 1366×768

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

### Application increments — executed

- INC-F: AC-F-01…07 — PASS ทั้งหมด (log ใน §5.3.1)
- Batch INC-A+E+B: AC-A-01…05, AC-E-01…04, AC-B-01…04 — PASS 13/13 (log ใน §5.8)

### Next application increment

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
- BQ-004 — RESOLVED (2026-07-19): default verification target (Chrome ล่าสุดบน macOS, file:// offline, viewport ปกติ + 1366×768) ถูกยอมรับผ่านการอนุมัติ v0.3 และใช้ต่อเนื่องในรอบ batch v0.4 โดยไม่มีข้อทักท้วง

### Blocking Questions

**ไม่มี** — ทุกรอบที่อนุมัติไปแล้ว (INC-F, batch INC-A+E+B) ปิดครบและตรวจรับแล้ว

### Non-blocking / strategic

- BQ-003 — ทิศทาง demo vs ระบบจริง: ยังเปิดอยู่ ไม่ block INC-F (INC-F เป็น mock feature สอดคล้อง ASM-001)
- BQ-005 — ลำดับ privacy vs presenter reliability สำหรับรอบถัด ๆ ไป: ยังเปิดอยู่

### Non-blocking until a related scope is selected

- OQ-001 — Is adding a dependency-free automated smoke check desirable, or should verification remain manual to preserve the zero-tooling repository?
- OQ-002 — Should accessibility become an explicit acceptance target in the next increment?
- OQ-003 — Should `.claude/settings.local.json` permissions be reviewed/cleaned by the user separately, given that the file is local and ignored?
- OQ-004 — RESOLVED (2026-07-19): ผู้ใช้ commit เอกสาร workflow เองแล้ว (`51d30d9` และ commit เอกสารรอบถัดมา) — audit trail อยู่ใน git ครบ

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
| DEC-008 | 2026-07-19 | INC-F audit = PASS; Product Owner ตรวจรับโดย commit `22f413c` + push | Product Owner |
| DEC-009 | 2026-07-19 | เลือก batch ถัดไป: INC-A + INC-E + INC-B ทำในหนึ่งรอบ implementation (ยกเว้นกติกา "ครั้งละหนึ่ง increment" โดยเจ้าของโปรเจกต์เอง) | Product Owner |
| DEC-010 | 2026-07-19 | Batch INC-A+E+B audit = PASS (13/13 AC); Product Owner ตรวจรับด้วย commit `2a8bd34` + `418ef45` + push และยืนยัน "approve" ในแชท | Product Owner |
| DEC-011 | 2026-07-19 | เลือกรอบถัดไป: INC-C + INC-G + PIA-001 (Codex batch เดียว) + อัปเดต README (Claude, งานเอกสาร หลัง audit PASS) — สเปก §5.9–§5.12 | Product Owner |
| DEC-012 | 2026-07-19 | Batch INC-C+G+PIA-001 audit = PASS (12/12 AC, ยืนยันด้วยผู้ตรวจอิสระ 4 ราย); Product Owner ตรวจรับด้วย commit `ca68ddb` + push และยืนยัน "approve" ในแชท | Product Owner |
| DEC-013 | 2026-07-19 | Claude อัปเดต README ครอบคลุมฟีเจอร์ใหม่ทั้งหมด (ผลแล็บ, กระดิ่ง, privacy gate, ตัวควบคุมคิว + คีย์ลัด, โหมดผู้สูงอายุ + SOS, escaping) ตามสเปก §5.12 หลัง audit PASS | Product Owner / Claude |
| DEC-014 | 2026-07-19 | เลือกรอบถัดไป (แนวสุขภาพ): INC-D (storage recovery) + INC-J (บัตรสุขภาพฉุกเฉิน) + INC-K (ผู้ดูแล/แจ้งลูกหลาน) — Codex batch เดียว, สเปก §5.14–§5.17 | Product Owner |
| DEC-015 | 2026-07-19 | Batch INC-D+J+K audit = PASS (13/13 AC); Product Owner ตรวจรับด้วย commit `a948687` + push และยืนยัน "เรียบร้อย" ในแชท | Product Owner |
| DEC-016 | 2026-07-19 | เลือก + อนุมัติรอบ design: INC-L (design polish pack จาก audit 4 มุมมอง) + INC-M (SOS เรียกรถพยาบาล — จำลอง 100%, ออกแบบ 2 จังหวะกันแตะพลาด) — สเปก §5.19–§5.21 | Product Owner |
| DEC-017 | 2026-07-19 | INC-L+M audit = PASS_WITH_RESIDUAL (12/12 AC, findings LOW ทั้งหมด); Product Owner สั่ง "แก้ไขเลย" = ยอมรับ v0.9 + อนุมัติ INC-N hardening mini-pack (§5.23) ให้แก้ finding ที่แก้ได้ (PIA-009/011/012) | Product Owner |
| DEC-018 | 2026-07-19 | INC-N landed ครบ 3 fix (verify static §5.24), commit รวมกับ INC-L+M ใน `3a41dfc` + deployed บน GitHub Pages — ปิด INC-N (debt เชิงกระบวนการ: ไม่แยก commit/ไม่ส่ง Completion Report แยก) | Product Owner / Claude |
| DEC-019 | 2026-07-19 | คำขอ real-time tracking + SOP workflow แบบแกร็บ → Claude สำรวจโค้ด grounded design; Product Owner เลือก **Option B** (เจาะเลือด + ส่งยา) เป็น INC-O (§5.25) | Product Owner |
| DEC-020 | 2026-07-19 | ขยาย INC-O (จากภาพอ้างอิง "สถานะการบริการ"): + บริการไลฟ์สไตล์ (แม่บ้าน/นวด/ice bath/กายภาพ/petcare) + หน้าสถานะแบบเสร็จ (timeline "สำเร็จ" + เวลา) + รูปแบบเงิน **มัดจำ ~30% + จ่ายส่วนที่เหลือหน้างาน** (home-visit; meds จ่ายเต็ม) — payment เข้า scope INC-O · ขนาดขยับเป็น L (Codex แบ่ง O-1/O-2 ได้) | Product Owner |
| DEC-021 | 2026-07-24 | INC-O audit = PASS_WITH_RESIDUAL (12/12 AC, findings LOW: PIA-013/014/015); Product Owner ตรวจรับด้วย commit `6f1e5d3` + push และสั่ง "ปิด v0.11" — เริ่ม planning v0.12 | Product Owner |
| DEC-022 | 2026-07-24 | v0.12 ทิศทาง: Product Owner เลือก **สมุดสุขภาพ + กราฟค่าสุขภาพ** (health companion) → Claude grounded design → INC-P Option A MVP (§5.27) รออนุมัติ | Product Owner |
| DEC-023 | 2026-07-24 | INC-P (§5.27) อนุมัติ v0.12 · คำขอเสริม "ประวัติย้อนหลังการใช้บริการต่อลูกค้า" = **DEFERRED** (Product Owner สั่ง "ยังไม่ต้องทำ") — เก็บเป็น candidate รอบหน้า (user self-history ใน hub / operator view คู่ INC-H) | Product Owner |

## 20. Approval Record

| Version | Status | Approved scope | Approver | Date | Notes |
|---|---|---|---|---|---|
| 0.1 | DRAFT | None | Pending Product Owner | — | Blocking Questions remain; no implementation authorization |
| 0.2 | DRAFT | None | Pending Product Owner | — | Phase 1 architecture audit added: baseline re-verified, uncommitted diff audited read-only (§14.2), increment menu §5.2 ready for selection; Blocking Questions BQ-001–BQ-005 remain |
| 0.3 | DRAFT | None (INC-F spec §5.3 awaiting approval) | Pending Product Owner | — | BQ-001/BQ-002 resolved (DEC-006/DEC-007); INC-0 executed PASS with reduced-motion limitation; full INC-F specification added; awaiting formal approval + user commit of celebration baseline |
| 0.3 | APPROVED_FOR_IMPLEMENTATION | INC-F ตาม §5.3 ทั้งหมด รวม default verification target (Chrome ล่าสุด, file:// offline, viewport ปกติ + 1366×768) ตามหมายเหตุ BQ-004 | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท | 2026-07-19 (Asia/Bangkok) | ข้อยกเว้น/เงื่อนไข: (1) precondition — ผู้ใช้ต้อง commit งาน celebration ก่อน Codex เริ่ม (ณ เวลาอนุมัติยังเป็น `M index.html`, HEAD `818249b`, hash `f471e803…a00b976`); (2) reduced-motion spot-check ของ INC-0 ยังเป็น open limitation ฝั่งผู้ใช้; (3) Claude ถูกห้ามแก้ implementation — งานแก้เป็นของ Codex เท่านั้น |
| 0.4 | DRAFT | None (batch INC-A+E+B §5.4–§5.7 awaiting approval) | Pending Product Owner | — | INC-F closed (PASS, `22f413c`); batch specifications complete; no blocking questions — awaiting formal approval |
| 0.4 | APPROVED_FOR_IMPLEMENTATION | Batch INC-A + INC-E + INC-B ตาม §5.4–§5.7 ทั้งหมด (หนึ่งรอบ implementation, ลำดับแนะนำ B→A→E, verification 13 AC + มาตรฐานเดิม) | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.4") | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline `22f413c`, `index.html` สะอาด ณ เวลาอนุมัติ; (2) `SPEC.md`/`TODO.md` มี doc diff ค้าง — ผู้ใช้ commit เองตามสะดวก ไม่ block งาน Codex; (3) Claude ห้ามแก้ implementation — Codex เท่านั้น |

Only the Product Owner may authorize changing the first line to `Status: APPROVED_FOR_IMPLEMENTATION`. Approval must reference the exact version/scope and resolve all Blocking Questions that affect that scope
| 0.5 | DRAFT | None — batch INC-A+E+B ปิดรอบแล้ว (audit PASS §5.8, ตรวจรับ DEC-010) | Pending Product Owner | — | ไม่มี scope ค้าง · เมนู increment ที่เหลือใน §5.2 (INC-C/D/G/H/I) รอผู้ใช้เลือก · stale sections (§7, §15.2, §16, §18) ถูก normalize ในรุ่นนี้ |
| 0.6 | DRAFT | None (batch INC-C+G+PIA-001 + README §5.9–§5.12 awaiting approval) | Pending Product Owner | — | รอบ INC-A+E+B ปิดแล้ว; สเปกรอบใหม่ครบ 12 AC; ไม่มี blocking question — รออนุมัติอย่างเป็นทางการ |
| 0.6 | APPROVED_FOR_IMPLEMENTATION | Batch INC-C + INC-G + PIA-001 ตาม §5.9–§5.12 ทั้งหมด (หนึ่งรอบ Codex, ลำดับแนะนำ C→G→PIA-001, verification 12 AC + มาตรฐานเดิม) + อัปเดต README เป็นงานเอกสารของ Claude หลัง audit PASS | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.6") | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline `e2a50ec`, `index.html` สะอาด ณ เวลาอนุมัติ; (2) doc diff ของ `SPEC.md` v0.6 ค้างอยู่ — ผู้ใช้ commit เองตามสะดวก ไม่ block; (3) Completion Report ต้องระบุ cosmetic deviation ทุกจุด (กติกาใหม่จาก PIA-003); (4) Claude ห้ามแก้ implementation — Codex เท่านั้น |
| 0.7 | DRAFT | None — batch INC-C+G+PIA-001 + README ปิดรอบแล้ว (audit PASS §5.13, ตรวจรับ DEC-012/013) | Pending Product Owner | — | ไม่มี scope ค้าง · เมนู increment ที่เหลือใน §5.2 (INC-D/H/I) รอผู้ใช้เลือก · AUD-002 ปิดแล้ว เหลือ finding เปิด: AUD-003/006/007/008 |
| 0.8 | DRAFT | None (batch INC-D + INC-J + INC-K §5.14–§5.17 awaiting approval) | Pending Product Owner | — | รอบ INC-C+G+PIA-001 ปิดแล้ว; สเปกรอบใหม่ครบ 13 AC (D-01…04, J-01…05, K-01…04); ไม่มี blocking question — รออนุมัติอย่างเป็นทางการ |
| 0.8 | APPROVED_FOR_IMPLEMENTATION | Batch INC-D + INC-J + INC-K ตาม §5.14–§5.17 ทั้งหมด (หนึ่งรอบ Codex, ลำดับแนะนำ D→J→K, verification 13 AC + มาตรฐานเดิม) | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.8") | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline application blob = `ca68ddb`, `index.html` สะอาด ณ เวลาอนุมัติ (hash `62d25830…`); (2) doc diff ของ README/SPEC/TODO (งานปิดรอบ v0.7 + สเปก v0.8) ค้างอยู่ — ผู้ใช้ commit เองตามสะดวก ไม่ block งาน Codex; (3) Completion Report ต้องระบุ cosmetic deviation ทุกจุด; (4) ข้อมูลใหม่เป็นข้อมูลจำลองล้วน (ASM-002) ห้ามเบอร์โทร/ข้อมูลจริง; (5) Claude ห้ามแก้ implementation — Codex เท่านั้น |
| 0.9 | DRAFT | None — batch INC-D+J+K ปิดรอบแล้ว (audit PASS §5.18, ตรวจรับ DEC-015) | Pending Product Owner | — | ไม่มี scope ค้าง · AUD-007 ปิดแล้ว (เหลือเปิด: AUD-003 monolith/no-test, AUD-006 simulation-only, AUD-008 accessibility) · เมนู increment ที่เหลือ §5.2: INC-H, INC-I |
| 0.9 | DRAFT | None (INC-L design pack + INC-M SOS ambulance §5.19–§5.21 awaiting approval) | Pending Product Owner | — | design audit 4 มุมมอง (49 findings/33 strengths) แปลงเป็นสเปกแล้ว; ไม่มี blocking question — รออนุมัติ v0.9 |
| 0.9 | APPROVED_FOR_IMPLEMENTATION | Batch INC-L + INC-M ตาม §5.19–§5.21 ทั้งหมด (Codex รอบเดียว ลำดับ L→M, verification 12 AC + inventory L รายข้อ) | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.9") | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline `a948687` (SHA-1 `41323998…`), `index.html` สะอาด; (2) doc diff README/SPEC/TODO ค้าง — ผู้ใช้ commit เอง ไม่ block; (3) Codex ขอแบ่งรอบ L→M ได้ถ้าประเมินว่าเสี่ยง; (4) INC-M จำลอง 100% ห้าม tel:/network; (5) ห้ามแตะจุดแข็ง/บริเวณ protected; (6) Claude ห้ามแก้ implementation |
| 0.9→ปิด | COMPLETED | INC-L+M เสร็จ — audit PASS_WITH_RESIDUAL (§5.22, 12/12 AC, findings LOW); Product Owner ยอมรับด้วยคำสั่ง "แก้ไขเลย" (DEC-017) | Product Owner (ผู้ใช้) | 2026-07-19 | การ commit v0.9 ยังค้าง ณ เวลาปิด — แนะนำ commit `INC-L+M: design polish + SOS ambulance dispatch` |
| 0.10 | APPROVED_FOR_IMPLEMENTATION | INC-N hardening mini-pack ตาม §5.23 ทั้งหมด (3 fix ~5 บรรทัด: PIA-009 SOS overlay guard, PIA-012 fitPhone zoom:0, PIA-011 token) — verification 5 AC | Product Owner (ผู้ใช้) — สั่ง "แก้ไขเลย" ในแชท | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline application state = `index.html` SHA-1 `36fcb910…` (v0.9 applied); (2) แนะนำ commit v0.9 ก่อนรัน Codex — ถ้าไม่ ให้ preserve v0.9 diff เป็น pre-existing work และแยก INC-N ใน Completion Report; (3) ไม่แตะพฤติกรรม SOS อื่น/protected; (4) Claude ห้ามแก้ implementation |
| 0.11 | DRAFT | None (INC-O SOP live tracker + หน้าสถานะการบริการ §5.25 awaiting approval) | Pending Product Owner | — | INC-N landed + ปิด (§5.24, DEC-018); INC-O ขยายตามภาพ + มัดจำ/จ่ายหน้างาน (DEC-020, 12 AC, ขนาด L); ไม่มี blocking question — รออนุมัติ v0.11 |
| 0.11 | APPROVED_FOR_IMPLEMENTATION | INC-O ตาม §5.25 ทั้งหมด (SOP live tracker + หน้าสถานะการบริการ, 3 กลุ่มบริการ, timeline "สำเร็จ" + เวลา, มัดจำ/จ่ายหน้างาน) — verification 12 AC + re-verify payment | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.11") | 2026-07-19 (Asia/Bangkok) | เงื่อนไข: (1) baseline `3a41dfc` (SHA-1 `3974d870`), working tree สะอาด; (2) ขนาด L — Codex ขอแบ่ง O-1→O-2 ได้ (AGENTS.md §6); (3) payment เข้า scope (re-verify) — protected อื่นห้ามแตะ; (4) จำลอง 100% ห้าม map/tel:/network; (5) design invariants §5.25 (direct-DOM timer, key by ref, กัน empty-overlay lock, modalKey คงที่) บังคับ; (6) Claude ห้ามแก้ implementation |
| 0.12 | DRAFT | None — INC-O ปิดรอบแล้ว (audit PASS_WITH_RESIDUAL §5.26, ตรวจรับ DEC-021) · v0.12 planning | Pending Product Owner | — | ไม่มี scope ค้าง · เมนู/ต่อยอดใน §5.2 · PIA-013 (meds label) polish เล็กรอเลือก |
| 0.12 | DRAFT | None (INC-P สมุดสุขภาพ + กราฟ §5.27 awaiting approval) | Pending Product Owner | — | grounded design เสร็จ (Option A MVP read-only, 9 AC); ไม่มี blocking question — รออนุมัติ v0.12 |
| 0.12 | APPROVED_FOR_IMPLEMENTATION | INC-P สมุดสุขภาพ + กราฟค่าสุขภาพ ตาม §5.27 (Option A MVP, read-only, 9 AC) | Product Owner (ผู้ใช้) — อนุมัติเป็นลายลักษณ์อักษรในแชท ("อนุมัติ v0.12") | 2026-07-24 (Asia/Bangkok) | เงื่อนไข: (1) baseline `6f1e5d3` (SHA-1 `6d225e06`); (2) additive — ห้ามแตะ protected; (3) SVG invariants §5.27 บังคับ; (4) read-only ไม่มี persisted key; (5) จำลอง 100%; (6) ประวัติลูกค้า deferred (DEC-023); (7) Claude ห้ามแก้ implementation |
