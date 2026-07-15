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

---

## 🛏️ Accommodations & Live Calendar Sync (Card 2)
The rooms inventory management tier matches local parameters and coordinates real-time availability tracking directly on `rooms.html`.

1. **Localized Inventory Specs:** Room cards feature identical air-conditioned layouts with flexible capacity setups (1, 2, or 3 bed configurations). The primary differentiation is the in-room sink utility included in the upgraded tier.
2. **Community Perks & Logistics:** Extended documentation for common facilities including open-air rooftop laundry/hang-dry deck parameters, and neighborhood accessibility metrics (under 10-minute walking thresholds to local plaza/beaches; 5-minute transit window to regional shopping hubs via local cab providers or private motorbike hire).
3. **Live Calendar Integration:** Direct read-only Google Calendar iframe integration (`panglaoroomrentalph@gmail.com`) embedded inside a CSS aspect-ratio responsive container. This layout dynamically stretches to prevent visual breakages or overlapping on mobile devices while keeping client personal data secure.

---

## 🛵 Motorbike & Vehicle Rentals (Card 3)
The vehicle rental system on `rentals.html` segments the hostel's fleet into distinct transportation tiers to accommodate both independent explorers and groups.

1. **Two-Wheel Fleet Specifications:** Displays exact model configurations for targeted scooter classes:
   * **Honda Click 125i V1:** Optimized for fuel efficiency and light, automatic urban transit.
   * **Yamaha Mio Aerox 155:** High-performance automatic tier for enhanced power and long-distance stability.
2. **Four-Wheel Fleet Specifications:** Showcases multi-passenger transit alternatives:
   * **Ford Escape:** Premium 5-seater SUV built for luggage management and air-conditioned group transit.
   * **Suzuki Multicab:** Robust manual transmission utility platform built for high-capacity gear and group excursions.
3. **Rental Safety & Policy Guardrails:** Integrates standardized documentation tracking for verification thresholds (license verification, ID profiling, and security deposit workflows) with conversion routing driving back to the core inquiry engine.

### 📱 Mobile-First Optimization & Navigation Fixes
Optimized the global platform structure to ensure fluid responsiveness for travelers managing itineraries on mobile viewports.

* **Responsive Hamburger Architecture:** Seamless dynamic collapsing of the main navigation menu into a clean vertical touch-accessible drawer.
* **Sticky Navigation Toggle:** Desktop navigation stays pinned to viewports for quick path execution, while mobile viewports automatically release screen space dynamically when scrolling through content zones.
* **Fluid Layout Safety Nets:** Transformed standard desktop features and item blocks into responsive single-column structural grids (`1fr`) on tablet and mobile viewports to stop horizontal overflow breakages completely.

## 📝 Recent Updates (July 2026)

* **Synced Multi-Page Layouts:** Standardized the header element selectors and IDs (`#nav-menu` and `#hamburger-btn`) across all pages so global interactive logic hooks execute universally.
* **Expanded Media Breakpoints:** Shifted the main mobile responsive viewport limit up to `1024px` to cleanly catch tablets (iPads, Surface Pros, Folds) and smart displays that were previously breaking out of the desktop layout boundaries.
* **Fluid Geometry Overhaul:** Replaced rigid viewport-height (`vh`) mechanics on the hero section with a `min-height` boundary and fluid vertical padding to prevent text from overflowing on different mobile aspect ratios.
* **Eliminated Layout Horizon Artifacts:** Implemented a targeted negative-offset layout shift to smoothly yank the background image behind the mobile header border, permanently removing the peeking white line artifact.
* **Optimized Hero Image Crop:** Fine-tuned the vertical asset alignment to a precise `65%` percentage anchor. This captures the ideal visual framing—preserving both the top tropical palm leaf canopy and the lower beach shoreline, umbrellas, and lounge chairs for a premium user experience.