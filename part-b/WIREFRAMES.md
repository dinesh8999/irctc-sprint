# IRCTC Wireframes — Part B
**Mid-fidelity wireframes for all 6 feature solutions**

---

## Wireframe 1: Tatkal Virtual Queue Screen (Mobile & Desktop)

### Problem Solved
Problem 1: Tatkal Booking Crashes at 10:00 AM — provides queue visibility and prevents cascade failures

### MOBILE WIREFRAME (375px width)

```
┌─────────────────────────────────┐
│ ← IRCTC          [Profile]      │  Header
├─────────────────────────────────┤
│                                 │
│  TATKAL BOOKING QUEUE           │
│  ═══════════════════════════════│
│                                 │
│  YOUR POSITION IN QUEUE         │
│ ┌─────────────────────────────┐ │
│ │  #4,281                     │ │
│ │  Estimated wait: ~9 minutes │ │
│ └─────────────────────────────┘ │
│                                 │
│  PROGRESS                       │
│ ████████░░░░░░░░░░  85/400     │ Users ahead released
│                                 │
│  QUEUE DETAILS                  │
│ ├─ Train: 12622 Tamil Nadu Exp │
│ ├─ Class: Sleeper              │
│ ├─ Passengers: 2               │
│ └─ Date: 15-May-2026           │
│                                 │
│  ℹ️  Tips:                       │
│  • Your booking is saved        │
│  • Do not close this tab        │
│  • You'll see updates every 2s  │
│  • 90 sec to pay when called    │
│                                 │
│  [← Go Back] [Stay in Queue]    │
│                                 │
└─────────────────────────────────┘

↓ After position reaches #1:

┌─────────────────────────────────┐
│ TATKAL BOOKING QUEUE            │
├─────────────────────────────────┤
│                                 │
│  🎉 YOUR TURN IS COMING!        │
│                                 │
│  POSITION #1                    │
│  ↓                              │
│  Next → Proceed to Payment      │
│  Loading in 3 seconds...        │
│                                 │
│  PAYMENT WILL OPEN IN:          │
│ ⏱️  00 : 03                      │
│                                 │
│  Prepare these items:           │
│  ✓ Payment method ready         │
│  ✓ Phone nearby (for OTP)       │
│  ✓ Seats confirmed              │
│                                 │
│  [← Cancel & Leave Queue]       │
│                                 │
└─────────────────────────────────┘
```

### DESKTOP WIREFRAME (1024px width)

```
┌──────────────────────────────────────────────────────┐
│ ← IRCTC Logo        [Search Trains]  [Profile] [Help]│
├──────────────────────────────────────────────────────┤
│                                                      │
│  TATKAL BOOKING QUEUE — 10:03 AM                    │
│  ═════════════════════════════════════════════════  │
│                                                      │
│ ┌────────────────────────┐  ┌──────────────────────┐
│ │  YOUR QUEUE POSITION   │  │  ESTIMATED TIME      │
│ │                        │  │                      │
│ │     #4,281             │  │   ~9 minutes         │
│ │                        │  │  (Based on server    │
│ │  Out of ~45,000 total  │  │   capacity)          │
│ └────────────────────────┘  └──────────────────────┘
│                                                      │
│ REAL-TIME PROGRESS                                 │
│ ███████████░░░░░░░░░░░░░░░░░░░  18.9% Complete   │
│ 8,500 users ahead processed | ~36,500 ahead       │
│                                                      │
│ ┌──────────────────────────────────────────────────┐
│ │ BOOKING DETAILS                                   │
│ ├──────────────────────────────────────────────────┤
│ │ Train: 12622 Tamil Nadu Express                  │
│ │ Class: Sleeper (Non-AC)                         │
│ │ Departure: 15-May-2026 at 06:05 from Chennai   │
│ │ Passengers: 2                                    │
│ │ Reserved Seats: L15, L16 (Coach A)              │
│ └──────────────────────────────────────────────────┘
│                                                      │
│ LIVE UPDATES (refreshing every 2 seconds)         │
│ 10:03:42 → Position: #4,281 | Wait: 8m 52s       │
│ 10:03:40 → Position: #4,282 | Wait: 8m 54s       │
│ 10:03:38 → Position: #4,283 | Wait: 8m 56s       │
│                                                      │
│ ℹ️  YOU'RE SAFELY IN QUEUE                          │
│ If you close this tab, your position is saved.    │
│ Return within 24 hours to resume booking.         │
│                                                      │
│                  [← Leave Queue]  [Refresh Status]  │
│                                                      │
└──────────────────────────────────────────────────────┘
```

### PAYMENT FLOW SCREEN (when position #1)

```
┌─────────────────────────────────┐
│ ← TATKAL QUEUE      [ ]  [ ]    │
├─────────────────────────────────┤
│                                 │
│  🎯 READY FOR PAYMENT           │
│  Queue Position: #1 (YOUR TURN) │
│                                 │
│  TIME REMAINING: 90 SECONDS     │
│ ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░ 65s left   │
│                                 │
│  STATUS:                        │
│ ✅ Connected to payment gateway │
│ 🟡 Sending payment...           │
│ ⏳ Awaiting bank auth...         │
│ ⏳ OTP verification...          │
│                                 │
│  "⏱️ You have 65 seconds.        │
│   Complete payment to book."    │
│                                 │
│  [Proceed to Payment Page]      │
│                                 │
│  [Cancel & Rejoin Queue]        │
│                                 │
└─────────────────────────────────┘
```

### Key Interactions Annotated
- **Real-time position updates:** Every 2-3 seconds, position decreases as users ahead complete bookings
- **Session persistence:** Closing tab = position saved for 24 hours (user can resume)
- **Auto-transition to payment:** When position reaches #1, automatically advance to payment screen
- **90-second countdown:** Strict timeout with visual countdown timer
- **Back to queue fallback:** If payment not completed in 90 seconds, return to queue with lower priority

---

## Wireframe 2: Search Results with Persistent Filters (Mobile & Desktop)

### Problem Solved
Problem 2: Search Filters Do Not Work Reliably — shows filter persistence across page refresh

### MOBILE WIREFRAME (375px)

```
┌─────────────────────────────────┐
│ ← Search      IRCTC   [Menu]    │  Header
├─────────────────────────────────┤
│ Delhi → Mumbai  15-May-2026    │  Search summary
│ [Modify Search]                 │
├─────────────────────────────────┤
│                                 │
│  FILTERS (COLLAPSIBLE)          │
│ ┌───────────────────────────────┐
│ │ ▼ CLASS                       │
│ │ ☑ Sleeper          (22 trains)│
│ │ ☐ AC 3A             (15)      │
│ │ ☐ AC 2A             (8)       │
│ │ ☐ General           (5)       │
│ └───────────────────────────────┘
│ ┌───────────────────────────────┐
│ │ ▼ SEATS AVAILABLE             │
│ │ ☑ Available Only   (12)       │
│ │ ☐ WL Ok            (28)       │
│ │ ☐ RAC Ok           (14)       │
│ └───────────────────────────────┘
│ ┌───────────────────────────────┐
│ │ ▼ DEPARTURE TIME              │
│ │ ☐ Early morning (4-8am)       │
│ │ ☑ Morning (8-12pm)  (18)      │
│ │ ☐ Afternoon (12-4pm)          │
│ │ ☐ Evening (4-8pm)             │
│ │ ☐ Night (8pm-4am)             │
│ └───────────────────────────────┘
│                                 │
│  [Clear All Filters]            │
│                                 │
├─────────────────────────────────┤
│  RESULTS: 12 TRAINS             │  ← Filtered results
│                                 │
│ 1. 12622 Tamil Nadu Exp         │
│    05:35 → 13:45 | 8h 10m       │
│    Sleeper | 12 seats | ₹2,900 │
│    [View Details]               │
│                                 │
│ 2. 12345 Express                │
│    06:15 → 14:30 | 8h 15m       │
│    Sleeper | 8 seats | ₹2,850  │
│    [View Details]               │
│                                 │
│ 3. 11045 Coromandel Exp         │
│    08:00 → 16:15 | 8h 15m       │
│    Sleeper | 5 seats | ₹3,100  │
│    [View Details]               │
│                                 │
│ ... (showing 12 trains)         │
│                                 │
│ [Load More Results]             │
│                                 │
└─────────────────────────────────┘

KEY: URL shows filters
Path: /trains?from=delhi&to=mumbai&date=2026-05-15&class=sleeper&quota=available&time=morning
```

### DESKTOP WIREFRAME (1024px)

```
┌──────────────────────────────────────────────────────┐
│ ← [Search] IRCTC  [Bookings]  [Offers]  [Help]     │
├──────────────────────────────────────────────────────┤
│ Search: Delhi → Mumbai, 15-May-2026  [Modify]      │
├─────────────────┬──────────────────────────────────┤
│  FILTERS        │  RESULTS (12 trains)             │
│  ═════════════  │  ════════════════════════════    │
│                 │                                  │
│ CLASS           │ Sort by: [Departure ▼]          │
│ ☑ Sleeper (22)  │                                  │
│ ☐ AC 3A (15)    │ 1. 12622 Tamil Nadu Express    │
│ ☐ AC 2A (8)     │    05:35 → 13:45 | 8h 10m      │
│ ☐ General (5)   │    Sleeper | 12 available      │
│                 │    ₹2,900 [Book]                │
│ QUOTA           │                                  │
│ ☑ Avail (12)    │ 2. 12345 Express               │
│ ☐ WL Ok (28)    │    06:15 → 14:30 | 8h 15m      │
│ ☐ RAC Ok (14)   │    Sleeper | 8 available       │
│                 │    ₹2,850 [Book]                │
│ TIME            │                                  │
│ ☐ 4am-8am       │ 3. 11045 Coromandel Express   │
│ ☑ 8am-12pm (18) │    08:00 → 16:15 | 8h 15m      │
│ ☐ 12pm-4pm      │    Sleeper | 5 available       │
│ ☐ 4pm-8pm       │    ₹3,100 [Book]                │
│ ☐ 8pm-4am       │                                  │
│                 │ [... 9 more trains matching ...]│
│ PRICE           │                                  │
│ ₹ 2500 — 5000   │                                  │
│ [────●────────] │                                  │
│                 │                                  │
│ [Clear Filters] │                                  │
│                 │                                  │
└─────────────────┴──────────────────────────────────┘
```

### Before/After Comparison

**BEFORE (Problem State):**
```
User selects filters → Filters don't apply → Results show 8 AC + 5 WL trains 
despite "Sleeper + Available" being selected → User refreshes page → Filters 
reset → User frustrated, manual scanning takes 15 minutes
```

**AFTER (Solution State):**
```
User selects "Sleeper" + "Available" → URL changes to 
/trains?...&class=sleeper&quota=available → Results immediately update to 
show only 12 matching trains → User refreshes page → Filters persist in URL → 
Search restored exactly as left → User completes booking in 5 minutes
```

### Key Interactions
- **Filter checkbox interaction:** Click checkbox → URL parameter added/removed → API called with new params → Results updated in 1-2 seconds
- **Page refresh:** URL query params remain → React reads params → filters re-applied → Results restored
- **Back button:** Returns to previous search with filters intact (browser history preserved)
- **Clear filters button:** Removes all filter params from URL, shows all 40 unfiltered trains

---

## Wireframe 3: Seat Selection Screen with State Persistence

### Problem Solved
Problem 3: Seat Selection Resets Randomly — shows selected seat highlighted and persisted

### MOBILE WIREFRAME (Seat Map + Form)

```
STEP 1: SEAT MAP VIEW (375px)
┌─────────────────────────────────┐
│ ← Select Seats   IRCTC  [Help]  │
├─────────────────────────────────┤
│ Train: 12622 | Class: Sleeper   │
│ Passengers: 2                   │
├─────────────────────────────────┤
│                                 │
│  SELECT SEATS (Coach A)         │
│                                 │
│  COUPE 1    COUPE 2    COUPE 3  │
│  U1   L1    U2   L2    U3   L3  │  ← Upper & Lower berths
│  ░░   🟦    ░░   ░░    ░░   ░░  │     Blue = selected (L1)
│  S1         S2         S3       │     Grey = booked
│                                 │     White = available
│  COUPE 4    COUPE 5    COUPE 6  │
│  U4   L4    U5   L5    U6   L6  │
│  ░░   ░░    ░░   ░░    ░░   ░░  │
│  S4         S5         S6       │
│                                 │
│  [Other coaches A-F below...]   │
│                                 │
│  YOUR SELECTION:                │
│  ☑ L1 (Lower Berth, Coach A)    │
│  ☐ (Select 2nd passenger seat)  │
│                                 │
│  Tips:                          │
│  • Lower berths best for elderly│
│  • Same coupe = closer to family│
│  • Side berths = aisle access   │
│                                 │
│  [← Back]  [Next → Passengers]  │
│                                 │
└─────────────────────────────────┘

STEP 2: PASSENGER FORM (after clicking "Next")
┌─────────────────────────────────┐
│ ← Passenger Details  IRCTC       │
├─────────────────────────────────┤
│                                 │
│  PASSENGER 1                    │
│  ┌─────────────────────────────┐
│  │ Name: Ramesh Kumar          │
│  │ Age: 65                     │
│  │ Gender: Male                │
│  │                             │
│  │ SEAT ASSIGNED:              │
│  │ 🟦 L1 (Lower Berth, Coach A)│  ← Seat from Redux
│  │                             │
│  │ [Change Seat]               │
│  └─────────────────────────────┘
│                                 │
│  PASSENGER 2                    │
│  ┌─────────────────────────────┐
│  │ Name: Priya Kumar           │
│  │ Age: 35                     │
│  │ Gender: Female              │
│  │                             │
│  │ SEAT ASSIGNED:              │
│  │ ☐ Auto-assign (choose)      │
│  │ 🟦 L3 (selected earlier)    │
│  │                             │
│  │ [Change Seat]               │
│  └─────────────────────────────┘
│                                 │
│  [← Back to Seats]  [Continue →]│
│                                 │
└─────────────────────────────────┘

SCENARIO: Mobile Orientation Change
→ User on seat map in portrait mode, selects L1
→ Rotates phone to landscape
→ Component remounts BUT sessionStorage hydrates Redux
→ L1 still highlighted in blue ✅ (not lost)
```

### DESKTOP WIREFRAME (Seat Map)

```
┌──────────────────────────────────────────────────┐
│ ← Select Seats for Your Booking        IRCTC     │
├──────────────────────────────────────────────────┤
│ Train: 12622 Tamil Nadu Express                 │
│ Route: Chennai → Delhi, 15-May-2026             │
│ Class: Sleeper (Non-AC), 72 berths total        │
├─────────────────────┬──────────────────────────┤
│  COACH SELECTOR     │ COACH A SEAT MAP          │
│  ═════════════════  │ ══════════════════════    │
│  ☑ Coach A (chosen) │                          │
│  ☐ Coach B          │  Coupe 1   Coupe 2  ...  │
│  ☐ Coach C          │  U1  L1    U2  L2        │
│  ☐ Coach D          │  ░░  🟦    ░░  ░░        │
│  ☐ Coach E          │  S1        S2            │
│  ☐ Coach F          │                          │
│                     │  Coupe 3   Coupe 4  ...  │
│  LEGEND             │  U3  L3    U4  L4        │
│   🟦 Selected        │  ░░  ░░    ░░  ░░        │
│  ░░ Booked          │  S3        S4            │
│  ☐ Available        │                          │
│                     │  [... all 6 coupes ...]  │
│  BERTH TYPES:       │                          │
│  U = Upper          │  SELECTED BERTH:         │
│  L = Lower          │  🟦 L1 (Lower, Coupe 1) │
│  S = Side           │  Safe for elderly ✓     │
│                     │  Aisle-side access ✓    │
│  PASSENGERS:        │                          │
│  • Ramesh (65, M)   │  [← Back] [Next →]       │
│  • Priya (35, F)    │                          │
│                     │                          │
└─────────────────────┴──────────────────────────┘
```

### Key State Flow Diagram
```
SEAT SELECTION STATE FLOW:

User clicks L1 on desktop/mobile
    ↓
Redux action dispatch: selectSeat({coachId: "A", berthId: "L1"})
    ↓ (parallel)
    ├→ Redux store updates (in-memory)
    ├→ Component re-renders (L1 highlighted blue)
    └→ sessionStorage.setItem('selectedSeat_{bookingId}', JSON.stringify({...}))
    ↓
User clicks "Next → Passenger Details"
    ↓
Passenger form component loads
    ↓
useEffect reads Redux: store.getState().seatSelection
    ↓
Form pre-fills: <input value={seatSelection.berthId} /> → "L1"
    ↓
User orientation changes (mobile) OR navigates back
    ↓
Component remounts
    ↓
useEffect reads sessionStorage → Redux re-hydrates
    ↓
Seat selection persists ✅
```

---

## Wireframe 4: Payment Status Feedback Screen

### Problem Solved
Problem 4: Payment Page Missing Real-Time Status Feedback — shows step-by-step progress

### MOBILE WIREFRAME (375px)

```
BEFORE (Problem State):
┌─────────────────────────────────┐
│ Payment                         │
├─────────────────────────────────┤
│                                 │
│        🔄 Processing your      │
│           payment...            │
│                                 │
│        (spinner spins forever)  │
│                                 │
│        (user closes tab after   │
│         5 seconds - FAIL)       │
│                                 │
└─────────────────────────────────┘

AFTER (Solution State):
┌─────────────────────────────────┐
│ Payment Confirmation  [?]       │
├─────────────────────────────────┤
│                                 │
│  PAYMENT PROCESSING             │
│                                 │
│  ✅ Connecting to gateway       │
│  ✅ Gateway connected           │
│  🟡 Sending to your bank...     │
│  ⏳ Awaiting authorization...   │
│                                 │
│  PROGRESS:                      │
│  █████░░░░░░░░░░░░  60%        │
│                                 │
│  ESTIMATED TIME:                │
│  ⏱️  ~10 more seconds           │
│                                 │
│  💡 "Don't close this page.    │
│     Your payment is being      │
│     processed."                │
│                                 │
│  [← Cancel Payment]             │
│                                 │
└─────────────────────────────────┘

↓ When OTP needed:

┌─────────────────────────────────┐
│ Payment Confirmation            │
├─────────────────────────────────┤
│                                 │
│  ✅ Connected to gateway        │
│  ✅ Sent to bank                │
│  ✅ Bank is authorizing         │
│  🟡 📱 OTP sent to +91-****7654│
│                                 │
│  ENTER OTP:                     │
│  ┌─────────────────────────────┐
│  │ [_] [_] [_] [_] [_] [_]    │
│  └─────────────────────────────┘
│                                 │
│  Didn't receive OTP?            │
│  [Resend OTP (60s)]             │
│                                 │
│  [← Cancel]  [Verify OTP →]    │
│                                 │
└─────────────────────────────────┘

↓ After OTP verification:

┌─────────────────────────────────┐
│ Payment Confirmation            │
├─────────────────────────────────┤
│                                 │
│  ✅ OTP verified                │
│  🟡 Confirming booking...       │
│                                 │
│  ⏱️  ~3 seconds remaining       │
│                                 │
│  Please wait...                 │
│                                 │
└─────────────────────────────────┘

↓ Success:

┌─────────────────────────────────┐
│ Booking Confirmed! ✅            │
├─────────────────────────────────┤
│                                 │
│  🎉 Your ticket is confirmed!  │
│                                 │
│  PNR: 1234567890                │
│  Train: 12622 Tamil Nadu Exp   │
│  Date: 15-May-2026             │
│  Passengers: 2                  │
│  Fare: ₹5,800                   │
│                                 │
│  Booking confirmation SMS sent. │
│                                 │
│  [View Ticket]  [Download PDF]  │
│                                 │
│  [Go to My Bookings]            │
│                                 │
└─────────────────────────────────┘
```

### DESKTOP WIREFRAME (Status Timeline)

```
┌──────────────────────────────────────────────────────┐
│ Payment Processing             10:04:35 AM          │
├──────────────────────────────────────────────────────┤
│                                                      │
│  STATUS TIMELINE                                     │
│                                                      │
│  ✅ 10:04:23  Connecting to payment gateway        │
│      └─→ Connected to Razorpay gateway             │
│                                                      │
│  ✅ 10:04:24  Payment gateway received request     │
│      └─→ Transaction ID: TXN_234891203             │
│                                                      │
│  ✅ 10:04:26  Sending to HDFC Bank                 │
│      └─→ Forwarded for authorization               │
│                                                      │
│  🟡 10:04:27  Awaiting bank authorization          │
│      └─→ Bank processing your request...           │
│          (This typically takes 5-10 seconds)       │
│                                                      │
│  ⏳ 10:04:31  OTP sent to +91-****7654             │
│      └─→ Enter OTP below                           │
│                                                      │
│  ⏳ (pending) Verifying OTP                        │
│                                                      │
│  ⏳ (pending) Confirming booking on IRCTC         │
│                                                      │
│  ┌──────────────────────────────────────────────┐
│  │  ENTER OTP:                                  │
│  │  [_] [_] [_] [_] [_] [_]                    │
│  │                                              │
│  │  Resend OTP expires in: 3:45 (minutes:secs) │
│  │  [Resend OTP]                               │
│  └──────────────────────────────────────────────┘
│                                                      │
│  ELAPSED TIME: 12 seconds                           │
│  ESTIMATED REMAINING: 5 seconds                     │
│                                                      │
│  ℹ️  Do not close this page. Your payment is being  │
│      processed securely through your bank.         │
│                                                      │
│  [Cancel Payment]                                   │
│                                                      │
└──────────────────────────────────────────────────────┘
```

### Key Interactions
- **Real-time updates:** Every 0.5-1 second, status advances (✅ → 🟡 → ⏳)
- **Timeout warning:** After 10 seconds, show "Still processing. Estimated: 5+ seconds" 
- **OTP prompt:** When bank returns OTP requirement, form appears
- **Automatic progression:** After OTP verified, automatically advance to booking confirmation
- **Error handling:** If any step fails, show specific error message with retry option

---

## Wireframe 5: Consolidated PNR Status Dashboard

### Problem Solved
Problem 5: PNR Status Page Missing Critical Journey Information — shows all info on one page

### MOBILE WIREFRAME (375px - Scrollable)

```
┌─────────────────────────────────┐
│ PNR Status     ← [Share] [Menu] │
├─────────────────────────────────┤
│                                 │
│  SECTION 1: JOURNEY OVERVIEW    │
│  ═════════════════════════════  │
│  PNR: 1234567890                │
│  Status: ✅ CONFIRMED           │
│                                 │
│  📍 Chennai Central → Delhi     │
│     15-May 06:05 → 16-May 14:20│
│     Duration: 32h 15m           │
│                                 │
│  👤 Ramesh Kumar (65M)          │
│     Seat: L15, Coach A          │
│     Fare: ₹2,900                │
│     Booking: 10-May             │
│                                 │
├─────────────────────────────────┤
│  SECTION 2: LIVE TRAIN STATUS   │
│  [Updated 2 minutes ago]        │
│  ═════════════════════════════  │
│                                 │
│  🚂 TRAIN 12622                 │
│  Currently at: Bangalore Sta    │
│  │                              │
│  ├─ Bangalore [12:45]  ✅       │
│  ├─ Jalarpet [16:30]   ⏳       │
│  ├─ Salem [20:15]      ⏳       │
│  └─ Delhi [14:20 +45m] ⏳       │
│                                 │
│  ⏱️  Status: Running 45 min late│
│  ⚠️  Delay trending: INCREASING │
│                                 │
│  Next stop: Jalarpet at 16:30   │
│  Time to your arrival: ~1h 40m  │
│                                 │
├─────────────────────────────────┤
│  SECTION 3: PLATFORM INFO       │
│  ═════════════════════════════  │
│  Departure Platform:             │
│  🟡 Not yet allocated            │
│  Typically shown: 1-2 hrs before │
│  Expected: Tomorrow 04:00-05:00  │
│                                 │
│  💡 You'll receive SMS when     │
│     platform is assigned.       │
│                                 │
├─────────────────────────────────┤
│  SECTION 4: YOUR SEAT            │
│  ═════════════════════════════  │
│  Coach A | Berth L15            │
│  Lower Berth (aisle-side)       │
│  ✅ Accessible for boarding     │
│                                 │
│  Nearby facilities:             │
│  🚽 Restroom: Coach B (2 back)  │
│  🍽️  Pantry: Coach D (3 ahead)  │
│  👨‍⚕️ Attendant: Coach A (same)    │
│                                 │
├─────────────────────────────────┤
│  SECTION 5: CANCELLATION POLICY │
│  ═════════════════════════════  │
│  ✅ Can cancel: Until 13-May    │
│  ⏰ Deadline: 06:05 (48 hrs)    │
│                                 │
│  Refund chart:                  │
│  • Before 48h: 90% = ₹2,610    │
│  • 24-48h: 75% = ₹2,175        │
│  • <24h: 0% = ₹0               │
│                                 │
│  [Cancel Ticket]                │
│                                 │
├─────────────────────────────────┤
│  [Notifications ▼]              │
│  [Share PNR]  [Download Ticket] │
│                                 │
└─────────────────────────────────┘
```

### DESKTOP VIEW (1024px - Multi-column)

```
┌────────────────────────────────────────────────────────┐
│ My Bookings > PNR 1234567890    [Share] [Download]    │
├────────────────────────────────────────────────────────┤
│                                                        │
│ ┌────────────────────────────┐ ┌────────────────────┐
│ │  JOURNEY OVERVIEW          │ │ LIVE TRAIN STATUS │
│ ├────────────────────────────┤ ├────────────────────┤
│ │ Status: ✅ CONFIRMED       │ │ Train: 12622       │
│ │ PNR: 1234567890            │ │ Location: Blore    │
│ │                            │ │ Status: 45m late   │
│ │ Chennai → Delhi            │ │ Trend: Increasing  │
│ │ 15-May 06:05 → 14:20 (32h) │ │                    │
│ │                            │ │ Route Progress:    │
│ │ Passenger: Ramesh (65M)    │ │ ████░░░░░░░░ 32%  │
│ │ Seat: L15, Coach A         │ │                    │
│ │ Fare: ₹2,900 | Booked: 10M │ │ Next: Jalarpet 1h  │
│ │                            │ │ Arrival: +1h 40m   │
│ └────────────────────────────┘ └────────────────────┘
│                                                        │
│ ┌────────────────────────────┐ ┌────────────────────┐
│ │  PLATFORM & SEAT           │ │ REFUND POLICY      │
│ ├────────────────────────────┤ ├────────────────────┤
│ │ Platform: Not allocated    │ │ Can cancel by:     │
│ │ ETA: Tomorrow 04:00-05:00  │ │ 13-May 06:05       │
│ │                            │ │                    │
│ │ Seat: L15 (Lower Berth)    │ │ Refund options:    │
│ │ Coach: A (Non-AC Sleeper)  │ │ • 48h+: 90%=₹2610 │
│ │                            │ │ • 24-48h: 75%=₹21 │
│ │ Facilities:                │ │ • <24h: 0%        │
│ │ • Restroom: Coach B (2)    │ │                    │
│ │ • Pantry: Coach D (3)      │ │ [Cancel] [TDR]     │
│ │ • Attendant: Coach A       │ │                    │
│ └────────────────────────────┘ └────────────────────┘
│                                                        │
│ ℹ️  Auto-refresh enabled • Last updated: 2 min ago    │
│ [Refresh Now] [Share via SMS] [Set Alerts]           │
│                                                        │
└────────────────────────────────────────────────────────┘
```

### Key Interactions
- **Auto-refresh:** Every 60 seconds, fetch fresh train status from Railway API
- **Real-time updates:** If delay changes (45m → 60m), Section 2 updates automatically
- **Platform highlight:** When platform assigned, Section 3 highlights in yellow with emphasis
- **Conditional sections:** Section 5 (WL status) only shows if booking was WL
- **One-tap actions:** Cancel, TDR, Share, Download all single-tap from dashboard

---

## Wireframe 6: Waitlist Notification Flow

### Problem Solved
Problem 6: Waitlist Confirmation Process Has No Proactive Notification — shows SMS alerts at each stage

### NOTIFICATION TIMELINE DIAGRAM

```
BOOKING TIME: 15-May 12:00 PM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

User books on WL
    ↓
SMS 1: "✅ Booking confirmed. PNR: 1234567890. You are WL 15."
         Position improves as cancellations arrive
    ↓
DAY 2: 15-May 8:00 AM
    ↓
Position improved: WL 15 → WL 8 (7 more cancellations)
    ↓
NO NOTIFICATION SENT (improvement < 5 seat threshold)
    ↓
DAY 2: 15-May 2:00 PM
    ↓
Position improved: WL 8 → WL 2 (6 more cancellations)
    ↓
SMS 2: "🚀 Queue updated! You're now WL 2. Confirmation coming soon."
         [Tap to check PNR]
    ↓
DAY 2: 15-May 4:30 PM
    ↓
Final cancellation received → CONFIRMED (WL → RAC)
    ↓
SMS 3: "🎉 URGENT! Your ticket confirmed! Status: RAC
        ⏰ CONFIRMATION DEADLINE: Today 5 PM
        [Tap to confirm now]
        After 5 PM, ticket auto-cancels."
    ↓
USER TAPS LINK 4:45 PM → Redirected to PNR page
    ↓
PNR page shows:
    ┌─────────────────────────────┐
    │ ⏱️  CONFIRM BY 5 PM TODAY!   │
    │                             │
    │ Time remaining: 15 minutes  │
    │ ████████░░░░░░░░░░ 75%    │
    │                             │
    │ Status: RAC (confirmed)      │
    │ "Click 'Confirm Booking'    │
    │  to secure your seat."      │
    │                             │
    │ [Confirm Booking Now]       │
    │                             │
    └─────────────────────────────┘
    ↓
USER CLICKS "Confirm Booking" 4:50 PM
    ↓
Backend confirms booking
    ↓
SMS 4: "✅ Booking confirmed successfully. No further action needed.
        Your ticket is now fully confirmed. PNR: 1234567890"
    ↓
JOURNEY SECURE ✅

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ALTERNATIVE: USER MISSES CONFIRMATION DEADLINE
    ↓
DAY 2: 4:45 PM
    ↓
SMS REMINDER 3: "⚠️ FINAL REMINDER: Only 15 minutes left!
                 Confirm now or your ticket expires.
                 [Tap to confirm]"
    ↓
USER DOESN'T ACT → 5:00 PM PASSES
    ↓
DAY 2: 5:01 PM
    ↓
System auto-cancels unconfirmed booking
    ↓
SMS 4: "😔 Your ticket (PNR: 1234567890) was auto-cancelled at 5 PM.
        Full refund of ₹2,900 processed. Check account within 2 hours."
    ↓
BOOKING LOST ❌ (but refund guaranteed)
```

### PNR PAGE WHEN CONFIRMATION DEADLINE ACTIVE

```
┌─────────────────────────────────┐
│ PNR: 1234567890      [Share]    │
├─────────────────────────────────┤
│                                 │
│  ⏱️  CONFIRMATION DEADLINE       │
│  ═════════════════════════════  │
│  URGENT ACTION REQUIRED         │
│                                 │
│  CONFIRM BY: 5:00 PM TODAY      │
│  ⏰ Time remaining: 14:32       │
│  ████████░░░░░░░░░░ 75%       │
│                                 │
│  Your ticket is CONFIRMED but   │
│  requires your action to secure │
│  the booking. After 5 PM, the   │
│  system will auto-cancel if you │
│  don't confirm.                 │
│                                 │
│  [CONFIRM BOOKING NOW]  [Info]  │
│                                 │
├─────────────────────────────────┤
│  STATUS DETAILS                 │
│  ═════════════════════════════  │
│  Status: RAC (Reservation       │
│          Against Cancellation)  │
│  Originally booked as: WL 15    │
│  Confirmed at: 15-May 4:30 PM  │
│                                 │
│  Passenger: Ramesh Kumar (65M)  │
│  Train: 12622 Tamil Nadu Exp   │
│  Date: 16-May-2026             │
│  Seat: To be assigned          │
│  Fare: ₹2,900                  │
│                                 │
│  [View Full Details]            │
│                                 │
├─────────────────────────────────┤
│  WHAT HAPPENS IF I CONFIRM?     │
│  Payment may be required if WL  │
│  charges differ. Takes <1 min.  │
│                                 │
│  WHAT IF I MISS DEADLINE?       │
│  Automatic refund of ₹2,900.    │
│  Takes 2 hours to process.      │
│                                 │
│  [Back to Bookings]             │
│                                 │
└─────────────────────────────────┘
```

---

## Summary: All 6 Wireframes Complete ✅

| Wireframe | Purpose | Format | Key UX Elements |
|-----------|---------|--------|-----------------|
| 1. Tatkal Queue | Queue visibility + 90-sec payment | Mobile + Desktop | Real-time position, session persist, countdown |
| 2. Search Filters | Filter persistence across refresh | Mobile + Desktop | URL state sync, filter UI, clear filters |
| 3. Seat Selection | Seat state across navigation | Mobile + Desktop | Redux + sessionStorage, highlights, form binding |
| 4. Payment Status | Payment progress feedback | Mobile + Desktop | Multi-stage status, OTP form, timeout warning |
| 5. PNR Dashboard | Unified information display | Mobile + Desktop | Real-time updates, multi-section layout, alerts |
| 6. WL Notifications | SMS notification timeline | Flow diagram | SMS prompts, confirmation deadline, auto-cancel |

All wireframes are **mid-fidelity**: grey boxes with clear labels, proper proportions, interaction annotations, and before/after comparisons showing problem → solution.
