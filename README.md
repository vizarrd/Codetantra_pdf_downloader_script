# CodeTantra Answer Script PDF Exporter

Export your visible CodeTantra answer script pages into a single PDF directly from your browser.

> [!IMPORTANT]
> This tool only exports answer script pages that are already visible and accessible to the currently logged-in user.

---

# Features

- Export all answer script pages into a single PDF

---

# Requirements

- Google Chrome / Microsoft Edge
- Access to your answer script in CodeTantra
- Internet connection (to load jsPDF library)

---

# Step 1: Open Your Answer Script

1. Login to CodeTantra
2. Open the answer script viewer
3. Verify that all pages are visible through the Page Navigation

Example:

```text
Page: 1 / 42
```

---

# Step 2: Open Browser Developer Tools

Press:

```text
F12
```

or

```text
Ctrl + Shift + I
```

Developer Tools will open.

---

# Step 3: Open the Console Tab

Inside Developer Tools:

1. Click **Console**
2. Make sure the Console prompt (`>`) is visible

Example:

```text
>
```

---

# Step 4: Allow Pasting

Some browsers display:

```text
Warning:
Don't paste code into the DevTools Console...
```

If prompted, type:

```text
allow pasting
```

and press Enter.

---

# Step 5: Load jsPDF Library

Paste the following code into the Console and press Enter:

```javascript
const s = document.createElement('script');
s.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js';
document.head.appendChild(s);

s.onload = () => console.log('jsPDF loaded');
```

Wait until you see:

```text
jsPDF loaded
```

---

# Step 6: Verify jsPDF Loaded Successfully

Run:

```javascript
typeof window.jspdf
```

Expected output:

```text
"object"
```

If you see:

```text
undefined
```

refresh and repeat Step 5.

---

# Step 7: Go to Page 1

Before exporting:

- Navigate to the first page of the answer script
- Ensure the viewer displays:

```text
Page: 1 / N
```

where N is the total page count.

---

# Step 8: Export All Pages Into a PDF

Paste the following script into the Console and press Enter.

Update:

```javascript
const TOTAL_PAGES = 42;
```

to match your answer script.

```javascript
(async () => {
    const { jsPDF } = window.jspdf;

    const TOTAL_PAGES = 42;
    const pdf = new jsPDF("p", "mm", "a4");

    const sleep = ms => new Promise(r => setTimeout(r, ms));

    for (let page = 1; page <= TOTAL_PAGES; page++) {

        await sleep(1500);

        const canvas = document.querySelector("canvas");
        const imgData = canvas.toDataURL("image/jpeg", 0.95);

        if (page > 1) pdf.addPage();

        pdf.addImage(
            imgData,
            "JPEG",
            0,
            0,
            210,
            297
        );

        console.log(`Captured page ${page}`);

        if (page < TOTAL_PAGES) {

            const nextBtn =
                document.querySelector(".nextPage-scannedPdf");

            if (!nextBtn) {
                console.error("Next button not found");
                break;
            }

            nextBtn.click();

            await sleep(2500);
        }
    }

    pdf.save("Answer_Script.pdf");

    console.log("Export completed");
})();
```

---

# Step 9: Wait For Export To Complete

The script will:

1. Capture Page 1
2. Move to Page 2
3. Capture Page 2
4. Continue until the last page
5. Generate a PDF

Console output:

```text
Captured page 1
Captured page 2
Captured page 3
...
Captured page 42
Export completed
```

---

# Step 10: Download the PDF

Once complete:

```text
Answer_Script.pdf
```

will automatically download.

---

# Browser Download Permission

Some browsers block automatic downloads.

If you see:

```text
This site is trying to download multiple files
```

Click:

```text
Allow
```

or

```text
Always allow downloads
```

depending on your browser.

---

# Troubleshooting

## PDF Contains Same Page Repeated

Increase render delay:

Replace:

```javascript
await sleep(2500);
```

with:

```javascript
await sleep(5000);
```

---

## Export Stops Midway

Possible reasons:

- Network interruption
- Page render delay
- Browser tab became inactive

Try:

- Keeping the tab active
- Increasing sleep delay

---

## jsPDF Not Loading

Verify internet access.

Re-run:

```javascript
const s = document.createElement('script');
s.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js';
document.head.appendChild(s);
```

---

# How It Works

CodeTantra renders answer script pages inside an HTML Canvas.

This exporter:

1. Reads the Canvas content
2. Converts each page to an image
3. Inserts each image into a PDF
4. Navigates pages automatically
5. Downloads a single combined PDF

---

# Disclaimer

This project is intended for exporting answer script pages that are already accessible to the authenticated user.

Users are responsible for complying with:

- Institutional policies
- Examination regulations
- CodeTantra Terms of Service

The author is not responsible for misuse of this tool.

---

# License

MIT License
