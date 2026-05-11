# Impact vs Effort Matrix — IRCTC Sprint Prioritization

## The Matrix

|                   | Low Effort         | High Effort        |
|-------------------|--------------------|--------------------|
| **High Impact**   | Spec 2: Search Filters Persistent<br>Spec 3: Seat Selection State<br>Spec 4: Payment Status Feedback | Spec 1: Tatkal Queue<br>Spec 5: PNR Dashboard<br>Spec 6: WL Notifications |
| **Low Impact**    | (None identified in Part A problems) | (None identified in Part A problems) |

**Visual distribution:**
```
         HIGH IMPACT
              ↑
              │  🚀 QUICK WINS        🏗  MAJOR PROJECTS
              │  (2,3,4)             (1,5,6)
              │  Deploy in weeks      Deploy in months
              │
        ◄─────┼─────► EFFORT (Low → High)
              │
              │  🧩 FILL-INS         ❌ TIME SINKS
              │  (None)              (None)
              │
         LOW IMPACT
```

---

## Impact Scoring Details

### Impact Dimensions

**1. Users Affected**
- Score 1: <1M monthly | 2: 1-10M monthly | 3: 10-50M monthly | 4: 50-100M monthly | 5: >100M monthly

**2. Core Booking Flow Impact**
- Score 1: Peripheral feature | 2: Post-booking | 3: Mid-booking | 4: Payment/confirmation | 5: Critical path

**3. Severity of Consequence**
- Score 1: Frustration only | 2: Time waste (10+ mins) | 3: Money loss (<₹100) | 4: Trip missed/₹100-1000 loss | 5: Trip missed/₹1000+ loss

### Individual Problem Scores

| Problem | Users (1-5) | Core Flow (1-5) | Severity (1-5) | **Impact Total (1-15)** |
|---------|--------|---------|-----------|---|
| 1. Tatkal Queue | 5 (40L daily = 1.2B yearly) | 5 (payment final step) | 5 (₹60-360Cr loss) | **15** 🔴 |
| 2. Search Filters | 4 (8Cr monthly) | 3 (search step) | 3 (8-12% abandon) | **10** 🟡 |
| 3. Seat Selection | 4 (3.6-4.8L daily) | 4 (pre-payment) | 4 (mobile 35-40% fail) | **12** 🟡 |
| 4. Payment Status | 4 (3-3.6L daily) | 5 (payment step) | 3 (₹50-80L daily refunds) | **12** 🟡 |
| 5. PNR Dashboard | 3 (8-12Cr monthly) | 2 (post-booking) | 2 (helpline cost) | **7** 🟢 |
| 6. WL Notifications | 4 (72-90L monthly) | 4 (confirmation step) | 4 (₹40-45Cr loss) | **12** 🟡 |

---

## Effort Scoring Details

### Effort Dimensions

**1. System Components Touched**
- Score 1: Frontend only | 2: Frontend + 1 backend service | 3: Frontend + 2-3 services | 4: 4+ services | 5: Complete rewrite

**2. Infrastructure Needed**
- Score 1: None | 2: Minor config | 3: New service/DB migration | 4: New platform (Redis, Queue) | 5: Major infrastructure

**3. Risk of Breaking Existing**
- Score 1: No risk (isolated) | 2: Low risk (contained) | 3: Medium risk (affects search/booking) | 4: High risk | 5: Critical risk

**4. Railway API Dependencies**
- Score 1: No dependency | 2: Optional | 3: Required but stable | 4: Required but unstable | 5: Uncontrollable/missing

### Individual Problem Scores

| Problem | Components (1-5) | Infra (1-5) | Risk (1-5) | API Dependency (1-5) | **Effort Total (1-20)** |
|---------|--------|---------|-----------|---|---|
| 1. Tatkal Queue | 4 (frontend + payment + queue services) | 5 (Redis, WebSocket, microservices) | 4 (touches payment flow) | 2 (Railway API optional) | **15** 🔴 |
| 2. Search Filters | 2 (frontend component + API param) | 1 (no new infra) | 1 (isolated to filters) | 1 (uses existing API) | **5** 🟢 |
| 3. Seat Selection | 2 (Redux state + form component) | 1 (sessionStorage only) | 2 (touches form, low risk) | 1 (no dependency) | **6** 🟢 |
| 4. Payment Status | 2 (payment component + status events) | 2 (event streaming possible) | 2 (payment flow, but isolated) | 1 (no dependency) | **7** 🟢 |
| 5. PNR Dashboard | 3 (frontend dashboard + API aggregation) | 3 (cache layer, API integration) | 2 (read-only, low risk) | 3 (needs Railway running status API) | **11** 🟡 |
| 6. WL Notifications | 3 (WL processor + notification service) | 3 (event queue, notification DB) | 2 (WL processor exists) | 2 (Railway API optional) | **10** 🟡 |

---

## Placement Justifications

### 🚀 QUADRANT 1: Quick Wins (High Impact, Low Effort)

---

### Spec 2: Search Filters Persistence — QUICK WIN

**Impact Score: 10/15 | Effort Score: 5/20**

**Justification:**

1. **Impact:** This problem affects all 8 crore registered IRCTC users every session. When filters reset or don't work (30-40% of the time), users are forced into 10-18 minute manual scanning instead of 3-5 minute filtered search. From Part A frequency analysis: 8-12% of users abandon booking at search stage due to filter frustration. This translates to 10-12 lakh lost bookings monthly × ₹3,000 average fare = ₹30-36 crore in abandonment loss. However, it's not a complete blocker (users can still manually scan and book), so impact is "high" not "critical."

2. **Effort:** Implementing URL-based filter persistence is a pure frontend refactoring (2-3 weeks work). No infrastructure needed — existing backend search API already supports filter parameters. Effort is isolated to the React search component. Risk of breaking existing functionality is minimal (new filters don't modify existing code, just add parameters). 

3. **Outcome:** This feature should be deployed first in any sprint. ROI is immediate and measurable. Reducing search abandonment by 50% (from 8-12% to 4-6%) recovers ₹15-18 crore in bookings with just 2-3 weeks of effort. It's the highest ROI feature in the set.

---

### Spec 3: Sticky Seat Selection — QUICK WIN

**Impact Score: 12/15 | Effort Score: 6/20**

**Justification:**

1. **Impact:** On mobile (where 65% of bookings happen), seat selection resets 35-40% of the time, directly affecting 3.6-4.8L users daily. The consequence is severe: elderly passengers get unsafe berths, families are separated, disabled travelers can't ensure accessible seating. From Part A: mobile abandonment at booking stage is 50% higher than desktop due partly to seat issues. Fixing this increases mobile completion rate from ~50% to ~78%, recovering ₹20-30 crore annually. However, the problem is primarily a mobile UX issue, not universal (desktop works 88-92% of the time), so impact is "high" not "critical."

2. **Effort:** Using Redux for state management + sessionStorage is a well-understood pattern (3-4 weeks work). No new infrastructure needed. The risk is contained (seat selection is isolated from payment/main booking flow). No Railway API dependency.

3. **Outcome:** This feature has the highest impact-per-effort ratio. Mobile completion rate improvement of 28 percentage points is transformational for revenue. Should be deployed in Sprint 1 alongside Spec 2.

---

### Spec 4: Real-Time Payment Status Feedback — QUICK WIN

**Impact Score: 12/15 | Effort Score: 7/20**

**Justification:**

1. **Impact:** Currently, ₹50-80 lakh in payments are refunded daily due to users closing the tab mid-payment out of panic (no progress feedback). This is ₹18-29 crore annually. Over a year, fixing this recovers nearly ₹20 crore. The immediate cause: users see a loading spinner with no explanation and assume failure after 5 seconds. Showing step-by-step status ("Connecting... Connected... Sending to bank...") would reduce tab-closes from 25-30% to 5-8%, recovering most of the lost revenue. However, it's "only" a UX improvement to an existing flow, not a new feature, so impact is high-but-not-critical.

2. **Effort:** This is primarily a frontend UI enhancement (adding status messages + WebSocket or polling for real-time updates). Payment processing backend already has the capability; it just needs to emit status events. Event streaming is commonly available in modern systems (Kafka, or simple WebSocket). Effort is 2-4 weeks. Risk is low (status messages don't modify payment logic, just display). No Railway API dependency.

3. **Outcome:** One of the highest ROI improvements by pure financial metric. ₹20 crore recovery for 2-4 weeks of work. Deploy in Sprint 1.

---

### 🏗️ QUADRANT 2: Major Projects (High Impact, High Effort)

---

### Spec 1: Tatkal Virtual Queue System — MAJOR PROJECT

**Impact Score: 15/15 | Effort Score: 15/20** → **MOST CRITICAL**

**Justification:**

1. **Impact:** Every single day at 10:00 AM, 30-45 lakh users experience complete booking failure (502 errors, timeouts). During peak season (April-June), this translates to 40-80 lakh failed bookings daily. Lost revenue: ₹60-360 crore annually. The consequence is catastrophic: users miss essential travel, lose money, abandon IRCTC forever. This single problem has destroyed IRCTC's reputation ("Tatkal = broken") and driven users to third-party agents and competitors. Impact is **5/5 maximum**.

2. **Effort:** Implementing a virtual queue system requires:
   - Redis (in-memory queue store) — new infrastructure
   - WebSocket server for real-time updates — new component
   - Queue management microservice — new backend service
   - Payment processing modification (respect queue release rate) — backend change
   - Frontend queue waiting screen — new React component
   - This touches 4+ system components and requires new platforms. Risk is high: if queue breaks during peak traffic, could be worse than current state. Railway API dependency for payment processing. **Effort is 4-5/5 = very high.**

3. **Outcome:** This feature *must* be prioritized despite high effort. ROI justifies it: saving even 10% of the ₹60-360 crore loss = ₹6-36 crore value annually. The feature will take 6-8 weeks but its impact on brand perception and revenue is worth every minute. Assign senior engineering team.

---

### Spec 5: PNR Consolidated Dashboard — MAJOR PROJECT

**Impact Score: 7/15 | Effort Score: 11/20**

**Justification:**

1. **Impact:** This problem affects 8-12 crore users monthly (all PNR status checkers) but is not a *blocking* issue. Users can get all information by visiting 2-3 websites (IRCTC + indianrailways.gov.in + email), just with friction. Users waste 10-15 minutes, and helpline receives 50-100 lakh calls monthly with basic questions ("Where is my train?" / "What platform?"). The consequence is operational cost (helpline) and mild user frustration, not lost bookings. Impact is lower than other "high impact" problems: it's post-booking, not in the critical path, and users have workarounds.

2. **Effort:** Requires:
   - New aggregation API to pull data from Railway APIs
   - Cache layer (5-minute TTL) for API responses
   - New PNR dashboard component (frontend)
   - Dependency on Railway running status API (which may be unreliable or have limited SLA)
   - Effort is 4-6 weeks, medium-high risk (depends on Railway API stability).

3. **Outcome:** This feature improves UX and reduces helpline load (good for long-term cost) but doesn't directly recover lost revenue. Should be deprioritized vs. Specs 1, 2, 3, 4 which have direct revenue impact. However, it's still worth doing after quick wins to improve overall product polish. **Sprint 2 timeline.**

---

### Spec 6: Proactive WL Notifications — MAJOR PROJECT

**Impact Score: 12/15 | Effort Score: 10/20**

**Justification:**

1. **Impact:** Currently, 40-50% of WL confirmations are missed because users don't manually check PNR status. This results in ₹40-45 crore annual loss (missed bookings that could have been confirmed). Users miss critical travel, frustration is high. From Part A frequency analysis: 72-90 lakh WL bookings monthly; if 50% miss confirmation deadline, that's ₹40-45 crore = massive impact. However, the problem is primarily about *missed confirmations due to lack of notification*, not about WL not confirming (most WL tickets do confirm, users just don't know). So impact is high-but-not-critical (users can still manually check, unlike Tatkal where there's zero visibility).

2. **Effort:** Requires:
   - Event-driven architecture (Kafka/SNS message queue) to emit WL status change events
   - Notification service to consume events and send SMS
   - Database table to track WL events (for audit)
   - Frontend: PNR page confirmation deadline countdown
   - No new infrastructure platforms needed (most of this exists), but requires significant backend integration. Effort is 3-4 weeks.

3. **Outcome:** This feature directly recovers ₹40-45 crore in missed confirmations with high ROI (4-week effort for ₹40-45 crore value). However, it depends on successful implementation of Spec 6 AI model (which shows probability). Better deployed as a package: AI probability + proactive notifications. **Should pair Spec 6 + AI Feature in Sprint 2.**

---

## Recommended Sprint Order

### ⏱️ Sprint 1: Quick Wins (Weeks 1-6)
**Deploy: Search Filters + Seat Selection + Payment Status**

- **Week 1-2:** Search filters persistence (Spec 2) — frontend refactor, URL state, API integration
- **Week 3-4:** Seat selection state (Spec 3) — Redux setup, sessionStorage hydration, form binding  
- **Week 5-6:** Payment status feedback (Spec 4) — status event emission, UI component, WebSocket/polling

**Expected outcome:**
- Search abandonment drops from 8-12% to 4-6% → +₹15-18 crore revenue
- Mobile completion improves from 50% to 68% → +₹10-15 crore
- Payment refunds drop from ₹50-80L daily to ₹10-15L daily → +₹12-20 crore annually
- **Total recovery: ~₹37-53 crore from 6 weeks effort**

**Deployment strategy:** Canary release (10% users) for 2 days, then 100% rollout. Each feature is independent.

---

### 🏗️ Sprint 2: Major Projects Part A (Weeks 7-14)
**Deploy: Tatkal Queue (Phase 1 — Queue System without full payment integration)**

- **Week 7-10:** Tatkal queue infrastructure (Redis, WebSocket, queue management service)
- **Week 11-14:** Frontend queue waiting screen, session persistence, basic payment release

**Expected outcome:**
- Tatkal crashes reduced by 60% (from 100% failure rate to 40% failure rate) → +₹50-100 crore
- User satisfaction with Tatkal jumps from 2.1 stars to 3.5 stars
- **Phase 2 (payment integration + 90-sec timeout):** Weeks 15-20

**Deployment strategy:** Internal testing on non-peak hours first, then limited rollout during off-peak Tatkal window, then gradual rollout.

---

### 🏗️ Sprint 3: Major Projects Part B (Weeks 15-24)
**Deploy: WL Notifications + AI Feature + PNR Dashboard**

- **Week 15-16:** WL notification event infrastructure (event queue, notification service)
- **Week 17-18:** AI WL confirmation predictor model training and API server setup
- **Week 19-22:** PNR dashboard aggregation API and frontend component
- **Week 23-24:** Integration testing and rollout

**Expected outcome:**
- WL confirmations discovered on time: 90% (from 40%) → +₹20-30 crore
- AI prediction trusted by users → 25% reduction in unnecessary backup bookings → +₹5-10 crore
- Helpline volume drops 40% → +₹5-10 crore in operational cost savings
- **Total recovery: ~₹30-50 crore**

---

## Alternative Prioritization (Risk-Averse Approach)

If organization is risk-averse and prefers smaller incremental changes:

**Sprint 1:** Spec 2 (Search) + Spec 4 (Payment Feedback) only
- Smallest scope, safest changes, highest ROI
- **Weeks 1-4, ₹25-32 crore recovery**

**Sprint 2:** Spec 3 (Seat Selection)
- **Weeks 5-8, +₹10-15 crore**

**Sprint 3:** Spec 1 (Tatkal Queue Phase 1)
- **Weeks 9-16, +₹20-40 crore**

**Sprint 4:** Everything else
- Specs 5, 6, AI, Tatkal Phase 2
- **Weeks 17-30**

---

## Sprint Capacity & Resource Allocation

| Sprint | Duration | Recommended Team | Key Roles | Critical Path |
|--------|----------|------------------|-----------|--------------|
| **1 (Quick Wins)** | 6 weeks | 8-10 people | 3 FE + 2 BE + 1 QA + 1 PM | None (features independent) |
| **2 (Tatkal Queue)** | 8-10 weeks | 12-15 people | 5 FE + 4 BE + 2 DevOps + 1 QA + 1 PM | Redis setup → Queue service → Integration |
| **3 (WL+AI+PNR)** | 10 weeks | 15-18 people | 4 FE + 5 BE + 2 ML + 2 DevOps + 1 QA + 1 PM | AI model training (parallel) + Notification infra + PNR API |

---

## Success Metrics Dashboard

Track these KPIs post-deployment to validate feature ROI:

```
SPRINT 1 METRICS:
├─ Search abandonment: 8% → 4% (target: <5%)
├─ Mobile completion: 50% → 70% (target: >68%)
├─ Payment refunds: ₹65L/day → ₹15L/day (target: <₹20L/day)
└─ Feature satisfaction: 3.8+ stars

SPRINT 2 METRICS:
├─ Tatkal success rate: 40% → 75% (target: >70%)
├─ Queue system uptime: >99.5%
├─ User satisfaction with queue: 4.0+ stars
└─ Daily recovery: ₹20-40 crore (peak season)

SPRINT 3 METRICS:
├─ WL confirmation discovery: 40% → 90% (within 1 hour)
├─ Backup booking reduction: 40% → 30%
├─ AI model accuracy: >85% precision
├─ Helpline reduction: 50M calls → 30M calls monthly
└─ User satisfaction: 4.2+ stars
```

---

## Summary: Prioritization Complete ✅

| Spec | Quadrant | Sprint | Duration | ROI | Go? |
|------|----------|--------|----------|-----|-----|
| 1. Tatkal Queue | Major Project | 2-3 | 6-8 weeks | ₹50-100Cr | ✅ YES (critical) |
| 2. Search Filters | Quick Win | 1 | 2-3 weeks | ₹15-18Cr | ✅ YES (first) |
| 3. Seat Selection | Quick Win | 1 | 3-4 weeks | ₹10-15Cr | ✅ YES (first) |
| 4. Payment Feedback | Quick Win | 1 | 2-3 weeks | ₹12-20Cr | ✅ YES (first) |
| 5. PNR Dashboard | Major Project | 3 | 4-6 weeks | ₹2-5Cr (helpline) | ✅ YES (phase 2) |
| 6. WL Notifications | Major Project | 3 | 3-4 weeks | ₹20-30Cr | ✅ YES (with AI) |

**Total potential recovery: ₹119-182 crore over 6-9 months**
