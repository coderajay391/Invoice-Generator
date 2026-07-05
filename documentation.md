# Invoice Generator — Documentation

## Overview

**Invoice Generator** is a modern React (Vite) + TypeScript application to create invoices, manage them on a dashboard, and export an invoice preview to **PDF**.

The app supports:

- Invoice creation/editing
- Status tracking: **Draft / Unpaid / Paid**
- Line items with quantity + rate
- Tax and discount
- A4 preview rendering
- Local persistence (in-browser)

> Note: `html2canvas` + `jsPDF` export uses the rendered invoice preview DOM as the source.

## Features in Detail

### 1) Dashboard

- Shows all saved invoices
- Displays:
  - Invoice number
  - Client name
  - Issue date & due date
  - Calculated amount (subtotal → discount → tax → total)
  - Status badge styling
- Actions per invoice:
  - **Export PDF**
  - **Edit**
  - **Delete**

### 2) Editor

- Form fields:
  - Invoice number
  - Status (Draft/Unpaid/Paid)
  - Issue Date and Due Date
  - Currency (USD/EUR/GBP/INR)
  - Sender details (name/email/address)
  - Client details (name/email/address)
  - Line items (add/remove, edit description/qty/rate)
  - Tax Rate (%) and Discount Rate (%)
  - Notes / Terms
- Buttons:
  - **Save** (updates local storage)
  - **Export Invoice** (generates PDF from the A4 preview)
  - Back to dashboard

### 3) Invoice Preview (A4)

- Fixed A4 sizing for consistent PDF export:
  - Width: **210mm**
  - Height: **297mm**
- Summary calculations:
  - Subtotal = Σ(quantity × rate)
  - Discount = subtotal × (discountRate / 100)
  - After discount = subtotal − discount
  - Tax = afterDiscount × (taxRate / 100)
  - Total = afterDiscount + tax

## PDF Export (How it works)

When you click **Export Invoice**:

1. The current invoice is saved to local storage.
2. The A4 preview is captured using `html2canvas`.
3. The canvas is converted to an image (`canvas.toDataURL`).
4. A `jsPDF` document is created with A4 sizing.
5. The captured image is inserted and saved as `${invoice.invoiceNumber}.pdf`.

## Tech Stack

- React + Vite
- TypeScript
- TailwindCSS
- `html2canvas`
- `jsPDF`
- lucide-react (icons)

## Key Files

- `src/App.tsx`
  - Manages navigation between **Dashboard** and **Editor**.
- `src/components/Dashboard.tsx`
  - Invoice list, totals/status display, delete/edit/export actions.
- `src/components/Editor.tsx`
  - Invoice editing UI and PDF export logic.
- `src/components/InvoicePreview.tsx`
  - A4 invoice renderer + totals display.
- `src/lib/store.ts`
  - Local persistence utilities.
- `src/lib/utils.ts`
  - Currency/date formatting helpers.

## Running Locally

```bash
npm install
npm run dev
```

Open:

- http://localhost:3000

## Build

```bash
npm run build
npm run preview
```

## Troubleshooting

### PDF looks cropped or distorted

- Ensure the preview container remains fixed to **210mm × 297mm**.
- Avoid adding elements outside the preview container that could affect layout.

### Images/fonts don’t appear in PDF

- For externally hosted assets, `html2canvas` may require CORS.
- The export sets `useCORS: true`; keep image URLs CORS-accessible.
