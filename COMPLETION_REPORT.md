# Completion Report — SPEC v0.9 INC-L + INC-M

Status: IMPLEMENTED_AND_VERIFIED

Date: 2026-07-19 (Asia/Bangkok)

Implementation and Verification Lead: Codex

Approved scope: `SPEC.md` §5.19–§5.21

Application baseline: `a948687` (`index.html` Git blob `8c729c64a99ce133c0d1722d8d8ab8b993b21055`)

## 1. Preflight

Result: **PASS**

| Check | Evidence / Result |
|---|---|
| Working directory | `/Users/dome/Desktop/ORNSIRIN PROJECT` |
| Required documents | Read `AGENTS.md`, `CLAUDE.md`, `SPEC.md`, `README.md`, `TODO.md` and the prior Completion Report completely |
| SPEC status | First line is exactly `Status: APPROVED_FOR_IMPLEMENTATION` |
| Approval Record | SPEC v0.9 approved by Product Owner on 2026-07-19; implementation owner is Codex; exact scope is INC-L + INC-M (§5.19–§5.21), 12 AC |
| Blocking Questions | None; scope, Non-goals, rollback and all 12 AC are testable |
| Git baseline | `main` tracks `origin/main`; HEAD and origin/main both `a948687cb9116748d1ebc7d37454d3206855287a` |
| Application baseline | Clean `index.html`: file SHA-1 `41323998712bbee07f97ccd9965925b156edaa93`, SHA-256 `f419894bc7058b239a5f48633b9ea49d6ea39edf32daff2625bc179efdafd299`, 2,588 lines, 1,857,243 bytes |
| Pre-existing work | Unstaged `README.md`, `SPEC.md`, `TODO.md`; 152 insertions / 8 deletions in total; no staged or untracked files; `index.html` had no diff |
| Architecture | Unchanged single-file offline client: vanilla JS state, `sessionStorage`, template strings into `innerHTML`, embedded libraries/assets, no backend/build/test framework |
| Scope control | Implementation limited to `index.html`; this file records the required report; no dependency, external service, persisted key, API or architecture change |

The stale historical sentence in SPEC §16 (“Next application increment NOT DEFINED”) conflicts with the newer Document Control, DEC-016, §§5.19–5.21 and Approval Record. The latest explicit records and the Product Owner’s command all authorize INC-L + INC-M, so this was treated as non-blocking document drift and is disclosed for Post-implementation Audit.

No reset, revert, checkout, dependency installation, staging, commit or push was performed.

## 2. Implemented Scope

### INC-L — Design polish pack (§5.19)

All 30 approved items were implemented. Inventory required by §5.21:

| # | Result | Implementation |
|---:|---|---|
| 1 | DONE | Logged-out sales hero, three benefit chips and reduced login hint |
| 2 | DONE | Consult form exposes ฿400 consultation and optional +฿100 certificate before submission |
| 3 | DONE | Consult form uses green `screenHero`; psychiatry reassurance card uses a pale surface rather than green-on-green |
| 4 | DONE | Added `--ink-soft:#5B6350`; removed all `#8A9080` and `#7A8271` occurrences |
| 5 | DONE | Bottom-nav labels are 11.5px/500 and inactive labels use `--ink-soft` |
| 6 | DONE | Queue wait is a separate 13px `--ink-soft` line |
| 7 | DONE | Lab chips are 11px with darker passing/follow-up palettes and an alert-circle on “ควรติดตาม” |
| 8 | DONE | Resident benefit, health allergy and SOS-summary typography follow the specified sizes/weights |
| 9 | DONE | User bar uses 26px avatars, 30px bell, `py-1.5` logout and 64px name cap |
| 10 | DONE | Symptom chips are 13px/`py-2.5`; icon close buttons are 40px with `aria-label="ปิด"`; time captions are 10px `#6D7766` |
| 11 | DONE | Overlay/modal headings are standardized at 17px |
| 12 | DONE | Full-row chevrons are 16px; only compact pet-status rows retain 13px |
| 13 | DONE | Soft card shadow includes `.rounded-3xl` in app and overlay roots |
| 14 | DONE | Service section headings use 14px semibold `serif-th` |
| 15 | DONE | Added `--danger:#B0543F`; removed `#C0503F` from call-end/validation paths |
| 16 | DONE | Login palette uses project tokens, medication icon uses clay and green gradients use primary variables |
| 17 | DONE | Medication empty state now has icon circle, serif heading and guidance |
| 18 | DONE | Unpaid queue primary action is payment; Home is secondary |
| 19 | DONE | Closing a lifestyle payment sheet restores the exact detail/provider/day/time selection |
| 20 | DONE | Consultation payment lists ฿400 consultation and optional ฿100 certificate line items |
| 21 | DONE | Disabled Specialty/Consult CTAs expose helper captions and `aria-disabled` |
| 22 | DONE | Unavailable doctors show “ไม่ว่าง” at opacity 0.6 |
| 23 | DONE | Lifestyle carousel cards are fully clickable, service-colored, chevron-led and show resident benefit or starting price |
| 24 | DONE | Detail flow defaults to tomorrow when every today slot has passed; provider selection also advances when that provider has no today slot |
| 25 | DONE | Service price units are displayed and call duration uses grammatically correct minute/second formatting |
| 26 | DONE | Call-end success shows doctor/avatar, duration, exact 15-minute summary copy and “ขอบคุณที่ดูแลตัวเองวันนี้” |
| 27 | DONE | Lab cards stagger only under `.screen-enter`; internal re-render has no stagger replay |
| 28 | DONE | Connected-call avatar retains the existing breathing/listening ring |
| 29 | DONE | Queue-zero primary CTA receives one `exhale-once` event |
| 30 | DONE | Queue notification is urgent/clay; bell badge exhales only on count change; health-card icon exhales only when opened |

### INC-M — SOS ambulance tracker (§5.20)

- Replaced single-tap callback dispatch with a three-action confirmation stage.
- Preserved the v0.8 callback path and its emergency health/caregiver summary.
- Added a simulated ambulance dispatch tracker with ETA 8, five-second cadence, progress, mock hospital/vehicle/team details, health-card transfer, caregiver notification and permanent simulation notice.
- Added a distinct green arrival state, direct-DOM updates, persisted modal ETA, F5 recovery, ArrowRight acceleration and clean single-timer start/stop behavior.
- Added cancel and close controls without introducing a real call, message, GPS, map or network action.

## 3. Files Changed

| File | Ownership / Reason |
|---|---|
| `index.html` | Codex implementation for the approved INC-L + INC-M scope; final diff 406 insertions / 192 deletions |
| `COMPLETION_REPORT.md` | Codex replaced the prior-round report with this v0.9 handoff; prior content remains recoverable in Git history |

`README.md`, `SPEC.md` and `TODO.md` were already modified before implementation and were not edited by Codex. `CLAUDE.md` and `AGENTS.md` remain unchanged.

## 4. Verification

| Verification | Result |
|---|---|
| Inline-script syntax | **PASS**, exit 0 — bundled Node compiled all 3 non-empty inline scripts with `new Function` |
| `git diff --check` | **PASS**, exit 0 |
| Forbidden design values | **PASS** — `rg` found no `#8A9080`, `#7A8271`, `#C0503F`, `text-gray-400` or `text-black` (expected `rg` exit 1 = no match) |
| New outbound surface | **PASS** — added-line scan found no HTTP(S), `tel:`, `fetch`, XHR, WebSocket, new form or anchor (expected `rg` exit 1 = no match) |
| Contrast calculation | **PASS**, exit 0 — eight changed foreground/background pairs range from 4.610:1 to 6.552:1, all ≥4.5:1 |
| Protected-region hash comparison | **PASS**, exit 0 — escaping, storage/hydration, INC-J/K data, celebration CSS, confetti generator, count-up gate and privacy gate are byte-identical to `a948687` |
| Browser runtime | **PASS** in Codex in-app Chromium through a server bound only to `127.0.0.1:8765` |
| L primary flows | **PASS** — sales hero, consult pricing/helpers, unavailable doctor, queue hierarchy/line items, payment, queue-zero CTA, call summary, lab stagger/no-replay and health-card event motion observed in live DOM |
| Lifestyle recovery | **PASS** — selected B/พี่แดง, tomorrow, 15:00; opened payment; close restored all three selections and enabled CTA |
| 1366×768 normal + Elder layout | **PASS** at exact 1366×768 — document/app/modal horizontal overflow all 0; `fitPhone` kept phone bottom at 750.67px; u3 Elder Mode confirmed; SOS dialog scrolls vertically |
| Default viewport | **PASS** at 1280×720 — document/app/modal horizontal overflow all 0; phone bottom 701.75px; Elder SOS dialog remains scrollable |
| SOS confirmation/callback | **PASS** — first tap showed exactly 3 actions and no tracker; cancel returned to unchanged u3 Home; callback retained health summary and caregiver confirmation |
| SOS tracker | **PASS** — observed ETA 8, progress, all mock vehicle/health/caregiver/simulation content; ArrowRight changed 8→7; live arrival reached green “รถถึงหน้าบ้านแล้ว”, “ถึงแล้ว” 22px and 100% progress |
| SOS persistence/timer | **PASS** — reload during dispatch returned to the tracker with persisted progress; measured successive timer changes 4→3→2 with 4,979ms separation; cancel followed by 5.4s wait left no tracker/overlay; repeated open/close used one cadence |
| Privacy/gating | **PASS** — u3-only emergency UI appears while logged in; after logout, SOS/health/caregiver/blood data and overlay are absent |
| Presenter-key regression | **PASS** — queue ArrowRight reached queue zero; tracker ArrowRight accelerated ETA; ArrowRight on u3 Home with no tracker caused no overlay/state-visible change |
| Reduced motion | **PASS by deterministic static rule** — all new lab/SOS progress/event selectors are included in the existing `prefers-reduced-motion: reduce` block; lab animation is scoped to `.screen-enter`; count-up retains its existing `matchMedia` guard |
| Console | **PASS** — 0 errors after all flows/reloads; only the pre-existing embedded Tailwind production-use warning |
| Offline packaging | **PASS** — source/diff scan found no new outbound reference and the local server log contained only three requests for `/index.html` (200, 200, 304), no other resource/network request |

Direct `file://` automation is blocked by the in-app Browser URL policy, so runtime used loopback-only HTTP; no bypass was attempted. Single-file packaging and offline behavior were separately verified by added-reference scan and request log.

Final `index.html` fingerprints: file SHA-1 `36fcb910765859ccabd997407d8da5ded4448fcc`; SHA-256 `600184fe378deb64b1ac54e0800bd1a776b517317bc7bf5161ce9b8963f5d49f`; 2,802 lines; 1,876,266 bytes.

## 5. Acceptance Criteria

| AC | Result | Evidence |
|---|---|---|
| AC-L-01 | PASS | All three Group A outcomes observed: logged-out sales hero/chips, ฿400/+฿100 pricing, green consult `screenHero` with non-green reassurance card |
| AC-L-02 | PASS | Forbidden colors absent; specified typography/colors present; all eight changed contrast pairs ≥4.5:1 |
| AC-L-03 | PASS | Modal headings, chevrons, 3xl shadow, serif sections, palette cleanup and upgraded medication empty state verified by source scan and browser DOM |
| AC-L-04 | PASS | All items 18–25 implemented; queue and lifestyle recovery exercised live; evening/tomorrow fallback verified in the explicit `every(isPastSlot)` branch |
| AC-L-05 | PASS | Call summary/listen ring, lab stagger/no replay, queue-zero CTA and health event motion observed; notification change gate and reduced-motion closure verified in source |
| AC-L-06 | PASS | Protected mechanics hash-identical; 1366×768/default/Elder horizontal overflow 0; console errors 0; offline request log clean |
| AC-M-01 | PASS | One SOS tap produced confirmation with exactly three large actions; no dispatch occurred; cancel returned to intact u3 Home |
| AC-M-02 | PASS | Tracker started at 8, advanced at ~5s, ArrowRight accelerated, progress reached 100% and arrival state changed clearly; vehicle mock complete |
| AC-M-03 | PASS | Health/caregiver values and permanent simulation label present; no added anchor, `tel:`, form or network action |
| AC-M-04 | PASS | Callback path retained v0.8 callback copy, full emergency summary, sent-to-team line and caregiver line |
| AC-M-05 | PASS | Cancel/close work; F5 restored dispatch; cadence measured 4,979ms with no stacked interval; cancellation remained clean beyond one tick |
| AC-M-06 | PASS | Exact 1366×768 + stricter 1280×720 Elder layouts scroll without horizontal overflow; logged-in u3 gate retained; keyboard paths do not conflict |

**Overall: 12/12 PASS.**

## 6. Deviations and Cosmetic Decisions

Functional or scope deviation: **None**.

Disclosed cosmetic/supporting decisions:

- Disabled time-slot background changed from `#EFEAE0` to `#FFFDF8`. This keeps the exact mandated 10px `#6D7766` caption while raising contrast from 3.909:1 to 4.610:1, resolving the tension between L10’s exact color and AC-L-02’s ≥4.5:1 requirement. Disabled state remains distinguishable through muted main text, line-through/status text and the disabled attribute.
- Live SOS arrival now updates ETA text to green 22px as well as the heading/icon/progress. This makes direct-DOM arrival visually identical to an F5 render at ETA 0.
- No generated assets, data changes, animation redesign or architecture refactor were introduced.

## 7. Pre-existing Work Preserved

- Unstaged Product Owner/Claude changes in `README.md`, `SPEC.md` and `TODO.md` were preserved byte-for-byte throughout Codex implementation.
- The clean `a948687` application baseline was used for ownership/diff separation; only approved regions in `index.html` were changed.
- INC-C escaping, INC-D storage recovery, INC-J/K data/gating, payment celebration mechanics, count-up behavior and the privacy gate were not refactored; protected-block hashes are recorded in §4.
- No destructive Git command, dependency installation, staging, commit or push occurred.

## 8. Residual Risks

- The repository has no automated test framework; verification combines deterministic Node/static checks with repeatable browser flows.
- Direct `file://` runtime automation and reduced-motion media emulation are unavailable in the in-app browser. Offline behavior and reduced-motion closure therefore use source/request-log evidence; a final OS-level reduced-motion spot-check remains prudent before presentation.
- The all-today-slots-passed condition was source-verified because the real test clock still had a 19:00 slot available; changing system time was not necessary or authorized.
- The embedded Tailwind runtime emits its existing production-use warning; there are no console errors.
- Existing modal accessibility debt remains: no focus trap/return, dialog semantics, ETA `aria-live` or progressbar semantics. These are outside approved L/M scope and should be separately specified if required.
- SOS, health, caregiver, payment and VDO behavior remain synthetic presentation simulations, not production health/emergency services.
- Background-tab timer throttling remains browser-dependent; persisted ETA and F5 restoration mitigate presentation recovery but do not model real dispatch time.

## 9. Git Status at Handoff

Expected final status after this report update:

```text
## main...origin/main
 M COMPLETION_REPORT.md
 M README.md
 M SPEC.md
 M TODO.md
 M index.html
```

Ownership:

- Pre-existing preserved work: `README.md`, `SPEC.md`, `TODO.md`.
- Codex work in this round: `index.html`, `COMPLETION_REPORT.md`.
- No staged or untracked files; HEAD and origin/main remain `a948687`; no Codex commit or push.

## 10. Recommended Next Step

Hand off to Claude for the mandatory Post-implementation Audit: reconcile the stale §16 sentence, review every `index.html` hunk against SPEC §§5.19–§5.21, independently recheck the 30-item inventory and 12 AC (especially contrast support decision, timer cleanup/F5 and protected-region hashes), then present the audited result to the Product Owner for acceptance.
