Interactive Jencks Diagram (CR Fork)

This project is an interactive visualization tool for exploring author contributions and conceptual connections between the built environment and biology, using an annotated digital version of the Jencks "Evolutionary Tree" Diagram.
This codebase was originally developed as part of the IU Faculty Assistance in Data Science (FADS) initiative, and is being further developed and maintined with assistance from the HathiTrust Research Center.

🚀 Live Features Overview

    Interactive SVG Chronogram: Explore the Jencks Diagram with clickable and highlightable author names, all mapped to their respective metadata and external links.

    Smart Search Bar: Autocomplete and instant search for authors, with highlighting of all SVG matches and contextual metadata pop-up under the search area.

    Year Range Slider: Dual-handle slider filters visible authors by decade (1890–2010), with instant updates in the SVG.

    Pop-up Author Details: Click on any author name to open a floating metadata pane (with links to Wikipedia, VIAF, Getty), styled for visual clarity.

    HTDL Filter: “Show Authors in HTDL” filter highlights all authors with digitized works available, with an optional dimmer effect for clarity.

    Responsive, Accessible UI: Designed for both desktop and projection/teaching environments, using a modern dark Jencks-like aesthetic.

    Modular, Clean Code: Clear separation of concerns for search, filtering, pop-up logic, and SVG manipulation, making the app easy to extend and refactor.

📁 File Structure

jencks-fads/
├── index.html                  # Main interactive HTML/JS application
├── jencks-svg-1.svg            # Core SVG diagram (authorship/relationship map)
├── jencks-authors-1.csv        # Main cleaned and relabeled author metadata file
├── README.md                   # This documentation

🧩 Core Interactions
🔍 Author Search

    Autocomplete search with real-time suggestions.

    On selection, all matching SVG names are highlighted in yellow, all other names dim.

    Pop-up under the search shows detailed author info (and all their chronogram positions if an author appears multiple times).

    Reset returns the diagram to its original, unfiltered state.

⏳ Year Range Slider

    Adjust start/end handles to filter authors by their year/decade.

    Only authors within the selected date range remain visible/highlighted.

    Inactive authors are greyed out and unclickable.

👤 Author Pop-Up

    Hover: Highlights an author’s name in the SVG (visual glow).

    Click: Pops up a floating card with:

        Last Name, First Name (in bold), and (person) in italic.

        Chronogram position(s)

        Born/Died

        Works in HTDL (number, always shown)

        Number of Unverified Comparisons

        Top Bio Terms (up to 5)

        Wikipedia, VIAF, and Getty link buttons (greyed out if N/A)

    The pop-up floats beside the clicked SVG name or below the search bar for search results.

    Buttons: Wikipedia, VIAF, and Getty links open in new tabs; greyed out if data missing.

📖 HTDL Filter

    “Show Authors in HTDL” option in “More Filters” highlights all authors with HTDL works in the SVG.

    Optional dimmer effect reduces the visual prominence of non-highlighted names, making HTDL authors stand out.

🧑‍💻 Implementation Details

    All interactive logic is client-side (no server code or frameworks required).

    Uses D3.js v7 for all SVG and DOM manipulation.

    Author data and metadata are loaded dynamically from CSVs.

    Pop-up positioning: Search pop-up appears under the search bar; SVG pop-up appears beside the clicked name.

    All styling is handled in a unified CSS block in index.html.

    Multiple pop-ups can be open at once (search + SVG click), and each is styled for clarity and accessibility.

⚙️ Extensibility & Customization

    All data sources and SVG/CSV filenames can be swapped out in index.html.

    New filters can be added via the dropdown filter menu (More Filters).

    UI can be re-styled with a single CSS update.

    Collaboration-ready: Clear code comments, modular functions, and easy to adapt for expert collaborators or further extension.

🛠️ How to Run

    Clone the repo or download the source.

    Place the SVG and CSVs in the project directory.

    Open index.html in a web browser.

        (For best results, use a simple local HTTP server for CSV loading: e.g., python -m http.server in the project folder, then browse to http://localhost:8000/)

    All features work locally, with no build step required.

📝 Contributing/Expert Notes

    This repo is currently under active expert development and refactoring.

    If you’re extending the core logic (especially around pop-ups, filtering, or dimming), please note the modular function structure and update shared logic in only one place where possible.

    Feel free to open issues/PRs for any bugs, inconsistencies, or feature requests!

👏 Credits & Acknowledgements

    Original Jencks Diagram design: [Charles Jencks]

    Prototype created by: IU Faculty Assistance in Data Science (FADS) -- coding by Princy Reshma Ramaseshan, Neeraj Boyapati, and Achu Jeeju, led by Chris Reinhart, Ryan Dubnicek and John Walsh.

    Forked and extended by: Chris Reinhart, in preparation for additional work by HTRC and Nikolaus Nova Parulian.

    Special thanks to all prior contributors in the prototype repo, and to all collaborators extending this visualization for research and teaching.

