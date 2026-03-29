# CAA State Open Records Document Downloaders

A collection of GUI tools for bulk-downloading Clean Air Act compliance documents from state environmental agency open records portals. These tools are intended for EPA enforcement staff who need to retrieve large numbers of documents from state systems without clicking through each file individually.

---

## Tools

### KDHE — Kansas Department of Health and Environment
**Script:** `KDHE/KDHE_doc_dl.py`
**Source system:** [KEIMS (Kansas Environmental Information Management System)](https://keims.kdhe.ks.gov)

Downloads compliance documents from the KEIMS facility document portal. The tool reads a CSV export that KEIMS generates from the Documents tab on any facility page, parses the embedded download URLs, and retrieves the files in batch.

**Dependencies:** `requests` (tkinter is included with standard Python)

**Workflow:**
1. Open the target facility in KEIMS and navigate to the **Documents** tab.
2. Use the **Download as CSV** option to export the document list.
3. Run the script and load the CSV file.
4. Review the document table, select/deselect files as needed, choose an output folder, and click **Download Selected**.

---

### IDNR — Iowa Department of Natural Resources
**Script:** `IDNR/idnr_batch_downloader.py`
**Source system:** [Iowa DNR Document Search](https://programs.iowadnr.gov/docsearch/Home/Search)

Downloads Air Quality Bureau documents from the Iowa DNR document search portal. Like the KDHE tool, it reads a CSV export from the site, but because the Iowa DNR system requires resolving an internal OTCS object ID before a file can be downloaded, the script performs an additional API lookup for each document before retrieving it.

**Dependencies:** `requests`, `beautifulsoup4`

**Workflow:**
1. Navigate to the Iowa DNR Document Search portal and search for the facility or program of interest.
2. Export the results as a CSV.
3. Run the script and load the CSV file.
4. Select documents in the table and choose an output folder, then click **Download Selected**.

The script tries several search strategies to resolve each document's object ID (by Document ID, filename stem, notes field, or facility ID), falling back gracefully if a narrower search fails.

---

### NDWEE — Nebraska Department of Environment and Energy
**Script:** `NDWEE/NDWEE_doc_dl.py`
**Source system:** [ECMP Nebraska Public Access](https://ecmp.nebraska.gov/PublicAccess)

Downloads Air Quality documents from Nebraska's ECMP (Environmental Compliance Management Portal) public access system. Unlike the Kansas and Iowa tools, this script does not require a pre-downloaded CSV — it queries the ECMP API directly based on search parameters you enter in the GUI.

**Dependencies:** `requests` (tkinter is included with standard Python)

**Workflow:**
1. Run the script.
2. Enter the **Facility Number**, optional date range (YYYY-MM-DD), and output folder.
3. Click **Search & Download**. The tool queries the ECMP API, shows a confirmation dialog with the number of documents found, and proceeds to download on confirmation.

> **Note:** ECMP results may be truncated for facilities with a large number of records. If you see a truncation warning, narrow the date range and run multiple batches.

---

## Requirements

| Tool | Python | Dependencies |
|------|--------|--------------|
| KDHE | 3.8+   | `pip install requests` |
| IDNR | 3.8+   | `pip install requests beautifulsoup4` |
| NDWEE | 3.8+  | `pip install requests` |

tkinter is bundled with standard Python installations on Windows and macOS. On Linux it may need to be installed separately (`python3-tk`).

---

## General Notes

- All three tools include a small delay between downloads to avoid hammering state servers.
- Files are saved with sanitized filenames derived from the document metadata. If a filename already exists in the output folder, a numeric suffix is appended to avoid overwrites.
- These tools access only publicly available open records portals and do not require login credentials.
- All tools are GUI-based and do not require command-line arguments — just run the script with Python.

---

## Repository Structure

```
├── KDHE/
│   └── KDHE_doc_dl.py          # Kansas KEIMS batch downloader
├── IDNR/
│   └── idnr_batch_downloader.py # Iowa DNR batch downloader
├── NDWEE/
│   └── NDWEE_doc_dl.py         # Nebraska ECMP bulk downloader
└── README.md
```
