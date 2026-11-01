---
name: uploads
description: "Send files into Reliafy and import them: the single-use upload link and its fallbacks, inspecting workbooks, importing BlockSim / Open-PSA / Galileo / RePyability diagrams, Excel sheets, datasets and outage logs. Use whenever the user has a file to bring in."
---

# Files into Reliafy

Files (BlockSim / Open-PSA / Galileo / RePyability diagrams, Excel workbooks, CSVs of failure times or outages). Never read a file into your context to paste it while the upload link works.
1. change upload_create (purpose: rbd_import, excel, dataset, outage_log, or document for a PDF to keep; filename): a single-use URL (15 minutes) and a ready curl PUT. Run it from a shell that can read the file.
2. If the PUT can't connect or a proxy refuses the host (e.g. CONNECT 403), retry the same PUT once on url_fallback (curl_fallback): the same link on reliafy.com, slower for large files, still unused after a refused attempt.
3. If that fails too (no network), don't retry: a small text file (at most 200 KB) can be passed as text instead (change dataset_upload csv, change rbd_upload_outage_log csv, change import_rbd content for Open-PSA XML, Galileo or RePyability JSON); any other file of up to a few hundred KB (an .xlsx, a BlockSim project) goes through this connection with change upload_send_part (base64 parts 0, 1, 2… into the same upload_id, last=true with sha256 on the final one; not counted against the Free allowance), then import as usual. Anything larger: ask the user to upload it in the app (create's fallback says where).
4. get kind upload shows a workbook's sheets, columns and a guessed mapping, or a diagram file's diagrams.
5. Import with the upload_id: change import_rbd (diagrams; save=false previews with edges and cut sets; only picks diagrams by name; time_unit when the file states none), change import_excel (target dataset, or rbd_template), change dataset_upload, or change rbd_upload_outage_log.
Relay the import notes: they say what was approximated. Blocks imported without a non-repairable model (needs_model) need one before analysis: ask the user, don't invent values.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `change` action=`upload_create` (purpose, filename, size_bytes?): A single-use link (15 minutes) to send a file without pasting it: run the returned curl PUT from a shell that can read the file, then import with upload_id.
- `change` action=`upload_send_part` (upload_id, part_index, data_base64, last, sha256?): Send a file into an upload through this connection, in base64 parts, when the PUT (and url_fallback) can't get through.
- `change` action=`import_rbd` (upload_id?, content?, format?, name?, save?, only?, time_unit?): Import diagrams from a BlockSim, Open-PSA, Galileo, RePyability JSON or Reliafy Excel template file (upload_id), or small pasted text (content, at most 200 KB), saving each as an RBD (all or nothing against the plan's limit).
- `change` action=`import_excel` (upload_id, target, sheet?, mapping?, name?, rbd_id?, replace?, header_row?): Import an uploaded .xlsx: a sheet as a dataset, or Reliafy's RBD template as a diagram (rbd_id: a new copy of it; replace=true: over it).
- `change` action=`dataset_upload` (name, csv?, upload_id?, sheet?, no_header?): Save a dataset from CSV text (csv) or a file sent with change upload_create (upload_id; an .xlsx needs sheet); returns its id, columns and a preview.
- `change` action=`rbd_upload_outage_log` (rbd_id, csv?, columns?, unit?, window_start?, window_end?, date_order?, asset_map?, name?, upload_id?, sheet?): Save a real outage log (asset, start, end; blank end = still down) against one of the user's diagrams, as CSV text or an uploaded file, and return the system's observed history from it (as analyse rbd_system_history).
- `get` kind=`upload` (id): An uploaded file before importing it: a workbook's sheets with header rows, columns, row counts, sample rows and a guessed mapping (looks_like); a diagram file's diagrams and block counts; a CSV's columns.
