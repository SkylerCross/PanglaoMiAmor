# 🌴 Panglao Mi Amor - Website Backend Architecture

Documentation for the integrated lead capture system and accommodation engines built into the resort website.

## ⚙️ Contact Form Data Flow
The inquiry form on `contact.html` handles dual background routing concurrently via asynchronous JavaScript (`Promise.all`). This design avoids reliance on premium third-party platform subscriptions.

1. **Email Notification Engine:** Data hits Web3Forms API to send custom-branded alerts directly to the resort's inbox with dynamic subject lines (`🌴 New Inquiry - Panglao Mi Amor`) and instant 1-click customer replies.
2. **Spreadsheet Logging Engine:** Data handles a cross-origin webhook directly into the Google Apps Script framework.

---

## 📊 Google Apps Script Architecture
To bypass standard monetization paywalls on free form processors, the spreadsheet uses a custom backend listener. 

### Deployment Verification:
* **File Location:** Google Sheet Backend -> Extensions -> Apps Script
* **Web App URL Target:** `https://script.google.com/macros/s/AKfycbxRhcMnjQixJ5A9GHDkO9e8UzPvrRtUFwXvjsf8aeEvklKjJQkEWucJ4zw-2wkU8OcbwQ/exec`
* **Access Level:** Configured to execute as `Me` with permissions assigned to `Anyone` to facilitate anonymous external web submissions.

### Active App Script Source Code:
```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    var name = data["👤 Guest Name"] || "";
    var email = data["✉️ Guest Email"] || "";
    var type = data["🛎️ Inquiry Type"] || "";
    var message = data["💬 Guest Message"] || "";
    var date = new Date();
    
    sheet.appendRow([date, name, email, type, message]);
    
    return ContentService.createTextOutput(JSON.stringify({"result":"success"})).setMimeType(ContentService.MimeType.JSON);
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({"result":"error", "error": error.toString()})).setMimeType(ContentService.MimeType.JSON);
  }
}