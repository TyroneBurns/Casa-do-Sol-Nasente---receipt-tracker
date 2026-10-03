# Casa do Sol Nasente

A simple, mobile-friendly build spending and receipt tracker. No account, subscription, build tools or API keys required.

## Launch on GitHub Pages

1. Create a GitHub repository called `casa-do-sol-nasente` (a public repository is the simplest option).
2. Upload `index.html` from this folder to the repository root and commit it to `main`. You can also upload this README. Do not upload your receipts or backup files.
3. Open repository **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**, then **main** and **/(root)**. Click **Save**.
5. When deployment finishes, open the website link shown on that page.

Official instructions: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

On iPhone, open the site in Safari. You can use Share → Add to Home Screen for a shortcut. Use the same browser or shortcut consistently; a different browser or website address may have different saved data.

## Using the app

- Add the supplier, date, amount including VAT and category.
- Optionally attach a JPG, PNG, WebP or PDF receipt (maximum 12 MB per file). Photos can be selected using your phone's file/photo picker. Convert HEIC images to JPG first if needed.
- Tap **Take photo & scan** to open the rear camera on supported phones, or **Upload receipt** to choose an existing file. Desktops and some browsers may show a file picker instead.
- Photos are automatically read using Tesseract.js with Portuguese and English recognition. It suggests supplier, numeric date, currency, total and category. Always review the suggestions; OCR can misread blurred print, tax lines and receipt layouts. Unknown or ambiguous totals are left blank. Notes are entered manually. PDFs attach normally but are not scanned.
- Scanning happens in the browser. Internet access is required to download the scanner and language files from their CDNs; receipt photos are not sent to an OCR server. The first scan can take longer. Stop scanning or use manual entry if recognition fails.
- EUR is the default. GBP is supported and totalled separately; no exchange-rate conversion is performed.
- Edit or delete entries, view receipts and search your expenses.
- Export CSV for a spreadsheet summary. CSV does not include receipt file contents.
- Download a full JSON backup to keep both entries and receipt files. Restore merges by record ID, replacing matching records with their backup versions. Backups from separate devices are not automatic synchronisation. Deleted entries can reappear when restoring an older backup.

## Your data

Data is saved using IndexedDB on this browser/device. There is no backend, sign-in or analytics. The scanner loads Tesseract.js 5.1.1 and its worker/language resources from external CDNs. The public GitHub Pages site contains only the application code, not your locally entered receipts.

Download regular full backups and keep them somewhere safe. Clearing browser data, private browsing, browser storage eviction or device loss can remove records. Browser storage capacity varies. A failed save displays an error; do not assume the receipt was saved. Backup import is limited to 200 MB per file. If your archive approaches this size, keep the downloaded archive and seek an expanded storage version before relying on restore.

Anyone who can access your unlocked browser can access the records. Backups contain your receipts and are not encrypted. Do not commit backups or receipts to GitHub.

Keep this app on its own origin if you need isolation from other web apps: GitHub project Pages sites under the same username share a browser origin. Other scripts on that origin may access browser storage. Changing the site address can make existing data unavailable until restored from backup.

The site needs internet to load; this version does not include offline app caching.

## Development

The complete app is in `index.html`, with no install or build step. The scanner requires an internet connection. Serve it with a local static HTTP server for development. Test changes before replacing the live file, and download a backup first. Monetary amounts are stored as integer cents/pence to avoid floating-point addition errors. Imports are validated before a single atomic storage transaction.

## Updating an existing installation

Download a full backup first, then replace only `index.html` in the same repository and keep the same Pages address. The database name and saved record format are unchanged, so existing entries should remain in that browser. Refresh the live page after GitHub finishes deployment. Camera capture should be checked on your actual phone; native camera/file-picker behaviour varies by browser.

Validation: JavaScript syntax and receipt-parser checks passed for Portuguese and English totals, tax/change exclusion, decimal/thousands formats, ambiguous totals and invalid dates. End-to-end browser/camera and real-image OCR tests could not be run in the build environment.
