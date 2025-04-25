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
├── index.html                  # Final interactive HTML page with slider, popup, and search
├── final.html                  # Earlier HTML version (popup + search only)
├── jencks_1.svg                # SVG diagram of author relationships
├── author_data1.csv            # CSV used for search suggestions
├── author_editcore.csv         # Main author metadata (merged, cleaned)
├── extracted_text_positions.csv # CSV used for assigning year metadata to SVG names
├── README.md                   # Project documentation
```

## Search Engine: Interactive Author Highlighting
This module enhances user navigation in the Jencks Diagram by enabling author-based search, allowing focused exploration and contextual insights across repeated name instances.

---

### 🔧 Features
- 	Smart search bar for real-time author name lookup
- 	Auto-suggestion dropdown activated on keystroke
-	Click-to-select suggestions for quick navigation
-	Instant visual feedback:
-	Selected authors are highlighted in yellow
-	Non-matching names are dimmed and blurred
-	Handles multiple instances of the same name across the SVG
-	Reset button to clear search and restore original view
-	Search popup displaying number of matches found
  
---

### 🔍 Matching Logic
Search functionality operates on clean, case-insensitive comparison of input with <text> elements in the SVG.
1.	Auto-suggestion
o	Matches all authors from the CSV whose names start with the typed string
o	Populates a clickable dropdown list with these matches
2.	Highlighting
o	Exact matches in the SVG are highlighted using .highlight class
o	All others are dimmed and blurred using .blurred class
3.	Reset Mechanism
o	Clicking "Reset" reloads the SVG, removes all highlights, and resets the search input

---

### ⚙️ Key Functions
#### showSuggestions()
- 	Triggered on every keyup in the input field
- 	Filters names from namesList that start with current input
- 	Populates the .suggestions dropdown dynamically
  
#### selectSuggestion(name)
- 	Called when a dropdown suggestion is clicked
- 	Sets input field value and triggers highlighting via highlightText()
  
#### highlightText()
- 	Loops through all <text> elements in the SVG
- 	If textContent matches input (case-insensitive):
- o	Adds .highlight class and removes .blurred
- 	Else:
- o	Adds .blurred and removes .highlight
- 	Displays a temporary popup with the number of matches found
  
#### resetPage()
- 	Restores original SVG content
- 	Clears search input
- 	Reattaches all event handlers
- 	Hides author detail card
  
________________________________________

### 💡 Integration Note
This feature is entirely client-side and operates independently of the year slider. However, it coexists smoothly within the combined interface. The styling, color scheme, and interaction model remain consistent with the Jencks aesthetic, supporting seamless user experience.

---

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

---

## 🧠 Author Metadata Interaction: Hover & Pop-up Detail Cards

This feature enables users to interactively explore author details within the Jencks Diagram by hovering over or clicking on author names, dynamically pulling information from the `author_editcore.csv` file.

---

### 🔧 Features
- **Hover Interaction**: Hovering over an author's name displays a tooltip popup with the author’s name.
- **Click Interaction**: Clicking an author's name opens a detailed pop-up card with metadata.
- **Dynamic Data Retrieval**: Information is retrieved live from the CSV based on the author name.
- **Fallback Handling**: 
  - If an author's metadata is not found in the CSV, no pop-up is displayed.
  - If certain fields (like VIAF or Wikipedia links) are missing, "No Link" disabled buttons are shown.

---

### 📖 Metadata Displayed
- Last Name, First Name
- Birth Year
- Death Year
- Works in VIAF Catalog (shows **Yes** if VIAF link exists, **No** otherwise)
- Works in HTRC Catalog
- Number of Built/Biological Comparisons
- Buttons linking to Wikipedia and VIAF pages (if available)

---

### ⚙️ Key Functions

#### `attachEventHandlers()`
- Adds event listeners to all `<text>` elements in the SVG.
- On **hover**: Displays a yellow popup showing the author’s name.
- On **click**: Calls `showAuthorDetails()` to show a full metadata card.

#### `showAuthorDetails(author, event)`
- Retrieves metadata from `csvData` based on the author's name.
- Populates and displays a detail card next to the clicked name.
- Includes logic:
  - If Wikipedia or VIAF link exists → adds active link buttons.
  - If not → shows disabled buttons.
  - Shows "Works in VIAF Catalog" as **Yes** or **No** depending on VIAF data.

#### `resetPage()`
- Clears pop-ups and restores the original unhighlighted SVG view.

---

### 🔍 Matching Logic
- Matches the author's **lowercased Last Name** from the `author_editcore.csv`.
- **Exact matching**: Case-insensitive match between the clicked SVG text and the CSV name field.
- If a match is not found, no metadata popup is shown.
- Future versions can improve matching accuracy by using the "Jencks Name" column.

---

### 💡 Integration Note
- Works independently of the search bar and year slider.
- Fully integrated into the interactive SVG environment for seamless UX.

