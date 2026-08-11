# Changelog

All notable changes to Mizizi Trace are documented here, oldest to newest.

## v0.1 — Personal impact calculator
Single-audience prototype modeled on a coffee brand's own customer-facing impact report: cups served, kg sourced, seedlings/shade-trees equivalent, Scope 3 carbon comparison.

## v0.2 — Generalised, multi-commodity, multi-market
Rebuilt as a white-label tool: configurable organisation, commodity (expanded beyond coffee to cocoa, cassava, tea, cashew, produce, and more), and a destination-market switch generating either an EU EUDR due-diligence preview or a Gulf halal/traceability preview.

## v0.3 — Standards-grounded impact reporting
Replaced fixed, single-venture-specific impact fields (e.g. shade trees) with toggleable categories — Climate, Sequestration, Circularity, Social, Water — each tagged against a real standard (GHG Protocol, IRIS+, Ellen MacArthur Foundation MCI, AWS Standard) rather than self-reported, uncited numbers.

## v0.4 — Persona architecture
Recognised that compliance-and-investor language isn't legible to a farmer or cooperative. Rebuilt around four distinct audience views — Farmer/Cooperative, Consumer, Procurement/Distributor, Stakeholder/Investor — rendering the same underlying order data differently for each.

## v0.5 — Mizizi Trace rebrand
- Renamed to Mizizi Trace, tagline "redefining who controls value creation."
- Rebranded to the MK Designs colour system (navy / yellow / teal), rebalanced toward a lighter, less navy-heavy layout.
- Added a materiality-intake step: a short domain and yes/no questionnaire that sets impact-category toggles and the cited standard per venture, rather than manual guesswork.
- Added a procurement export-document checklist (Commercial Invoice, Certificate of Origin, EUDR DDS / Halal Certificate, etc.), dynamic per market and commodity.
- Added an email-gated full-report download for Procurement and Stakeholder views.
- Fixed a `DOMTokenList` error caused by adding an empty CSS class.
- Switched all currency to USD.

## v0.6 — Personalised dashboards & bilingual reporting
- Farmer and Consumer views expanded into fuller personal dashboards: cumulative delivery/payment history, on-time-payment rate, additional plain-language impact tiles (sequestration, water) shown only when relevant.
- Added a name field for personalised greetings.
- Replaced language toggle buttons with a proper English/Kiswahili dropdown.
- Downloadable reports for Farmer and Consumer are now bilingual by default (English section followed by a clearly labeled Kiswahili/SW section), ungated.

## v0.7 — Report quality pass (matched against the original Rutasoka report)
- Consumer view rebuilt to match the richness of the original Rutasoka PDF: a "Purchase Profile" qualitative section (Quality, Origin transparency, Direct producer impact, Story worth sharing), full carbon comparison bars (not just a single avoided number), per-unit gram comparison, both km-driven *and* short-haul-flight-hours equivalents, and a pull-quote.
- Added a genuine **"Download as PDF"** option (browser print, with a dedicated print stylesheet) for Farmer and Consumer views, alongside the existing bilingual `.txt` download — verified by rendering both in a headless browser before shipping.
- Print stylesheet hides all input/navigation chrome and keeps only the branded header and report content, so the PDF export reads as a real document, not a screenshot of the app.

## Unreleased — see `docs/developer-brief.md`
Connecting `config/` CSVs to a live spreadsheet source, adding real persistence, wiring the email gate to Klaviyo, and connecting climate figures to a live emission-factor source.

## v0.8 — Fixed: downloaded reports didn't match the screen
The Stakeholder and Procurement "full report" downloads were silently dropping everything except the compliance section — climate, circularity, social metrics, and SDG tags were visible on screen but missing from the actual downloaded file. `renderEU`/`renderGulf` now return their text to the caller instead of overwriting the report directly, so Stakeholder and Procurement each assemble a complete report matching what's shown. Verified by downloading and inspecting both files directly, not just reading the code.

## v0.9 — Honest scope: Agrifood only, for now
Discovered (by actually configuring the tool as a waste recycler and inspecting the output) that the "domain" selector in the materiality intake was cosmetic — it swapped citation text but the commodity list, origin-site fields, and compliance module (EUDR/halal) are all agrifood-specific underneath. Selecting Zero Waste, Textiles, or Mobility produced a mismatched, misleading report (e.g. a plastic recycler getting a coffee HS code and a deforestation-free stamp). Rather than offer a half-working option, Zero Waste / Textiles / Mobility / Other are now disabled in the domain selector with a "coming soon" label and an explanation, until each is genuinely built out.

## v0.10 — Setup panel hidden from default view
The materiality intake ("Impact profile — set this once per venture") is now hidden by default — it's a one-time configuration step for whoever sets up the tool for a client, not something farmers, consumers, procurement contacts, or demo audiences should see or be tempted to fiddle with. Access it via the small "Venture setup" link in the footer, or bookmark the page with `?setup=1` in the URL. Note: this is a visibility convenience, not real access control — there's no login system yet (see docs/developer-brief.md for that scope).

## v0.11 — Fixed: report gate re-locking on every recalculation
Once unlocked with an email, the Procurement/Stakeholder report gate re-rendered from scratch on every recalculation (toggling an impact category, changing market, re-clicking Calculate) and silently reverted to asking for an email again, even though it had already been provided. `REPORT_UNLOCKED` and the entered email are now respected across re-renders — verified by unlocking, triggering a recalculation, and confirming the download still works without re-entering anything.

## v0.12 — Real PDF export for Procurement and Stakeholder
Procurement and Stakeholder "full report" downloads were still `.txt`-only, unlike the Consumer/Farmer views. Both now have a genuine "Download as PDF" option (via html2pdf.js, same mechanism as Consumer/Farmer) alongside `.txt`, generating a document that matches the on-screen design — climate, circularity, social/SDGs, and the compliance table, all included. The report-gate buttons are excluded from the captured PDF (marked `no-print`) so the exported document reads as a clean report, not a screenshot of the app with buttons in it. Verified by simulating the exact hide-then-capture sequence and confirming the output.

## v0.13 — Reverted PDF export back to print-based (html2pdf.js produced blank PDFs)
The v0.12 switch to a screenshot-based PDF library (html2pdf.js) failed in real-world use — it produced completely blank PDF files with no error, no console warning, nothing to catch. This could not be fully reproduced in the development sandbox (the CDN it depends on isn't reachable there), so rather than keep patching a failure mode that couldn't be directly observed, the library has been removed entirely. All four "Print / Save as PDF" buttons now use the browser's native print dialog instead — verified, in v0.7, to render every section correctly. This costs one extra manual step (choosing "Save as PDF" as the print destination) but produces real, reliable content every time, which matters more. Buttons and a short explanatory note were updated so this tradeoff is stated up front, not discovered by accident.

## v0.14 — Print colors, one-page layout, new brand logo, and real download-only depth
- **Fixed the core print bug:** backgrounds/colors were being stripped in the printed/PDF output because browsers omit background colors by default unless explicitly forced. Added `print-color-adjust: exact` so the report now prints in full Mizizi Trace navy/yellow/teal, not washed-out grayscale.
- **All four persona reports now fit on one page** (verified with real print-to-PDF rendering, not assumed) — compressed spacing, type scale, and component padding specifically for print, without touching the on-screen experience.
- **Swapped in the official Mizizi Trace logo** (navy circle, "MT" mark) in the top bar, replacing the placeholder MK Designs mark used during earlier development.
- **Added a "Methodology & Sources" block, visible only in the printed/downloaded report** — full citations (GHG Protocol, IRIS+ metric codes, Ellen MacArthur MCI, AWS Standard, EUDR/UAE Decree-Law references) plus a unique, timestamped document reference generated at print time. This content does not exist anywhere on screen, so it cannot be captured by a screenshot — it's the concrete reason to download rather than screenshot.
- Farmer view gets a plain-language equivalent ("Official Record" with reference + timestamp) rather than technical citations, consistent with keeping that view jargon-free.
- Fixed an unrelated bug introduced during this pass: an unclosed `<style>` tag was swallowing the entire page body, rendering a blank page. Caught via full end-to-end browser testing before shipping, not left in.

## v0.15 — Brand capitalization standardized
"Mizizi Trace" and "Redefining who controls value creation" are now consistently capitalized everywhere the brand name and tagline appear — top bar wordmark, footer, and every generated download (.txt and print/PDF, across all four persona views). Verified with a full-file sweep confirming zero remaining lowercase instances, not just spot-checked.
