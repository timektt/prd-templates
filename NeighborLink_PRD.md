Product Requirements Document (PRD)
Product: NeighborLink – Neighbor Skill-Sharing Mobile App
Author: <your name>
Version: 1.2 (2025-05-25)
Status: Draft for stakeholder review
Last Updated: 2025-05-25
Target Launch City: <Pilot City>
Target Launch Date: Q4 2025

1. Executive Summary
NeighborLink is a cross-platform mobile application (iOS & Android) that matches neighborhood “Skill Providers” who can offer light assistance (basic tech help, pet sitting, lifting items, etc.) with nearby “Skill Seekers.” The MVP will handle discovery, matching, and secure in-app chat; all compensation, if any, happens off-platform.
Business Outcome: Increase neighborhood engagement and reduce friction in getting everyday help, while establishing a foundation for a future local services marketplace.

2. Success Metrics (OKRs)
Objective	Key Results (6 months post-launch)
O1 — Adoption	• ≥ 5 000 total installs in pilot city
• ≥ 2 000 MAU (40 % of installs)
O2 — Matching Efficiency	• ≥ 100 accepted matches
• ≥ 80 % of connection requests answered (accept / decline) within 48 h
O3 — Quality & Trust	• ≥ 90 % positive feedback (thumbs-up)
• ≤ 1 % abuse reports per interaction
O4 — Reliability	• Crash-free sessions ≥ 99.5 %
• P95 search latency ≤ 500 ms

3. Scope
3.1 In-Scope (MVP)
Area	Features
User Accounts	Email + password auth, password reset, profile photo (optional)
Provider Functions	Create/edit/delete skill listings, toggle Active/Inactive, set general availability
Seeker Functions	Category + keyword search, location-based ranking, view provider profiles, send connection requests
Matching Flow	Accept/decline, real-time push & in-app notifications, 1-to-1 chat after mutual acceptance
Feedback	One-time thumbs-up/down per interaction; aggregate score on profile
Administration	Web admin panel for category management, content moderation, user ban/unban, metrics dashboard

3.2 Out-of-Scope (Deferred)
Payments or escrow

Points/barter accounting

Public “help wanted” boards

Government ID / KYC verification

AI-driven skill suggestions

4. Stakeholders
Role	Name / Team	Responsibility
Product Owner	<PO name>	Vision, roadmap, backlog
Engineering Lead	<Eng Lead name>	Technical architecture, delivery
Design Lead	<Design name>	UX / UI research & mockups
QA Manager	<QA name>	Test plans, release certification
Legal Counsel	<Legal name>	Privacy, ToS, GDPR/PDPA compliance
Marketing Mgr	<Mkt name>	Pilot-city launch campaign

5. Personas (Condensed)
Persona	Goals	Pain Points
Emma (Provider, 28, freelance designer)	Meet neighbors, exchange light favors	Doesn’t want phone number exposed; limited free time
Mr. Somchai (Seeker, 67, retiree)	Needs help with smartphone & lifting groceries	Low tech literacy; distrusts strangers
Carlos (Both, 35, busy parent)	Occasionally needs pet sitting, can tutor Spanish	Wants “near-me” matches; prefers quick chat

6. User Stories & Acceptance Criteria
Full user-story backlog lives in Jira; key flows shown here.

6.1 Registration
US-01 As a new user, I can register with email + password so that I can sign in later.
Acceptance: Verification email sent; unverified accounts cannot search or list skills.

6.2 Provider: Add Skill
US-04 As a Provider, I can create a skill listing with (category, 80-char title, 200-char description, availability, Active flag).
Acceptance: Validation errors shown inline; listing appears in search within 30 s.

6.3 Search & Match
US-09 As a Seeker, I receive search results sorted by distance ASC, then feedback DESC.
Acceptance: Functional tests at 1 km/2 km/8 km radii; distance calc uses Haversine on stored geo hash.

6.4 Chat
US-13 Chat opens only after both parties accept.
Acceptance: Attempting to send message before acceptance returns 403.

7. Functional Requirements (Detailed)
ID	Requirement	Priority
FR-1	Server must geocode user’s selected neighborhood into latitude/longitude at sign-up.	Must
FR-2	Search endpoint /v1/skills/search supports filters: category, q, radius_km, lat, lon, limit 1-50.	Must
FR-3	Push notifications via FCM/APNs for request events; fallback to in-app inbox.	Must
FR-4	Chat uses WebSocket channel, retained 90 days, E2E-encrypted (Signal protocol).	Should
FR-5	Abuse reporting endpoint /v1/report flags user or chat; auto-hide content until moderator review.	Should

8. Non-Functional Requirements
Attribute	Target
Performance	P95 search response ≤ 400 ms @ 10 RPS
Availability	≥ 99.8 % uptime per calendar month
Security	OWASP Mobile Top 10 mitigated; penetration test passed before launch
Privacy	GDPR + Thailand PDPA compliant; data minimization applied
Scalability	50 k users, 500 concurrent chats without perf degradation
Accessibility	WCAG 2.1 AA for color, font scaling, screen-reader labels
Localization-Ready	String file separate; EN & TH for pilot

9. Technical Architecture (High Level)
java
Copy
Edit
Mobile (Flutter)
     │ REST / WebSocket
API Gateway (NGINX / Kong)
     │
GraphQL BFF  ─── Auth Service (JWT, bcrypt)
     │
 Postgres (Skills, Users, Feedback)
 Redis (presence, rate-limit)
 Firebase FCM / APNs (push)
 S3-compatible (profile images)
CI/CD: GitHub Actions → Test → Build (Fastlane) → Deploy (Play Console & App Store Connect).
Infrastructure: Kubernetes (GKE/EKS), autoscaled; Terraform IaC.

10. Data Model (Simplified)
Table	Key Fields
users	id PK, email UNIQ, password_hash, display_name, neighborhood_text, lat, lon, photo_url, thumbs_up_count
skills	id PK, user_id FK, category_code, title, description, availability_code, is_active
connections	id PK, seeker_id FK, provider_id FK, status(enum: pending/accepted/declined), created_at
messages	id, connection_id FK, sender_id FK, ciphertext, ts

Geo-queries powered by PostGIS ST_DWithin.

11. Analytics & Instrumentation
Mixpanel events: signup, skill_listed, search_executed, request_sent, request_accepted, message_sent, feedback_given

Amplitude funnel to measure drop-off from search → request → acceptance → thumbs-up.

Crashlytics for stability KPIs.

12. Compliance & Legal
Item	Detail
Terms of Service	Users must be 18+; disclaimer that app only facilitates connection, no liability for in-person interactions
Privacy Policy	Location stored as approx. lat/lon rounded to 2 decimals (~1 km)
Data Retention	Messages retained 90 days; skills auto-expire after 180 days of inactivity
Content Moderation	Zero-tolerance for harassment; three strikes → account suspension

13. Release Plan
Phase	Date	Milestone
Alpha (M-3)	Aug 2025	Core flows feature-complete; internal dog-food (20 users)
Beta (M-2)	Sep 2025	Pilot TestFlight & closed Play Store; collect UX feedback (200 users)
Launch (M-0)	Nov 2025	Public store release in pilot city
Post-Launch (+M +3)	Feb 2026	Decide on payments roadmap & second-city rollout

14. Risks & Mitigations
Risk	Probability	Impact	Mitigation
Low engagement after novelty fades	Med	High	Gamify reminders; push “Neighbors near you added new skills”
Abuse / harassment in chat	Med	High	E2E encryption + report flow + ML toxic language filter
Location privacy concerns	Low	Med	Store coarse location only; allow “Invisible mode”

15. Glossary
Term	Definition
Skill Provider	User offering help
Skill Seeker	User requesting help
Connection	Matched provider–seeker pair (request → accept)
Thumbs-Up Count	Sum of positive feedback received

16. Open Questions
Finalize “general availability” presets—do we allow custom times?

Minimum OS versions if we adopt SwiftUI/Jetpack Compose exclusively?

Acceptable radius defaults—should we adaptively widen if no results in 2 km?

Are real-time presence indicators (online/offline) essential for MVP?

Next Steps
UX team to deliver clickable Figma prototype by 2025-06-10.
Engineering to complete technical spike on E2E chat encryption by 2025-06-20.
Schedule stakeholder sign-off meeting for 2025-06-25.
