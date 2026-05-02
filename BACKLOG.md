# Rental Tracker Backlog

Status: ✅ Done · 🟡 In progress · ⬜ Not started · ⏸ Blocked

Last updated: 2026-05-02

---

## Phase 1 — Local app foundation ✅
## Phase 2 — Excel exports + monthly P&L ✅
## Phase 3 — Tenant rent roll + design refresh ✅
## Phase 3.1 — Management fee module ✅

*(Details consolidated; full history available in git.)*

---

## Phase 5 — Tax-prep depth ✅ COMPLETE

### New Tax Setup tab
- ✅ App-level config: configurable mileage rate (default $0.67/mi for 2026), lease expiration warning threshold (default 90 days)
- ✅ Per-property cards with sections for: Mortgage Interest 1098 · Depreciation · 1099-MISC contractors · PAL carryforward

### I1 — Mortgage interest from Form 1098 ✅
- ✅ Per-property, per-year entry (current year + prior 2 years shown by default)
- ✅ Auto-feeds Schedule E Line 12 in P&L Statement and Schedule E Excel export
- ✅ Spread evenly across 12 months for monthly P&L display

### I2 — Depreciation calculator ✅
- ✅ 27.5-year straight-line residential rental, mid-month convention
- ✅ Inputs: purchase price, land allocation %, in-service date
- ✅ Live preview: building basis · annual full-year SL · current-year deduction (partial year if first/last)
- ✅ Auto-feeds Schedule E Line 18

### I3 — Mileage log ✅
- ✅ "Log Mileage" added as 3rd option in Add Transaction picker
- ✅ Form: date, miles, purpose, property, units, rate (defaults from app config)
- ✅ Computed deduction shown live as you type
- ✅ Saves as Auto/Travel expense (Schedule E Line 6) with `mileage` tag

### I4 — Bank/CSV import + auto-categorize ✅
- ✅ CSV upload from Settings → Bank/CSV Import card
- ✅ Auto-detects Date / Amount / Description columns (handles common bank formats)
- ✅ Auto-classifies expense vs revenue from amount sign
- ✅ Heuristic auto-category guess from description (ComEd→Utilities, Home Depot→Repairs, etc.)
- ✅ Staging table: review each row, edit category/property, deselect to skip
- ✅ Bulk import; tagged `bank-import` so they're identifiable later

### I5 — 1099-MISC contractor tracking ✅
- ✅ Flag vendor names per property as contractors
- ✅ YTD payment tracking auto-computed from expense transactions matching vendor name
- ✅ Visual chip turns amber + adds "1099 due" label when total ≥ $600
- ✅ Excel export: per-contractor totals + 1099-required flag + transaction-level detail

### I6 — Capital improvements vs repairs flag ✅
- ✅ Toggle on manual expense form (and ready to wire on receipt review): "Capital improvement"
- ✅ When true, expense is **excluded** from current-year P&L deductions
- ✅ Capitalized improvements expected to flow into depreciation schedule (manual for now — automated capitalize-to-schedule is Phase 7)

### I7 — 50% meals deduction rule ✅
- ✅ Toggle on manual expense form: "50% meals deduction"
- ✅ When true, half the amount is deducted in P&L and Schedule E export

### I8 — PAL §469 loss carryforward ✅
- ✅ Walks every prior year's transactions, computing NOI per property per year
- ✅ Negative NOI accumulates as carryforward; positive NOI absorbs the carryforward
- ✅ Surfaced on each property's Tax Setup card with disclaimer about §469(i) $25K allowance and AGI phase-outs
- ✅ Shown as informational line at bottom of Schedule E Excel export

---

## Phase 6 (selected) — Tenant management depth ✅ COMPLETE

### G14 — Multi-month bulk "Mark all paid" ✅
- ✅ "⚡ Mark YTD paid" button on each tenant row in Rent Roll
- ✅ Marks all unpaid YTD months paid in one action (skips already-paid; respects lease dates)
- ✅ Confirmation dialog lists exact months and total amount before posting

### G15 — Tenant history on tenant change ✅
- ✅ When a tenant name is replaced or unit marked vacant, old tenant pushed to `unit.tenant_history[]` with `archived_at` timestamp
- ✅ Past tenants displayed at bottom of tenant drawer when editing
- ✅ Lease + rent + dates preserved for historical reference

### G16 — Lease expiration reminders banner ✅
- ✅ Computes leases ending within configurable threshold (default 90 days)
- ✅ Banner displayed at top of Add Transaction tab listing up to 3 expiring leases with days-out
- ✅ Click "Open Rent Roll" to jump to that view

### G17 — Auto-recurring rent generation ✅
- ✅ "⏳ Generate this month's rent" button in Rent Roll header
- ✅ Creates pending revenue entries for every occupied unit, dated on rent due day
- ✅ Tagged `pending` and `rent-roll`; status='pending' on the record
- ✅ Skips months where rent is already recorded (idempotent)
- ✅ Respects lease dates; uses rent_history for the right rent amount

### G19 — Rent increase tracking (rent_history) ✅
- ✅ `tenant.rent_history[]` array stores `{ effective_date, monthly_rent }` entries
- ✅ Editor in tenant drawer to add/remove rent increases
- ✅ Rent roll calculations use the rent applicable for each month based on effective dates
- ✅ Auto-recurring rent + bulk-mark-paid both honor rent history

---

## Verified by smoke tests (2026-05-02)

End-to-end:
- ✅ Tax Setup tab loads with all 4 sections per property
- ✅ Depreciation math: $500K × 80% / 27.5 = $14,545.45 annual; mid-2024 in-service = $7,878.79 (6.5 mo / 12)
- ✅ Mortgage 1098 stored per year, fed into P&L Line 12 + Schedule E export
- ✅ Mileage transaction kind: 50 mi × $0.67 = $33.50 in Auto/Travel (L6)
- ✅ Capital improvement: $5K Repairs marked capital → **excluded** from P&L (correct)
- ✅ 50% meals: $100 meal → $50 in P&L (correct)
- ✅ Total expenses with all features: $33.50 + $200 + $50 + $8,500.50 + $14,545.45 = $23,329.45 ✓
- ✅ 1099-MISC: contractor with $750 payments → "YES, required" in export
- ✅ NOI 2026 with full deductions: -$24,079 → carryforward into 2027 = $24,079
- ✅ CSV import: 4 rows parsed, ComEd→Utilities, State Farm→Insurance, Home Depot→Repairs auto-detected
- ✅ Lease expiration banner: tenant with 60-day lease end detected
- ✅ Rent history: Jan rent = $1,500, Apr+ rent = $1,700 after rent_history change
- ✅ Tenant history: replaced tenant archived to `tenant_history[]`
- ✅ Bulk "Mark YTD paid", auto-recurring rent, lease banner all functional

---

## Phase 4 — PDF reports (deferred per user)
- ⬜ H1 — Schedule E PDF per property
- ⬜ H2 — Year-end P&L PDF per property
- ⬜ H3 — Per-receipt PDF
- ⬜ H4 — Tenant statement PDF
- ⬜ H5 — Schedule E "binder pack" PDF
- ⬜ H6 — Print stylesheet

## Phase 6 — Remaining items (not selected this round)
- ⬜ G13 — Auto-late-fee creation when rent crosses grace period
- ⬜ G18 — Tenant document attachments (lease PDF, ID)
- ⬜ G20 — Security deposit ledger (held / refunded / forfeited)
- ⬜ G21 — Vacancy days tracker (gaps between leases → estimated lost rent)
- ⬜ G22 — Tenant payment portal (stretch)

## Phase 7 — Automated linkages
- ⬜ Auto-flow capital improvements into a multi-year depreciation schedule (improvement basis → 27.5y SL component)
- ⬜ Receipt review: add capital improvement + 50% meals toggles
- ⬜ Receipt review: add property + unit picker (currently only manual form has these)

## Known limitations / future polish
- ⬜ Edit existing transactions inline (currently delete + re-add)
- ⬜ Search across all years
- ⬜ Bulk delete / multi-select in ledger
- ⬜ Receipt image migration from old Google Drive URLs (legacy `imageUrl`)
- ⬜ Dark-mode polish on accent colors
- ⬜ Mobile bottom-nav for the Tax Setup tab (currently only top tabs)
