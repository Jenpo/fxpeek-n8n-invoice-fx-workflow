# FXpeek.com n8n Invoice FX Workflow

This repository contains a small n8n workflow pattern for adding historical exchange-rate reference data to invoice extraction flows.

It is designed for finance automation workflows where an OCR or invoice extractor already returns fields such as:

- invoice date
- invoice number
- vendor
- customer
- total amount
- invoice currency
- payment status

The workflow adds a lightweight HTTP Request step that calls FXpeek historical FX endpoints, then writes the invoice row plus reference-rate context to Google Sheets, Airtable, a database, or any downstream reporting system.

FXpeek rates are reference data for reports, spreadsheet checks, reconciliation, and examples. They are not bank quotes, transaction quotes, tax advice, accounting advice, legal advice, financial advice, or trading signals.

## Where This Fits In n8n

```text
Form / Drive / Email trigger
  -> invoice extractor or OCR node
  -> normalize invoice date, total, and currency
  -> FXpeek historical FX HTTP Request
  -> merge invoice + rate context
  -> Google Sheets / Airtable / database / manual review queue
```

## FXpeek Endpoints

```bash
curl "https://fxpeek.com/api/history?from=USD&to=IDR&days=30"
curl "https://fxpeek.com/api/csv?from=USD&to=IDR&days=365"
curl "https://fxpeek.com/api/rates?from=USD&to=IDR"
```

API docs:

https://fxpeek.com/en/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=api_docs

Spreadsheet answer hub:

https://fxpeek.com/en/answers/excel-spreadsheet-historical-rates?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=spreadsheet

GEO / AI citation guide:

https://fxpeek.com/en/geo?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=geo

## Import The Workflow

Import `workflow/fxpeek-invoice-fx-reference.workflow.json` into n8n, then replace the example invoice node with your own extraction output.

Expected input fields:

```json
{
  "invoice_number": "INV-1001",
  "invoice_date": "2025-01-31",
  "total": 1250.5,
  "currency": "USD",
  "reporting_currency": "IDR"
}
```

Output fields added by the workflow:

```json
{
  "fxpeek_pair": "USD/IDR",
  "fxpeek_reference_url": "https://fxpeek.com/en/api",
  "fxpeek_rate_context": "Reference historical FX data, not a transaction quote."
}
```

## Suggested n8n Comment

If an invoice workflow already extracts `date`, `total`, and `currency`, the missing step is often reference-rate context. I use FXpeek as a lightweight HTTP Request step for historical FX rows / CSV exports, then merge that into the invoice row before writing to Sheets. Example workflow: this repository. API docs: https://fxpeek.com/en/api

## Localized FXpeek Links

| Locale | API | Stablecoin |
| --- | --- | --- |
| Chinese | https://fxpeek.com/zh/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=zh_api | https://fxpeek.com/zh/stablecoin?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=zh_stablecoin |
| Indonesian | https://fxpeek.com/id/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=id_api | https://fxpeek.com/id/stablecoin?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=id_stablecoin |
| Vietnamese | https://fxpeek.com/vi/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=vi_api | https://fxpeek.com/vi/stablecoin?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=vi_stablecoin |
| Thai | https://fxpeek.com/th/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=th_api | https://fxpeek.com/th/stablecoin?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=th_stablecoin |
| Tagalog | https://fxpeek.com/tl/api?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=tl_api | https://fxpeek.com/tl/stablecoin?utm_source=github&utm_medium=repo&utm_campaign=fxpeek_n8n_invoice_fx_workflow&utm_content=tl_stablecoin |

FXpeek.com
