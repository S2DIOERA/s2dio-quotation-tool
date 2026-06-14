# S2DIO ERA — CRM Full Plan

## Requirements in Simple English

---

### STAGE 1 — Lead Creation (Quick Capture)
**What:** A minimal form to capture a new lead fast.
**Fields:** Name (mandatory), Phone (mandatory), Interest → dropdown: "Book Photographer / Book Studio / Register with us"
**On Submit:**
- Lead is created instantly
- Status auto-set: **New**
- Substatus dropdown (single select): RNR (Ringing Not Responding), Follow-up, Dropped, Switched Off, Wrong Number, Want to Register

**Fix also:** The existing index.html enquiry form throws `Insufficient permissions to create new select option 'Noida'` in Airtable → fix by changing "Area" field in Airtable from Single Select → Text field (or pre-populate all city/area options).

---

### STAGE 2 — Enquiry Raised (Post Job)
**What:** When full enquiry is filled, instantly generate outreach templates.
**On Trigger:**
- WhatsApp Post Job template auto-ready (to send to photographer/studio group)
- Email draft auto-ready (internal/external)
- Status auto-changes: **New → Enquiry Raised**
- Substatus dropdown: Photographer Response Pending, Low Budget, Photographer Confirmed, Customer Pendency

**Sidebar Panel:** Show nearest matching Photographers / Studios / Creators based on city + service.

**Sharing Template:** A copy-paste WhatsApp message to send photographer's work link + quoted budget to the client.

---

### STAGE 3 — Quotation Sent
**What:** Once photographer is finalized, create quotation from the lead.
**On Trigger:**
- WhatsApp quote message auto-ready
- PDF quotation auto-generated
- Email draft auto-ready
- Status auto-changes: **Enquiry Raised → Quote Shared**
- Substatus dropdown: Customer Pendency, Negotiating, Price Issue

---

### STAGE 4 — Booking Confirmed + Advance
**What:** Mark lead as confirmed with amount + advance received.
**On Advance Received:**
- Remaining balance WhatsApp message auto-ready
- Remaining balance PDF auto-ready
- Status auto-changes: **Quote Shared → Advance Paid**

---

### STAGE 5 — Final Payment + Add-ons
**What:** Send final payment request — with option to add extra charges.
**On Trigger:**
- Final PDF with add-ons/extra charges
- WhatsApp message auto-ready
- Status auto-changes: **Advance Paid → Shoot Completed**

---

### STAGE 6 — Delivery + Review
**What:** Share Google Drive link with client. Collect review.
**On Trigger:**
- WhatsApp delivery message with Drive link
- Review request: WhatsApp + Email template auto-ready
- Status auto-changes: **Shoot Completed → Delivered → Closed**

---

## Airtable Fix (Immediate)

**Problem:** `Error: Insufficient permissions to create new select option 'Noida'`
**Root Cause:** `Area / Locality` field in Airtable `Client Leads` table is a **Single Select** field — it does not allow new options from API.
**Fix Options (choose one):**
1. Go to Airtable → Client Leads table → Area field → Change type to **Text** ✅ (easiest)
2. OR go to field settings → manually add all city/area options (Noida, Gurgaon, etc.)

---

## Creator/Studio Data Plan

**Source:** Firebase (CSV exported — creators + studios)
**Current CRM:** Uses localStorage `s2e_s2_v` (vendors) — manually added
**Target:** Import CSV data into CRM + easy single-step "Add New" flow

**Fields to store per Creator:**
- Name, Phone, Email, City, Area, Category (Photographer/Videographer/Studio), Rating, Starting Price, Portfolio Link, Instagram, Status (Active/Pending)

**Fields to store per Studio:**
- Name, Owner, Phone, City, Area, Studio Types, Price/Hour, Half Day, Full Day, Status

**Easy Add Flow:** One modal → fill 5–6 fields → saved to localStorage instantly → appears in Find Creator tab

**Future Website Sync:** All data uses a standard schema so when Firebase integration is added, same keys map directly.

---

## Implementation Checklist (Priority Order)

### 🔴 CRITICAL — Fix Now

- [ ] **#1** Fix Airtable Area field: Change Single Select → Text in Airtable dashboard
- [ ] **#2** Add `substatus` field to all leads (dropdown, single select)
- [ ] **#3** Update BSTATUS array in crm.html to match new flow stages

### 🟠 HIGH — This Week

- [ ] **#4** Quick Lead Capture: Minimal form (Name + Phone + Interest) → creates lead instantly
- [ ] **#5** Auto-set Status = "New" on lead creation
- [ ] **#6** Substatus dropdowns per stage (Stage 1-6 each has its own list)
- [ ] **#7** Post Job WhatsApp template auto-generates on enquiry fill
- [ ] **#8** Email draft auto-generates alongside WhatsApp template
- [ ] **#9** Nearest Creator/Studio sidebar panel on lead detail view

### 🟡 MEDIUM — Next Week

- [ ] **#10** Quotation → auto WhatsApp + PDF + Email (already partial — needs status auto-update)
- [ ] **#11** Advance received → remaining balance WhatsApp + PDF auto-ready
- [ ] **#12** Final payment with add-ons/extra charges option
- [ ] **#13** Delivery → Drive link sharing + Review request templates

### 🟢 ENHANCEMENT — When Ready

- [ ] **#14** Import creators CSV into CRM localStorage (one-time bulk import)
- [ ] **#15** Import studios CSV into CRM localStorage (one-time bulk import)
- [ ] **#16** Photographer share template: Copy-paste with work link + budget for client
- [ ] **#17** Review request: WhatsApp + Email auto template on delivery
- [ ] **#18** Future: Firebase sync toggle (localStorage → Firebase, same schema)

---

## Status Flow (Updated)

```
New → Enquiry Raised → Quote Shared → Negotiating → Confirmed → Advance Paid → Shoot Completed → Editing → Delivered → Closed
                                                                                                                        ↓
                                                                                                                    Cancelled (any stage)
```

---

## localStorage Schema (No Change Needed)

| Key | Data |
|-----|------|
| `s2e_s2_b` | Bookings/Leads (add `substatus` field here) |
| `s2e_s2_v` | Vendors/Creators/Studios |
| `s2e_s2_e` | Expenses |
| `s2e_s2_a` | Ads |
| `s2e_s2_t` | Targets |
| `s2e_at` | Airtable token |
