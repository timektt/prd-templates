# Product Requirements Document (PRD)

**Product** : NeighborLink – Neighbor Skill-Sharing Mobile App  
**Author**  : \<your name\>  
**Version**  : 1.3  (2025-05-29)  
**Status**  : Draft for stakeholder review  
**Last Updated** : 2025-05-29  
**Pilot City**  : \<city\>  
**Target Launch** : Q4 2025  

---

## 1  Executive Summary
NeighborLink คือแอปจับคู่ “ผู้ให้ทักษะ” (Skill Providers) เช่น ช่วยตั้งมือถือ, เลี้ยงหมา, ยกของ กับ “ผู้ขอความช่วยเหลือ” (Skill Seekers) ในละแวกบ้าน ฟีเจอร์ MVP ครอบคลุมการค้นหา → จับคู่ → แชตเข้ารหัส (E2E) การจ่ายเงิน (ถ้ามี) ทำกันนอกแพลตฟอร์มช่วงแรก  

**Business Outcome**

| # | ผลลัพธ์ธุรกิจ |
|---|----------------|
| 1 | เพิ่ม engagement ในชุมชน ≥ 20 % |
| 2 | ลดเวลา “หา Helper” จาก > 1 วัน เหลือ < 4 ชม. |
| 3 | วางฐานข้อมูลรีวิวเพื่อขยายสู่ local services marketplace |

---

## 2  Success Metrics (OKRs)

| Objective | Key Results (ภายใน 6 เดือนหลัง LAUNCH) |
|-----------|-----------------------------------------|
| **O1 Adoption** | • ≥ 5 000 installs ในเมืองทดสอบ <br>• ≥ 2 000 MAU (≥ 40 %) |
| **O2 Matching Efficiency** | • ≥ 100 matches/เดือน <br>• ≥ 80 % requests ถูกตอบใน 48 ชม. |
| **O3 Quality & Trust** | • ≥ 90 % feedback 👍 <br>• ≤ 1 % abuse report/interaction |
| **O4 Reliability** | • Crash-free ≥ 99.5 % <br>• P95 search latency ≤ 500 ms |
| **O5 Retention** | • Day-30 retention ≥ 25 % <br>• เฉลี่ย ≥ 3 sessions/user/สัปดาห์ |

---

## 3  Scope

### 3.1  In-Scope (MVP)

| หมวด | ฟีเจอร์ หลัก |
|------|--------------|
| Account & Identity | Email/Password + OAuth, reset, avatar |
| Provider Flow | CRUD skill, toggle Active, เลือก availability |
| Seeker Flow | ค้นตาม หมวด / คีย์เวิร์ด, เรียงตามระยะ, view profile |
| Matching | Accept/Decline, push noti, แชต E2E หลังรับกันทั้งคู่ |
| Feedback | 👍/👎 ครั้งเดียวต่อแมตช์, รวม rating ใน โปรไฟล์ |
| Admin Web | จัดการหมวด, แบนผู้ใช้, คิว moderation, dashboard |

### 3.2  Out-of-Scope (เลื่อนไปเฟสถัดไป)

* In-app payment / escrow  
* ระบบแต้ม/แลกเปลี่ยน  
* บอร์ด “ขอความช่วยเหลือ” สาธารณะ  
* KYC/ID verification  
* AI recommendation  

---

## 4  Stakeholders ( RACI )

| Role | ชื่อ | R | A | C | I | หน้าที่ |
|------|------|---|---|---|---|---------|
| Product Owner | \<PO\> |   | **●** | ● | ● | Vision / roadmap |
| Engineering Lead | \<Eng\> | **●** |   | ● | ● | สถาปัตย์, Delivery |
| UX Lead | \<UX\> | **●** |   |   | ● | Research, Prototype |
| QA Mgr | \<QA\> | **●** |   |   | ● | Test plan |
| Legal | \<Legal\> | **●** |   | ● |   | PDPA / GDPR |
| Marketing | \<Mkt\> | **●** |   | ● |   | Launch campaign |

> **R** Responsible • **A** Accountable • **C** Consulted • **I** Informed

---

## 5  Personas (Expanded)

| Persona | เป้าหมาย | Pain | Quote |
|---------|----------|------|-------|
| **Emma** 28, Designer | หาเพื่อนบ้านช่วยยกของ/รดน้ำ | ไม่อยากแชร์เบอร์โทร | “แค่คลิกเดียวถ้ามีคนช่วยได้จะดีมาก” |
| **Mr. Somchai** 67, Retiree | ตั้งค่ามือถือ, ยกของตลาด | ไม่ไว้ใจคนแปลกหน้า | “ขอคนละแวกบ้านที่รีวิวดี” |
| **Carlos** 35, พ่อทำงาน | ฝากแมว, แลกสอนภาษาสเปน | หา helper ใกล้บ้านยาก | “อยากดู rating ชัด ๆ ก่อน” |

---

## 6  Key User Stories & AC

<details>
<summary>กดดูตาราง User Story</summary>

| Epic | US ID | Story | AC (สำเร็จเมื่อ) |
|------|------|-------|-------------------|
| Registration | US-01 | New user sign up with Apple |🤝 Email verified; unverified = no post/chat |
| Provider | US-04 | Create skill listing |⏱ Index ≤ 30 s; validate inline |
| Search | US-09 | Results sorted by distance ASC → rating |⚡ Test 1 km/5 km/10 km; diff ≤ 1 % |
| Chat | US-13 | Chat unlock after mutual accept |🔒 Pre-accept POST `/messages` → 403  |

</details>

---

## 7  Functional Requirements

| ID | Description | Module | Priority |
|----|-------------|--------|----------|
| **FR-1** | Geocode neighbourhood at signup | Auth Svc | Must |
| **FR-2** | `GET /skills/search` with filters | Search BFF | Must |
| **FR-3** | Push notif via FCM/APNs | Notif Svc | Must |
| **FR-4** | WebSocket chat, retain 90 d, Signal E2E | Chat Svc | Should |
| **FR-5** | Abuse report => auto-hide content | Moderation | Should |

---

## 8  Non-Functional Requirements

* **Performance** : P95 search ≤ 400 ms @ 10 RPS  
* **Availability** : ≥ 99.8 % uptime / month  
* **Security** : OWASP Mobile Top 10 mitigated; quarterly pen-test  
* **Privacy** : Store lat/lon rounded ±0.01°; opt-out analytics  
* **Scalability** : 50 k users, 500 concurrent chats  
* **Accessibility** : WCAG 2.1 AA  
* **Localization** : EN & TH; external strings  

---

## 9  System Architecture (mermaid)

```mermaid
flowchart TD
  subgraph Mobile Apps
    A1[iOS Flutter]-- REST/WS -->GW
    A2[Android Flutter]-- REST/WS -->GW
  end
  GW[API Gateway]-->GQL(GraphQL BFF)
  GQL-->Auth(Auth Svc)
  GQL-->Skill(Skill Svc)
  GQL-->Chat(Chat Svc)
  Auth-->PG[(Postgres)]
  Skill-->PG
  Chat-->Redis[(Redis Streams)]
  Notif[Worker]-->FCM[FCM/APNs]
  Moderation-->AdminWeb[Vue Admin]


## 10  Data Model (Core)

```text
users(id PK, email UNIQ, password_hash, display_name, avatar_url,
      lat NUMERIC(9,6), lon NUMERIC(9,6), created_at, deleted_at)

skills(id PK, user_id FK, category, title, description, availability_json,
       is_active BOOL, rating_cached FLOAT, created_at)

connections(id PK, seeker_id FK, provider_id FK,
            status ENUM(pending,accepted,declined), created_at)

messages(id PK, connection_id FK, sender_id FK, ciphertext, sent_at)

feedback(id PK, connection_id FK, rater_id FK, vote BOOL, created_at)

