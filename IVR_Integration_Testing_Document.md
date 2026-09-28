# MCube IVR Integration — Feature & Testing Document
### Naadbramha Pvt Ltd | ERPNext
**Document Version:** 1.0  
**Date:** May 2, 2026  

---

## Table of Contents

1. [What is this Integration?](#1-what-is-this-integration)
2. [New Screens & Forms Added](#2-new-screens--forms-added)
3. [Changes to Existing Screens](#3-changes-to-existing-screens)
4. [How it Works — Simple Explanation](#4-how-it-works--simple-explanation)
5. [Testing Checklist](#5-testing-checklist)
6. [Agent Setup Guide](#6-agent-setup-guide)
7. [Frequently Asked Questions](#7-frequently-asked-questions)

---

## 1. What is this Integration?

This integration connects **MCube IVR (your phone system)** with **ERPNext** so that:

- When a **customer calls your business number (7066684448)**, the call is automatically recorded in ERPNext.
- The **agent handling the call gets a popup on their screen** showing who is calling.
- Agents can **call customers directly** from the Lead or Customer screen with one click — no need to dial manually.
- All calls are **automatically linked** to the correct Lead or Customer record.
- Managers can see **call reports, agent performance, and statistics** on a dashboard.
- Calls can be **categorized** as Franchisee Enquiry, Job Enquiry, Parcel Enquiry, or Complaint.

**In simple terms:** Every phone call in and out of your business is now tracked, logged, and visible inside ERPNext — just like how orders and invoices are tracked.

---

## 2. New Screens & Forms Added

### 2.1 New Forms

| # | Screen Name | How to Open | What it Does |
|---|------------|-------------|--------------|
| 1 | **MCube Settings** | Search "MCube Settings" in the search bar | Master settings page — turn the integration on/off, set up agents, configure phone number |
| 2 | **IVR Call Log** | Search "IVR Call Log" or go to `/app/ivr-call-log` | List of all **incoming calls** — shows caller number, agent, status (Answered/Missed/Cancelled), duration, recording |
| 3 | **IVR Outbound Log** | Search "IVR Outbound Log" or go to `/app/ivr-outbound-log` | List of all **outgoing calls** made by agents using Click-to-Call |
| 4 | **Franchisee Enquiry** | Search "Franchisee Enquiry" or go to `/app/franchisee-enquiry` | Enquiry form for franchise-related calls — caller details, city, investment budget, etc. |
| 5 | **Job Enquiry** | Search "Job Enquiry" or go to `/app/job-enquiry` | Enquiry form for job-related calls — position, experience, salary details |
| 6 | **Parcel Enquiry** | Search "Parcel Enquiry" or go to `/app/parcel-enquiry` | Enquiry form for parcel/delivery-related calls — order reference, delivery address, issue |

### 2.2 New Dashboard

| Screen | How to Open | What it Shows |
|--------|-------------|---------------|
| **IVR Dashboard** | Go to `/app/ivr-dashboard` | Total calls, answered calls, missed calls, answer rate, recent calls list, category-wise breakdown chart, agent-wise performance table |

---

## 3. Changes to Existing Screens

### 3.1 Lead Form — New Features Added

When you open any Lead record, you will now see:

| Feature | Where to Find | What it Does |
|---------|--------------|--------------|
| **Click to Call** button | Click the **"IVR"** dropdown menu at the top of the form → "Click to Call" | Calls the Lead's mobile number through MCube. Your phone will ring first, then it connects you to the Lead. |
| **Call History** button | Click the **"IVR"** dropdown menu → "Call History" | Opens a popup showing all past calls with this Lead |
| **Call History Table** | Scroll down below the form fields | Shows a table of recent calls with this Lead — type, number, agent, status, time, recording |

### 3.2 Customer Form — New Features Added

Same as Lead form — **Click to Call**, **Call History** button, and **Call History Table** are available.

### 3.3 Incoming Call Popup

When a customer calls and the call is routed to your MCube agent number:
- A **popup notification appears on your ERPNext screen** showing the caller's number
- You can click **"Open Call Log"** to go directly to the call record
- This works on any page in ERPNext — you don't need to be on a specific screen

---

## 4. How it Works — Simple Explanation

### 4.1 When a Customer Calls You (Inbound Call)

```
Customer dials 7066684448
        ↓
MCube IVR routes the call to an agent
        ↓
ERPNext automatically creates a Call Log (IVR-IN-XXXXX)
        ↓
The agent's ERPNext screen shows a popup: "Incoming Call from [number]"
        ↓
The call log shows: caller number, agent name, call status
        ↓
If the caller's number matches a Lead or Customer, it links automatically
        ↓
When the call ends, the log updates with duration and recording
```

### 4.2 When You Call a Customer (Outbound Call)

```
Agent opens a Lead or Customer form
        ↓
Agent clicks IVR → Click to Call
        ↓
MCube rings the agent's phone first
        ↓
When agent picks up, MCube connects to the customer
        ↓
An Outbound Log (IVR-OUT-XXXXX) is created automatically
        ↓
When the call ends, duration and recording are saved
```

### 4.3 How the System Knows Which Agent is Which

Each agent has a mapping in MCube Settings:

```
Agent Phone Number  →  ERPNext User Account

Example:
2523641250  →  dhanashri.yadav@naadbramha.com
9618791117  →  priyanka.raka@naadbramha.com
```

This mapping is used to:
- Show the popup to the **correct agent** (not everyone)
- Automatically fill the **agent name** on call logs
- Enable **Click to Call** (system knows which phone to ring)

---

## 5. Testing Checklist

Below is a step-by-step checklist to verify every feature is working. Go through each test in order. You only need a browser — no technical tools required.

---

### Test 1: Verify MCube Settings

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Search "MCube Settings" in the search bar and open it | The settings page opens |
| 2 | Check the "Enabled" checkbox | It should be ticked ✅ |
| 3 | Check the DID Number field | Should show `7066684448` |
| 4 | Check the API Token field | Should show dots (means token is saved) |
| 5 | Check the Webhook URL field | Should show a URL ending with `/api/method/naadb.api.ivr_webhook` |
| 6 | Scroll down to Agent Mapping table | Should show agents with their phone numbers and ERPNext user emails |
| 7 | Check "Auto Open Call Form on Incoming Call" | Should be ticked ✅ |

**Result:** PASS / FAIL

---

### Test 2: Verify IVR Call Log List

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Go to `/app/ivr-call-log` | List of call records opens |
| 2 | Check if records exist | Should show previous call records with colored status indicators |
| 3 | Open any record | Should show: Call ID, Customer Number, Agent Name, Agent Phone, Status, Timing details |
| 4 | Check if "ERP User" field is filled | For calls handled by mapped agents, this should show the agent's email |

**Result:** PASS / FAIL

---

### Test 3: Incoming Call Popup Notification

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Login to ERPNext as an agent who is in the Agent Mapping table | ERPNext opens normally |
| 2 | Keep the browser open on any ERPNext page | — |
| 3 | Ask someone to make a test call to 7066684448 that routes to your agent number | — |
| 4 | Watch your browser screen | A popup should appear saying "Incoming Call from [number]" with an "Open Call Log" button |
| 5 | Click "Open Call Log" | Should open the IVR Call Log record for this call |

**Result:** PASS / FAIL

---

### Test 4: Click-to-Call from Lead

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Login as an agent who is in the Agent Mapping table | — |
| 2 | Open any Lead that has a Mobile Number filled in | Lead form opens |
| 3 | Look for the **"IVR"** dropdown button at the top | Should show two options: "Click to Call" and "Call History" |
| 4 | Click **"Click to Call"** | A loading spinner appears, then a green message: "Call initiated! Check your phone." |
| 5 | Check your physical phone | Your phone should ring. When you pick up, it connects to the Lead's number. |
| 6 | After the call, go to `/app/ivr-outbound-log` | A new record should appear for this call |

> **Note:** This makes a real phone call. Only test when you are ready to receive a call on your phone.

**Result:** PASS / FAIL

---

### Test 5: Click-to-Call from Customer

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Open any Customer that has a phone number | Customer form opens |
| 2 | Click **IVR → Click to Call** | Same behavior as Lead — call gets initiated |

**Result:** PASS / FAIL

---

### Test 6: Call History on Lead

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Open a Lead that has received or made calls before | Lead form opens |
| 2 | Scroll down below the form fields | A section titled **"📞 Recent Call History"** with a table of calls |
| 3 | Check the table contents | Shows: Type (Inbound/Outbound), Number, Agent, Status, Time, Duration, Recording, Log link |
| 4 | Click on any Log link (e.g., IVR-IN-00045) | Opens that call log record |
| 5 | Click **IVR → Call History** from the menu | A larger popup appears with the full call history |

**Result:** PASS / FAIL

---

### Test 7: IVR Dashboard

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Go to `/app/ivr-dashboard` | Dashboard page loads |
| 2 | Look at the top section | Cards showing: Total Calls, Answered Calls, Missed Calls, Answer Rate % |
| 3 | Look at the middle section | Table of recent calls |
| 4 | Look at the charts section | Category breakdown (Franchisee, Job, Parcel enquiries) |
| 5 | Look at the bottom section | Agent Performance table — each agent's calls, answered, missed, answer rate |
| 6 | Change the date filters | Stats should update for the selected date range |

**Result:** PASS / FAIL

---

### Test 8: Create Enquiry from Call Log

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Open any IVR Call Log record | Call log form opens |
| 2 | Set the **Category** dropdown to "Franchisee Enquiry" | — |
| 3 | Save the record | Record saves with the category |
| 4 | Click **Create Linked Document** button (if available) | A new Franchisee Enquiry form opens with caller details pre-filled |
| 5 | Fill in additional details and save | Franchisee Enquiry record is created and linked to the call |

**Result:** PASS / FAIL

---

### Test 9: IVR Outbound Log List

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Go to `/app/ivr-outbound-log` | List of outbound call records |
| 2 | Check records | Should show calls made via Click-to-Call with status, agent, time |
| 3 | Open any record | Shows: Customer Number, Agent details, Duration, Recording URL |

**Result:** PASS / FAIL

---

### Test 10: Enquiry Forms

| Step | What to Do | What You Should See |
|------|-----------|-------------------|
| 1 | Go to `/app/franchisee-enquiry` | List of franchise enquiries |
| 2 | Open any record or create new | Form with: Caller Name, Number, Email, City, Investment Budget, Experience, Call Reference |
| 3 | Go to `/app/job-enquiry` | List of job enquiries |
| 4 | Open any record | Form with: Caller Name, Position Applied For, Experience, Salary, Qualification |
| 5 | Go to `/app/parcel-enquiry` | List of parcel enquiries |
| 6 | Open any record | Form with: Caller Name, Parcel Type, Order Reference, Delivery Address, Issue |

**Result:** PASS / FAIL

---

## 6. Agent Setup Guide

### What is Agent Mapping?

Agent Mapping tells ERPNext which phone number belongs to which user. Without this, the system cannot identify agents or send them notifications.

### Current Agents

| Agent Name | MCube Phone Number | ERPNext Login Email |
|------------|-------------------|-------------------|
| Dhanashri Yadav | 2523641250 | dhanashri.yadav@naadbramha.com |
| Naadbramha | 7066684448 | payal.gate@naadbramha.com |
| Priyanka Raka | 9618791117 | priyanka.raka@naadbramha.com |
| Revati Kadam | 2667783664 | revati.kadam@naadbramha.com |
| Snehal Suryavanshi | 2134567855 | snehal.suryavanshi@naadbramha.com |

### How to Add a New Agent

1. Search **"MCube Settings"** in the search bar
2. Scroll down to the **Agent Mapping** table
3. Click **"Add Row"**
4. Fill in: Agent Name, Agent Phone Number (same as registered in MCube), ERPNext User (their login email)
5. Click **Save**

### How to Remove an Agent

1. Open **MCube Settings**
2. Find the agent row in the Agent Mapping table
3. Click the **checkbox** on the left of the row
4. Click **Delete** (the trash icon)
5. Click **Save**

---

## 7. Frequently Asked Questions

**Q: Will the popup notification work if I'm on a different screen?**  
A: Yes. The popup appears on any ERPNext page as long as your browser tab is open and you are logged in as a mapped agent.

**Q: What happens if I click "Click to Call" but I'm not in the Agent Mapping?**  
A: The system will ask you to manually enter your phone number before making the call.

**Q: Can I listen to call recordings?**  
A: Yes. Open any IVR Call Log or IVR Outbound Log. If a recording exists, you will see a "Recording URL" field with a link to play it.

**Q: What happens if the same customer calls twice?**  
A: Each call creates a separate IVR Call Log record. Both will be linked to the same Lead/Customer if the phone number matches.

**Q: Will I lose my agent mapping if the app is updated?**  
A: No. The system automatically restores agent mapping after every update.

**Q: How is a call linked to a Lead or Customer?**  
A: The system matches the caller's phone number with the Mobile Number on Lead records or the Phone Number on Customer records.

**Q: Does Click-to-Call cost money?**  
A: Yes. Each Click-to-Call goes through MCube's telephony system. Charges apply as per your MCube plan.

**Q: Who can access the IVR Dashboard?**  
A: Any logged-in ERPNext user can view the dashboard. It shows overall statistics — not limited to a specific agent.

**Q: Can I filter calls by date or agent on the dashboard?**  
A: Yes. The dashboard has date range and agent filters at the top.

**Q: What do the call statuses mean?**  
A:
- **Ringing** — Call is in progress, not yet answered
- **Answered** — Call was picked up and conversation happened
- **No Answer** — Call rang but nobody picked up
- **Cancelled** — Call was disconnected before it could be answered
- **Busy** — Agent's line was busy
- **Failed** — Call could not connect due to a technical issue

---

**End of Document**
