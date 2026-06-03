# Constitutional Bench Data Search — `index.html`

A single-page Nepali legal-data table viewer/search app for Constitutional Bench or similar Excel-to-JSON datasets. The app reads `cbdata.js` from the same folder as `index.html` and displays a searchable, sortable, copy-friendly table with dynamic columns.

---

## 1. Files required

Keep these files in the same folder:

```text
index.html
cbdata.js
README.md
```

Optional sample file:

```text
cbdata_sample_from_uploaded_excel.js
```

Rename the sample file to `cbdata.js` if you want to test the app immediately.

---

## 2. Data format for `cbdata.js`

The app supports a JSON array of objects with any number of columns and any column headings.

Example:

```js
[
  {
    "क्र.सं.": "1",
    "न्यायाधीश": "स.प्र.न्या.श्री कल्याण श्रेष्ठ मा.न्या.श्री गिरिशचन्द्र लाल ...",
    "मुद्दा": "उत्प्रेषण",
    "मुद्दा नं.": "072-WO-0294 (नि.नं. 0001)",
    "पक्षको नाम": "गणेशराज राई",
    "विपक्षको नाम": "प्रधानमन्त्री तथा मन्त्रिपरिषदको कार्यालय समेत",
    "फैसला मिति": "2072-09-17",
    "फैसला": "रिट खारेज",
    "फैसलाको संक्षिप्त विवरण": "..."
  }
]
```

The app also tolerates common JS variable/export styles, but the safest format is a direct JSON array.

### Empty columns

Empty Excel columns such as `""`, `Unnamed: 9`, or columns with no meaningful values are automatically ignored.

---

## 3. How to run

### Option A: GitHub Pages

Upload the folder to GitHub Pages. Make sure `index.html` and `cbdata.js` are in the same directory.

### Option B: Local server

Because browser security may block reading `cbdata.js` under `file://`, use a small local server.

From the folder containing `index.html`:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/index.html
```

---

## 4. Main interface

### Search bar

Use the top search box to search across all columns or a selected column.

Supported search modes:

- Contains
- NOT / Exclude
- All words
- Any word
- Starts with
- Regex

Search history is stored locally in the browser and can be cleared from the dropdown.

### Nepali normalization

The `सिव=शीब` option helps normalize common Nepali spelling variations while searching.

---

## 5. Navigation & Jump panel

Open the compass/navigation button to access:

### Match navigation

Move to previous/next highlighted search match.

### Page navigation

Jump to first, previous, next, last, or a specific page.

### Text size scale

Table content text size can be adjusted continuously.

- Minimum: `8px`
- Maximum: `24px`
- Default: `13px`

### Wrap text

`Wrap text` controls whether long cell content wraps inside cells.

### Floating header row

When checked, the table header row floats/fixes at the top of the visible table area after scrolling stops.

### Column sequence

You may reorder columns without changing their internal/default identity.

Accepted formats:

```text
1,2,3
1;3;2
col1, col2, col3
c1, c3, c2
मुद्दा नं.; पक्षको नाम; फैसला
```

Notes:

- `1`, `col1`, and `c1` all mean the first column in the default/original view.
- Both comma `,` and semicolon `;` are accepted.
- Column nomenclature always follows the default/original column order so that other features continue to work.

### Transpose rows/columns

When checked, rows and columns are transposed in the display.

- Default: unchecked.
- Search/filter data logic remains based on the original dataset.

### Auto adjust column width

When checked, the app tries to fit all columns within the current window by adjusting column widths and wrapping content.

---

## 6. Filter / Sort / Columns panel

Open the slider/settings button to access advanced options.

### Search options

Choose:

- column to search
- search mode
- records per page

### Sort

Sort by any dynamic column. Columns are generated automatically from `cbdata.js`.

### Filters

Available filters are generated from likely columns where possible, such as:

- `फैसला`
- `मुद्दा`
- date/serial-like fields such as `फैसला मिति` or `क्र.सं.`

### Custom text add

This replaces the earlier fixed `श्री` checkbox.

You can add custom text to selected columns while displaying/copying.

Options:

- Enable/disable by checkbox.
- Enter custom text.
- Choose location:
  - ahead / prefix
  - last / suffix
- Select columns by column name or aliases.

Accepted column input examples:

```text
न्यायाधीश; पक्षको नाम; विपक्षको नाम
1,2,3
col1; col3
c1, c2; c5
```

### Show / Hide columns

Toggle any dynamic column on or off.

---

## 7. Copy features

The app includes multiple copy-to-clipboard tools:

### Copy every cell

Each cell has a copy icon to copy that single cell.

### Copy whole row

Each row has a copy icon to copy the full row.

### Copy whole column

Each column header has a copy icon to copy the full filtered column.

### Copy whole table

The toolbar copy button copies the current filtered table.

---

## 8. Floating scroll and navigation buttons

### Floating horizontal scrollbar

For wide tables, a floating horizontal scrollbar is available so the user does not need to scroll to the bottom of the table to move left/right.

### Go to top

Floating upward arrow moves to the top.

### Go to bottom

Floating downward arrow moves to the bottom.

---

## 9. Recommended Excel-to-JS workflow

1. Prepare Excel with clean column heads.
2. Convert rows into JSON array.
3. Save as `cbdata.js`.
4. Put `cbdata.js` in the same folder as `index.html`.
5. Open through GitHub Pages or a local server.

A valid `cbdata.js` should look like:

```js
[
  { "क्र.सं.": "1", "मुद्दा": "उत्प्रेषण", "फैसला": "रिट खारेज" },
  { "क्र.सं.": "2", "मुद्दा": "परमादेश", "फैसला": "रिट जारी" }
]
```

---

## 10. Troubleshooting

### Data not loading

Check that:

- file name is exactly `cbdata.js`
- `cbdata.js` is in the same folder as `index.html`
- the file contains valid JSON/JS array data
- you are using a local server or GitHub Pages, not only `file://`

### Changes not visible

Hard refresh the browser:

```text
Ctrl + F5
```

Or clear browser cache.

### Column sequence not working

Use one of these formats:

```text
1,2,3
c1,c2,c3
col1;col2;col3
```

Do not mix invalid names with valid aliases unless you intentionally want unmatched columns ignored.

### Header not floating/fixed

Ensure `Floating header row` is checked in Navigation & Jump.

### Very wide table

Try:

- enable `Auto adjust column width`
- enable `Wrap text`
- use floating horizontal scrollbar
- hide unnecessary columns
- use column sequence to bring important columns first

---

## 11. Notes for legal/court-data use

This app is designed for Nepali legal data such as Constitutional Bench decisions, but it can handle any Excel-exported JSON dataset with dynamic columns.

Useful columns may include:

- `क्र.सं.`
- `न्यायाधीश`
- `मुद्दा`
- `मुद्दा नं.`
- `पक्षको नाम`
- `विपक्षको नाम`
- `फैसला मिति`
- `फैसला`
- `फैसलाको संक्षिप्त विवरण`

The interface remains generic, so future datasets with different column numbers or column names can also be used without changing the HTML code.

---

## 12. Version notes

This README corresponds to the updated `index.html` containing:

- dynamic `cbdata.js` loading
- dynamic columns
- Nepali search normalization
- search history
- copy cell / row / column / table
- custom text add by column name or aliases
- text-size slider
- wrap text
- floating header row
- floating horizontal scrollbar
- go to top / bottom
- column sequence editor
- transpose rows/columns
- auto adjust column width
- dark/light theme
