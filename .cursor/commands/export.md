# Export Command

Provide instructions for exporting the HTML report to PDF.

## Instructions

When the user runs `/export` or `/export pdf`:

### Display Export Instructions

```
📄 EXPORT TO PDF

Your HTML report is ready at: Q{N}-{YEAR}-report.html

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Option 1: Browser Print (Recommended)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Open Q{N}-{YEAR}-report.html in Chrome or Edge
2. Press Cmd+P (Mac) or Ctrl+P (Windows)
3. Change "Destination" to "Save as PDF"
4. Click "Save"

💡 Tip: Enable "Background graphics" for best appearance

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Option 2: Command Line (Advanced)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

If you have wkhtmltopdf installed:
wkhtmltopdf Q{N}-{YEAR}-report.html Q{N}-{YEAR}-report.pdf

If you have Chrome CLI:
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --headless --print-to-pdf=Q{N}-{YEAR}-report.pdf \
  Q{N}-{YEAR}-report.html

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Option 3: Online Converter
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Upload your HTML file to:
• https://www.ilovepdf.com/html-to-pdf
• https://cloudconvert.com/html-to-pdf

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✨ The HTML report is print-optimized and will look great as PDF!

Need to generate a report first? Run /report
```

## Behavior

- Display instructions
- Do NOT attempt to actually convert the file
- STOP and wait for user





