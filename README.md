# Interactive Jencks Diagram

This project is an interactive visualization tool for exploring author contributions related to comparisons between the built environment and biology, using the Jencks Diagram.

## 💡 Features

- **Dynamic Search Bar**: Search author names using autocomplete and highlight functionality.
- **Interactive Hover**: Hover over names on the SVG to view quick info.
- **Author Metadata Popup**: Click on an author to see metadata from `author_editcore.csv`.
- **Date Filter Slider**: Filter authors visually by decade range.
- **Responsive Layout**: Optimized for readability and visual clarity.

### 📂 File Structure

```
jencks-fads/
├── final.html             # Main interactive HTML page
├── jencks_1.svg           # SVG diagram of author relationships
├── author_data1.csv       # CSV used for search suggestions
├── author_editcore.csv    # Main author metadata (merged, cleaned)
├── README.md              # Project documentation
```


## 🕰️ Timeline Slider: Interactive Year Filtering

This module enables interactive exploration of the Jencks Diagram by year range, helping users analyze author appearances across time.

### 🔧 Features
- Dual-handle **year range slider** from **1890 to 2010**
- **Dynamic filtering**: Author names fade in/out based on selected year range
- **Accurate matching** using author name and positional estimation
- Handles **multiple instances** of the same author in the SVG
- **Logs unmatched** names for refinement and debugging
- Styled in the **Jencks aesthetic**: black background, Helvetica font, yellow/gray text

---

### 🔍 Matching Logic
Each author from the CSV is matched to SVG `<text>` elements using:

1. **Name Matching**  
   - Case-insensitive, partial regex matching to account for name variations  

2. **Positional Estimation**  
   - Estimates the year of a `<text>` element based on its `x-position` in the SVG  
   - Matches to the closest author year from the CSV by minimizing absolute year difference  

This logic ensures correct pairing even when authors (e.g., *Wright*) appear multiple times across decades.

---

### ⚙️ Key Functions

#### `loadJencksDiagram()`
- Loads `jencks_diagram.svg` into `#diagram-holder` using `fetch`
- Logs any errors during load

#### `loadAndAssignCSV()`
- Loads author data from the CSV
- Waits for SVG to finish loading
- Triggers matching via:
  - `assignYearsToTextElements(svg, data)`
  - `updateAuthorTextVisibility(FORCED_MIN, FORCED_MAX)`

#### `assignYearsToTextElements(svg, data)`
- Cleans and normalizes author names
- Loops through `<text>` nodes
- Estimates each node's year from its x-position
- Matches to CSV authors and assigns a `data-year` attribute
- Logs unmatched names for review

#### `createYearSlider()`
- Constructs a **dual-handle range slider** using D3.js
- Decade-based ticks from 1890 to 2010
- Middle line indicates active year range
- Triggers `updateRange()` on handle drag

#### `updateRange()`
- Converts slider handle positions to years
- Updates the displayed range
- Calls `updateAuthorTextVisibility(minYear, maxYear)`

#### `updateAuthorTextVisibility(minSel, maxSel)`
- Loops through all `<text>` elements with `data-year`
- If within selected range:
  - Sets **yellow fill** and **full opacity**
- If outside range:
  - Applies **gray fill** and **reduced opacity**
- Uses inline styles with `!important` to override existing SVG styles
