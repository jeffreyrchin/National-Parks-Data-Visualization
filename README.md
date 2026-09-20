# Growth in Popularity of America's National Parks, 1904–2024

**CS7250 Data Visualization: Final Project** · D3.js · Observable · Python (pandas)

An interactive spike map showing 120 years of visitation to the 63 U.S. national parks. Drag a year slider from 1904 to 2024 and watch spikes grow at each park's location as visitation rises and parks join the National Park System.

**[Open the interactive visualization →](https://observablehq.com/d/d2731cc18bcf08e1)**

<img width="1026" height="704" alt="Visualization Screenshot" src="https://github.com/user-attachments/assets/17c0a3f4-dc55-4917-a657-1bb500f51456" />


## Relevance

In 2024, the 63 national parks logged nearly 100 million recreation visits. In 1904, fewer than 10 parks existed and they drew about 120,000 visits combined. The visualization is built to answer three questions:

1. Which parks have grown the most in recreational visits since 1904?
2. What historical events have significantly affected park visitation?
3. Which time periods saw the most growth across all parks?

## Data

The data is combined from two datasets built with the National Park Service's Visitor Use Statistics query builders (1904–1978 and 1979–2024), covering the same 63 parks. The fields are park name, unit code, year, and recreation visits. I added:

- **Coordinates** for each park (looked up via Google Maps)
- **Annual acreage** for each park, compiled from NPS annual acreage reports (1997–2025) and historical publications on npshistory.com
- **Authorization years** from the NPS park anniversaries page, so parks appear on the map when they joined the system

Map boundaries come from d3-composite-projections (50 states) and U.S. Census Bureau GeoJSON (five unincorporated territories).

## Data cleaning and quality decisions

Most of the effort went into making sure the visualization does not mislead.

- **Duplicates and merging (Python, pandas):** removed duplicate rows from the 1904–1978 export with `drop_duplicates()`, combined the two periods with `concat()`, and joined coordinates using the unique four-letter unit code with `merge()`.
- **Zero visits are not always zero.** Several parks report zero visits in certain years because they stopped counting, not because nobody came. Rather than plotting zeros, the visualization shows those years as **"Unknown visits."**
- **Counting-procedure changes.** North Cascades appears to drop from 456,444 to 22,796 visits between 1990 and 1991. Research showed the park simply stopped counting visitors at two of its most popular sites (Ross Lake and Lake Chelan). I added the separately published visitation for those sites to keep the series consistent. For other artificial breaks, I verified the cause and used linear interpolation between neighboring years.
- **Real spikes were left alone.** Sudden jumps that matched real events were kept, e.g. Gateway Arch's jump between 1965 and 1966, when construction of the park was completed.
- **Missing years:** parks with low visitation often had gaps, so I added rows for every year from each park's authorization year to 2024 to make the growth of the system visible.

## Design

- **Spike map.** Spike length encodes annual visits, which is easier to compare accurately than circle area. A circle at the base of each spike keeps low-visitation parks visible and hoverable.
- **Composite Albers projection.** Places Alaska, Hawaii, and the territories in insets, so distant parks aren't distorted.
- **Slider, bar, and line chart.** A year slider drives the map's evolution over time. A bar shows total visits for the selected year, which makes years when many parks moved together stand out, such as 2015 and 2020. A line chart shows the full 1904–2024 trend, annotated with major historical events.
- **Emphasis on national trends.** Spikes that move against the national trend animate slightly later, so overall patterns are perceived first (based on the Gestalt law of common fate).
- **Filtering and exploration.** Hover tooltips show visits and acreage, numeric filters narrow parks by visits and acres, a checkbox filters out unknown-visit parks, and a reset button restores defaults. A guided tutorial explains the interactions, and newly added parks briefly darken as they appear.
- **Peer feedback.** Six suggestions from classmates were incorporated, including the line chart, the reset button, and the tutorial.

## Repository contents

| File | Description |
|------|-------------|
| `Final Project Report.pdf` | Full write-up: data preparation, design rationale, alternatives considered, development process |
| Observable notebook (linked above) | The interactive visualization and its D3 code |

## Tools

D3.js · Observable · Python · pandas · d3-composite-projections · d3-simple-slider

## Acknowledgments

Data from the National Park Service and the NPS History Electronic Library & Archive. Spike map approach adapted from Mike Bostock's Observable notebook on U.S. county populations. See the report for the full list.

## Author

Jeffrey Chin
