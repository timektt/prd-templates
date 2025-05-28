# Product Requirements Document (PRD)

**Product:** NeighborLink – Neighbor Skill‑Sharing Mobile App
**Author:** <your name>
**Version:** 1.3  (2025‑05‑29)
**Status:** Draft for stakeholder review
**Last Updated:** 2025‑05‑29
**Target Launch City:** <Pilot City>
**Target Launch Date:** Q4 2025

---

## 1  Executive Summary

NeighborLink คือแอปมือถือ (iOS & Android) ที่จับคู่ “ผู้ให้ทักษะ (Skill Providers)” กับ “ผู้ขอความช่วยเหลือ (Skill Seekers)” ภายในรัศมีใกล้บ้าน เช่น ช่วยตั้งค่าโทรศัพท์ เลี้ยงสัตว์ ดูแลต้นไม้ หรือช่วยยกของหนัก ฟีเจอร์ MVP ครอบคลุม : ค้นหา → จับคู่ → แชตเข้ารหัส E2E หลังยอมรับกันทั้งสองฝ่าย การชำระเงิน (ถ้ามี) จัดการนอกแพลตฟอร์มในช่วงแรก

**กลยุทธ์ธุรกิจ**

1. เพิ่มการมีส่วนร่วมในชุมชน – ให้เพื่อนบ้านช่วยกันง่ายขึ้น
2. ลด Friction ในการขอความช่วยเหลือรายวัน
3. สร้างฐานผู้ใช้และข้อมูลรีวิวเพื่อปูทางสู่ **ตลาดบริการท้องถิ่น** (local services marketplace) ระยะยาว
4. เปิดทางรายได้อนาคต : ค่านายหน้า, subscription Premium, โฆษณาบริการ B2B (เช่น ร้านฮาร์ดแวร์ใกล้บ้าน)

---

## 2  Success Metrics (OKRs)

| Objective                    | Key Results (ภายใน 6 เดือนหลังเปิดตัว)                                     |
| ---------------------------- | -------------------------------------------------------------------------- |
| **O1 – Adoption**            | • ≥ 5 000 total installs ในเมืองทดลอง  • ≥ 2 000 MAU (> 40 %)              |
| **O2 – Matching Efficiency** | • ≥ 100 matches สำเร็จ/เดือน  • ≥ 80 % requests ถูกตอบรับ/ปฏิเสธภายใน 48 h |
| **O3 – Quality & Trust**     | • ≥ 90 % feedback 👍  • ≤ 1 % abuse reports ต่อ interaction                |
| **O4 – Reliability**         | • Crash‑free sessions ≥ 99.5 %  • P95 search latency ≤ 500 ms              |
| **O5 – Retention**           | • Day‑30 retained users ≥ 25 %  • Avg. sessions/user/week ≥ 3              |

---

## 3  Scope

### 3.1 In‑Scope (MVP)

| Area                      | Features                                                                                                               |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Account & Identity**    | Email + password, OAuth (Google/Apple), Forgot PW, avatar upload                                                       |
| **Provider Flow**         | CRUD skill listings, toggle Active, set availability schedule, location radius (home only / neighbourhood / city‑wide) |
| **Seeker Flow**           | Category & keyword search, distance‑sorted results, profile preview, send request + optional note                      |
| **Matching**              | Accept/decline, push + in‑app notifications, E2E chat unlock on mutual accept                                          |
| **Feedback & Reputation** | One‑time 👍/👎 per match, aggregate rating, report abuse button                                                        |
| **Admin Portal (Web)**    | Category CRUD, user suspension/unsuspend, moderation queue, dashboard (DAU, reports, conversion)                       |

### 3.2 Out‑of‑Scope (De‑scoped / Later Phases)

* In‑app payments / escrow
* Points or bartering economy
* Public “help wanted” board
* KYC / national‑ID verification
* AI‑driven skill recommendation
* Multi‑city marketplace / sponsorship program

---

## 4  Stakeholders & RACI

| Role          | Name     | R     | A     | C     | I     | Responsibility (ย่อ)                    |
| ------------- | -------- | ----- | ----- | ----- | ----- | --------------------------------------- |
| Product Owner | <PO>     |       | **A** | **C** | **I** | วิสัยทัศน์, roadmap, prioritise backlog |
| Eng Lead      | <Eng>    | **R** | **C** |       | **I** | สถาปัตย์, release quality               |
| UX Lead       | <Design> | **R** | **C** |       | **I** | Research, wireframe, usability test     |
| QA Mgr        | <QA>     | **R** |       | **C** | **I** | Test plan, defect triage                |
| Legal         | <Legal>  | **R** |       | **C** |       | Privacy, ToS, PDPA/GDPR                 |
| Marketing     | <Mkt>    | **R** |       | **C** | **I** | Campaign, pilot‑city onboarding         |

> **RACI Legend:** **R** Responsible • **A** Accountable • **C** Consulted • **I** Informed

---

## 5  Personas (Expanded)

| Persona                                   | Bio & Goal                                     | Key Pain                  | Tech Comfort | Quote                                        |
| ----------------------------------------- | ---------------------------------------------- | ------------------------- | ------------ | -------------------------------------------- |
| **Emma** (28) Designer, WFH 3 วัน/สัปดาห์ | อยากพบเพื่อนบ้าน สร้าง network                 | ไม่อยากให้เบอร์โทรสาธารณะ | ★★★☆☆        | “แค่คลิกเดียวถ้าใครช่วยยกกล่องให้ได้จะดีมาก” |
| **Mr. Somchai** (67) Retiree              | ต้องการคนช่วยตั้งค่ามือถือ, ยกของตลาด          | ไม่ไว้ใจคนแปลกหน้า        | ★☆☆☆☆        | “ขอให้เจอคนข้างบ้านที่ช่วยได้แบบรวดเร็ว”     |
| **Carlos** (35) พ่อทำงานเต็มเวลา          | ต้องการฝากแมวเวลาไปต่างจังหวัด, แลกสอนภาษาสเปน | หา provider ใกล้บ้านยาก   | ★★★★☆        | “อยากเห็น rating ชัด ๆ ก่อนนัดเจอกันจริง”    |

---

## 6  Key User Stories & Acceptance Criteria

> *Full backlog in Jira; below are critical paths for MVP.*

### 6.1 Registration & Onboarding

*US‑01* – *As a new user, I can sign up with Google to skip password.*
**AC:** Email verified; unverified users cannot post skills or chat.

*US‑02* – *As a user, I can set my neighbourhood on a map so results are relevant.*
**AC:** Map picker > save lat/lon ±50 m; location stored server‑side.

### 6.2 Provider: Create Skill

*US‑04* – Provider lists a skill with category, title ≤ 80 chars, desc ≤ 300 chars, availability (weekday + time block).
**AC:** Validation inline; index ≤ 30 s.

### 6.3 Search & Match

*US‑09* – Seeker gets results sorted distance ASC then rating DESC.
**AC:** Automated tests for 1 km/5 km/10 km; Haversine vs PostGIS result diff ≤ 1 %.

### 6.4 E2E Chat

*US‑13* – Chat only opens after both accept.
**AC:** Pre‑accept POST `/messages` returns 403; post‑accept returns 201.

### 6.5 Feedback

*US‑18* – Either party leaves 👍/👎 once per match.
**AC:** Duplicate feedback returns 409.

---

## 7  Functional Requirements (Detailed)

| ID   | Description                                                          | API / Module   | Priority |
| ---- | -------------------------------------------------------------------- | -------------- | -------- |
| FR‑1 | Geocode neighbourhood → lat/lon on signup                            | Auth Service   | Must     |
| FR‑2 | `GET /v1/skills/search` filters `category,q,radius_km,lat,lon,limit` | Search BFF     | Must     |
| FR‑3 | Push notif via FCM/APNs; fallback in‑app inbox                       | Notif Svc      | Must     |
| FR‑4 | WebSocket chat, retain 90 days, Signal protocol E2E                  | Chat Svc       | Should   |
| FR‑5 | POST `/v1/report` auto‑hide content pending review                   | Moderation Svc | Should   |
| FR‑6 | Rate‑limit: 30 requests/IP/min for search                            | API Gateway    | Must     |
| FR‑7 | Account deletion – GDPR 30‑day grace                                 | User Svc       | Must     |

---

## 8  Non‑Functional Requirements (NFR)

* Performance – P95 search ≤ 400 ms at 10 RPS; chat send‑latency ≤ 150 ms.
* Availability – 99.8 % monthly uptime (per service).
* Security – OWASP Mobile Top 10 mitigated; quarterly pen‑test before GA.
* Privacy – Store location rounded ≤ 2 decimal; opt‑out analytics.
* Scalability – 50 k users, 500 concurrent chats without degradation.
* Accessibility – WCAG 2.1 AA (font scale, voice‑over).
* Localization – EN/TH initially; string files externalised (i18n).

---

## 9  Technical Architecture (High‑Level, บรรยายธรรมดา)

* **Mobile** : Flutter (iOS / Android) เรียก REST + WebSocket
* **API Gateway** : NGINX / Kong ทำ rate‑limit และ JWT verify
* **BFF (GraphQL)** : รวมข้อมูลจาก Service ย่อย (Auth, Skill, Chat)
* **Service ย่อย**

  * Auth Service (JWT, bcrypt)
  * Skill Service (CRUD + Search)
  * Chat Service (WebSocket + เก็บข้อความ 90 วัน)
* **ฐานข้อมูล** : Postgres (User, Skill, Feedback) + Redis (presence / rate‑limit)
* **Notification** : Firebase FCM / APNs
* **Storage** : S3-compatible สำหรับรูปโปรไฟล์
* **CI/CD** : GitHub Actions → Fastlane build → Deploy TestFlight / Play Console
* **Infra** : Kubernetes (GKE/EKS) + Terraform IaC

---

## 10  Data Model (สรุปคีย์ฟิลด์)

* **users** : `id`, `email`, `password_hash`, `display_name`, `avatar_url`, `lat`, `lon`, `created_at`
* **skills** : `id`, `user_id`, `category`, `title`, `description`, `availability_json`, `is_active`, `created_at`
* **connections** : `id`, `seeker_id`, `provider_id`, `status` (pending / accepted / declined), `created_at`
* **messages** : `id`, `connection_id`, `sender_id`, `ciphertext`, `sent_at`
* **feedback** : `id`, `connection_id`, `rater_id`, `vote` (👍/👎), `created_at`

---

## 11  Instrumentation & Analytics

* ติดตาม Event หลัก: `signup`, `skill_listed`, `search`, `request_sent`, `match_accept`, `message_send`, `feedback`
* Funnel สำคัญ: search → request → accept → thumbs‑up
* Crash & Performance: Firebase Crashlytics + Performance SDK
* Log Format: JSON → Cloud Logging; แจ้งเตือนถ้า 5xx error rate > 1 %

---

## 12  Compliance & Legal

* **ToS** : แอปเป็นตัวกลาง ไม่รับผิดชอบการพบกันออฟไลน์
* **Privacy** : เก็บพิกัดหยาบ, ผู้ใช้ซ่อนโปรไฟล์ได้
* **Data Retention** : ข้อความลบทิ้ง 90 วัน, สกิลหมดอายุ 180 วันหากไม่เคลื่อนไหว
* **PDPA/GDPR** : ผู้ใช้ดาวน์โหลดหรือลบข้อมูลได้ภายใน 30 วัน

---

## 13  Release Plan

* **Alpha (M‑3)** : ส.ค. 2025 – Core flow เสร็จ, internal 20 คน
* **Beta  (M‑2)** : ก.ย. 2025 – ปล่อย TestFlight / Closed Play, 200 คน
* **Launch (M‑0)** : พ.ย. 2025 – เปิดสโตร์ในเมืองนำร่อง
* **Post‑Launch (M+3)** : ก.พ. 2026 – ตัดสิน roadmap payment + เมืองที่สอง

---

## 14  Risk & Mitigation

* Engagement ตกหลังช่วงแรก → เพิ่ม gamification, ส่ง notification "เพื่อนบ้านเพิ่มสกิลใหม่"
* การคุกคามในแชต → AI toxic filter + ปุ่มรายงาน
* ความกังวลเรื่องตำแหน่ง → เก็บพิกัดหยาบ + โหมด Invisible

---

## 15  Glossary (ศัพท์สำคัญ)

* **Skill Provider** : คนให้ความช่วยเหลือ
* **Skill Seeker** : คนขอความช่วยเหลือ
* **Connection** : คู่ที่กดรับแมตช์แล้ว
* **MAU** : ผู้ใช้แอปรายเดือน

---

## 16  Open Questions

1. จะเปิดบอร์ด "ช่วยหน่อย" สาธารณะในเฟสถัดไปไหม?
2. จะตัด OS เก่าแล้วใช้ SwiftUI / Compose only ได้หรือไม่?
3. ค่า radius ค้นหาเริ่มต้นสมควรเป็นเท่าไรดี?
4. Presence online/offline มีผลต่อ Retention แค่ไหน?

---

## 17  Next Steps

* UX : คลิก prototype บน Figma – **ครบ 10 มิ.ย. 2025**
* Eng : ทดลองเข้ารหัสแชต E2E – **ครบ 20 มิ.ย. 2025**
* Meet : ประชุม sign‑off เอกสาร – **25 มิ.ย. 2025**

---

> *เอกสารนี้เป็นแนวทาง ปรับยืด‑หดได้ตามบริบททีมของคุณครับ* 🙂
