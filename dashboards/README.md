# Detection Engineering Dashboard

`detection_engineering_dashboard.xml` is a Splunk Simple XML dashboard that implements the twelve panels in the Detection Engineering layout.

## Install

1. In Splunk, open the target app and upload the CSV files in `lookups/` as lookup files. Keep the filenames unchanged.
2. Create a new dashboard from source and paste in `detection_engineering_dashboard.xml`.
3. Save it as **Detection Engineering**.

The dashboard uses `inputlookup`, so it works directly from the exported Phase 2 detection results rather than relying on live indexed data.

## Signature data note

The supplied `sig.csv` is preserved as raw Suricata evidence, but it contains no `alert.signature` column. Panel 4 therefore uses `Top_IDS_Signatures.csv`, the CSV in the same ZIP that contains the summarized IDS signature counts.
