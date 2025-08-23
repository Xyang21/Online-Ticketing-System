# Negative Test Cases — Online Event Ticketing System

This file lists deliberately failing/edge scenarios to verify robustness, validation, and security hardening.  
**Status** defaults to **Not Run** until executed.

---

## Authentication & Session

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-01 | Auth | Reject invalid password | User exists | `username=admin`, `password=wrongpass` | Error “Invalid credentials”; no session created | Not Run |
| N-02 | Auth | Reject locked account login | Account flagged locked | Valid credentials for locked account | Error “Account locked” + guidance | Not Run |
| N-03 | Auth | Rate-limit brute force | None | 10 rapid failed logins from same IP | Temporary lock / CAPTCHA after threshold | Not Run |
| N-04 | Auth | Expired session handling | Session older than TTL | Navigate to checkout | Forced re-auth; no data loss message | Not Run |
| N-05 | Auth | CSRF protection on logout | Logged in | Cross-site forged logout request | CSRF token missing → request rejected | Not Run |

---

## Search & Filtering

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-06 | Search | Handle empty results | Events exist | Category with no events | “No events found” state; no errors | Not Run |
| N-07 | Search | Block injection patterns | None | `name="Music' OR '1'='1"` | Input sanitized; no SQL error; safe results | Not Run |
| N-08 | Search | Validate date range | None | End date < start date | Validation error; no query executed | Not Run |

---

## Seat Selection

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-09 | Seats | Prevent selecting sold seat | Seat already sold | Select seat A1 | Error “Seat unavailable”; UI refreshes | Not Run |
| N-10 | Seats | Prevent double-reserve race | Parallel users | Two users pick A2 simultaneously | Only first succeeds; second gets conflict | Not Run |
| N-11 | Seats | Block invalid seat ID | None | Seat “Z99” (not in map) | Validation error; no cart add | Not Run |

---

## Cart & Checkout

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-12 | Cart | Block negative quantity | Item in cart | Set qty = -1 | Validation error; qty not updated | Not Run |
| N-13 | Cart | Prevent stale cart checkout | Seat released | Proceed to checkout | Error “Seat expired”; back to selection | Not Run |
| N-14 | Cart | Detect tampered price | Devtools modified price | Submit altered total | Server-side reprice → reject mismatch | Not Run |

---

## Payment

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-15 | Pay  | Declined card path | Gateway alive | Declined test card | Clear error; order remains Unpaid; retry allowed | Not Run |
| N-16 | Pay  | Expired card | None | Expired expiry date | Validation error; no gateway hit | Not Run |
| N-17 | Pay  | Network timeout | Gateway slow | Valid card; induce timeout | Graceful timeout; retry CTA; no double charge | Not Run |
| N-18 | Pay  | Duplicate submission | Rapid double-click | Two POSTs | Idempotency: only one charge/order | Not Run |

---

## QR & Email

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-19 | QR   | Reject duplicate QR generation | Paid order already has QR | “Generate QR” again | Existing QR reused; no duplicate; audit log | Not Run |
| N-20 | Email| Handle invalid email | User email malformed | Complete payment | Email not sent; bounce logged; UI shows download link | Not Run |

---

## Admin (Events, Capacity, Refunds)

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-21 | Admin| Required field validation | None | Publish event w/o date | Validation error on date | Not Run |
| N-22 | Admin| Over-capacity prevention | Capacity=80, sold=80 | Increase sales to 81 | Operation blocked; error shown | Not Run |
| N-23 | Admin| Refund invalid ID | None | Refund `order=BAD-ID` | Error “Order not found”; no side effects | Not Run |

---

## Scanning & Security

| ID   | Area | Objective | Precondition | Input | Expected Result | Status |
|------|------|-----------|--------------|-------|-----------------|--------|
| N-24 | Scan | Prevent reused ticket | Ticket status=Scanned | Scan same QR again | Blocked; incident logged; no entry | Not Run |
| N-25 | Sec  | Block unsigned QR token | None | Tampered/unsigned QR | Invalid signature → reject | Not Run |

---

## Execution Notes

- Update **Status** to **Pass/Fail/Blocked** after runs and capture evidence (screenshots/log IDs) in `/testing/test_evidence/`.
- Record defects with IDs (e.g., **D-N-14**) and link them back to these cases.

## Suggested Repository Placement

