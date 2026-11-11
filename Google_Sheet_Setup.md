# Connect your website form to YOUR Google Sheet

Your sheet: https://docs.google.com/spreadsheets/d/13Us7rvHYZbRPorAPH2DEqnWsCsEAP_M9Ta9nbEtvVSM/edit

A website can't write into a sheet using only its normal share link, so you create a small "connector" once. It takes about 3 minutes, and your sheet ID is already filled in below.

## 1. Open the connector editor
Go to https://script.google.com and click **New project**. (Use the same Google account that owns the sheet.)

## 2. Paste this code
Delete everything in the editor and paste:

```js
const SHEET_ID = '13Us7rvHYZbRPorAPH2DEqnWsCsEAP_M9Ta9nbEtvVSM';

function doPost(e) {
  const ss = SpreadsheetApp.openById(SHEET_ID);
  const s = ss.getSheetByName('Enquiries') || ss.insertSheet('Enquiries');
  const d = JSON.parse(e.postData.contents);
  if (s.getLastRow() === 0) s.appendRow(['Time','Name','Email','Company','Service','Budget','Details']);
  s.appendRow([d.time, d.name, d.email, d.company, d.service, d.budget, d.details]);
  return ContentService.createTextOutput(JSON.stringify({ok:true})).setMimeType(ContentService.MimeType.JSON);
}
function doGet() { return ContentService.createTextOutput('CODE BASE form connection is working.'); }
```

Click the save icon.

## 3. Get the link
Click **Deploy > New deployment**, choose the gear icon > **Web app**.
- Execute as: **Me**
- Who has access: **Anyone**

Click **Deploy** and approve the permissions (Advanced > Go to project > Allow). Copy the **Web app URL** (it starts with `https://script.google.com/macros/s/`).
Check: open that URL in your browser. You should see "CODE BASE form connection is working."

## 4. Paste it into the website
In `index.html`, near the top, find:

```html
<meta name="enquiry-link" content="">
```

and paste the Web app URL between the quotes. Don't paste the Google Sheets link there, only the Web app URL.

## 5. Upload and test
Upload the website to your hosting (the form only sends from your own hosting, not from a preview). Submit a test enquiry and a new row will appear in an **Enquiries** tab of your sheet.

To get Excel: in the sheet choose **File > Download > Microsoft Excel (.xlsx)**.

If you change the connector code later, use **Deploy > Manage deployments > Edit > New version** so the link keeps working.
