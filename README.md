// ------------------------------
// Query: README_Aesthetic
// Purpose: produce an "aesthetic README" as a Power Query table (Section, Content, Notes).
// Paste this into Power Query Advanced Editor as a new blank query.
// The result is a table you can load to Power BI or export to CSV/PDF.
// ------------------------------

let
    // Helper: create a line break constant for readability
    lf = "#(lf)",

    // Build the rich README content as a list of records.
    // Each record is a section with a title and long-form content.
    READMERecords =
        {
            [
                Order = 0,
                Section = "Project Title",
                Content = "Data Leverager — Power Query Transformation Project" & lf & lf &
                          "Power BI ETL-only project demonstrating extraction, cleaning, integration, transformation, profiling, parameterization, and refresh simulation using Power Query (M). No DAX or visualizations required."
            ],

            [
                Order = 1,
                Section = "Summary / Purpose",
                Content =
                    "Purpose:" & lf &
                    "To simulate an enterprise-like data engineering workflow using Power Query. The focus is on robust, repeatable ETL: ingest files from a folder, scrape an HTML table from the web, clean and standardize data, enrich with master data, profile quality, parameterize sources, and support automated refresh." & lf & lf &
                    "Outcomes:" & lf &
                    "- Cleaned & consolidated sales dataset (Jan–Mar, plus refresh-ready Apr)" & lf &
                    "- Employee master enrichment" & lf &
                    "- Aggregations by Region and basic quality metrics" & lf &
                    "- Readme doc & M-code for reproducibility"
            ],

            [
                Order = 2,
                Section = "Files & Folder Structure",
                Content =
                    "Local folder (example: C:\\Data\\SalesFiles\\) contains:" & lf &
                    "- Sales_Jan.xlsx" & lf &
                    "- Sales_Feb.xlsx" & lf &
                    "- Sales_Mar.xlsx" & lf &
                    "- (optional) Sales_Apr.xlsx for refresh simulation" & lf &
                    "- Employee_Data.xlsx" & lf &
                    "- wikipedia_sources.csv (list of web source URLs)" & lf & lf &
                    "Project file:" & lf &
                    "- PowerBI-First-Project.pbix (contains Power Query queries)"
            ],

            [
                Order = 3,
                Section = "Data Sources (Web)",
                Content =
                    "Suggested HTML tables (use Get Data → Web):" & lf &
                    "- GDP by country: https://en.wikipedia.org/wiki/List_of_countries_by_GDP_(nominal)" & lf &
                    "- COVID-19 by country: https://en.wikipedia.org/wiki/COVID-19_pandemic_by_country_and_territory" & lf &
                    "- Population by country: https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population"
            ],

            [
                Order = 4,
                Section = "High-level Architecture",
                Content =
                    "Extract → Clean → Standardize → Transform → Integrate → Profile → Parameterize → Refresh" & lf & lf &
                    "Key principles:" & lf &
                    "- Parameterize all environment-specific paths (folder path, employee file path)" & lf &
                    "- Keep provenance (SourceFile, Date modified) for dedupe and troubleshooting" & lf &
                    "- Make transformations idempotent so refresh remains stable"
            ],

            [
                Order = 5,
                Section = "Step-by-step Build Instructions (Detailed)",
                Content =
                    "1) Folder ingestion (Sales files):" & lf &
                    "- Get Data → Folder → Combine & Transform. This creates a reusable sample-file transformation and a combined query." & lf & lf &
                    "2) Employee master ingestion:" & lf &
                    "- Get Data → Excel → Employee_Data.xlsx → Transform → rename query Employee_Raw." & lf & lf &
                    "3) Web table ingestion:" & lf &
                    "- Get Data → Web → paste Wikipedia link → select the table in Navigator → Transform." & lf & lf &
                    "4) Cleaning sequence (applied to combined sales table):" & lf &
                    "- Remove blank rows and empty columns" & lf &
                    "- Promote first row to headers" & lf &
                    "- Trim, Clean, upper/lower-case as needed" & lf &
                    "- Change Type with Locale for Order Date, Cost, Revenue" & lf &
                    "- Remove duplicates (use OrderID + SourceFile provenance) and filter nulls on mandatory fields" & lf & lf &
                    "5) Text tools:" & lf &
                    "- Text.Trim, Text.Clean, Text.Replace, Text.Upper or Text.Lower" & lf &
                    "- Split columns by delimiters when required" & lf & lf &
                    "6) Numeric & calculated columns:" & lf &
                    "- Ensure numeric currency parsing (remove currency symbols & thousand separators as needed)" & lf &
                    "- Round Revenue and Cost to 2 decimals" & lf &
                    "- Add Profit = Revenue - Cost" & lf & lf &
                    "7) Date & time:" & lf &
                    "- Extract Day, Month, Year, Quarter" & lf &
                    "- Create FiscalMonth based on fiscal year strategy (e.g., April start): use Date functions" & lf & lf &
                    "8) Conditional columns & indexing:" & lf &
                    "- Sales Category: High (≥10,000), Medium (5,000–9,999), Low (<5,000)" & lf &
                    "- Add Index columns (0-based and 1-based)" & lf & lf &
                    "9) Pivot / Unpivot:" & lf &
                    "- Unpivot is used to normalize wide monthly columns; Pivot to aggregate where needed" & lf & lf &
                    "10) Merge & append:" & lf &
                    "- Append Jan–Mar is achieved via Folder combine; merge Sales with Employee on Region or EmployeeID using Left Outer join" & lf & lf &
                    "11) Grouping & aggregation:" & lf &
                    "- Group by Region → Total Sales (sum), Avg Order Value (average), Transaction Count (count rows)" & lf & lf &
                    "12) Data profiling:" & lf &
                    "- Enable Column Profile, Column Distribution, Column Quality → identify missing values, errors, and distinct counts" & lf & lf &
                    "13) Parameters & Source Settings:" & lf &
                    "- Create parameter SalesFolderPath and EmployeeFilePath; reference them in Folder.Files and Excel.Workbook calls" & lf & lf &
                    "14) Refresh simulation:" & lf &
                    "- Drop Sales_Apr.xlsx into folder → Home → Refresh in Power BI Desktop → confirm new rows are loaded and transformations still apply"
            ],

            [
                Order = 6,
                Section = "Power Query (M) Design Decisions",
                Content =
                    "- Use text cleaning with Text.Clean & Text.Trim early to avoid propagation of bad values." & lf &
                    "- Convert OrderID to text to avoid type mismatches across files." & lf &
                    "- Keep SourceFile and Date modified (from Folder.Files) for provenance and deduplication logic." & lf &
                    "- Use Table.PromoteHeaders on the sample file generated by Combine to normalize header mapping." & lf &
                    "- Apply 'Change Type with Locale' to ensure correct parsing of dates/decimal separators across locales."
            ],

            [
                Order = 7,
                Section = "Common Pitfalls & Mitigations",
                Content =
                    "Pitfall: Dates imported as text or ambiguous dd/mm vs mm/dd." & lf &
                    "Mitigation: Use Change Type with Locale or Date.FromText fallback." & lf & lf &
                    "Pitfall: Thousand separators & currency symbols on numeric columns." & lf &
                    "Mitigation: Remove non-numeric characters before Number.FromText." & lf & lf &
                    "Pitfall: Schema drift across months (different headers)." & lf &
                    "Mitigation: Standardize headers in the Transform Sample File and map all known variants to canonical names in M." & lf & lf &
                    "Pitfall: Refresh fails when new file has different sheet/table format." & lf &
                    "Mitigation: Use robust workbook parsing and handle missing columns by adding them with nulls to preserve schema stability."
            ],

            [
                Order = 8,
                Section = "Outputs & Deliverables",
                Content =
                    "- PowerBI-First-Project.pbix (Power Query only)" & lf &
                    "- Source folder with Sales_Jan/Feb/Mar.xlsx and Employee_Data.xlsx" & lf &
                    "- README (this document) exported to PDF" & lf &
                    "- Optional: Sales_Apr.xlsx to demonstrate refresh"
            ],

            [
                Order = 9,
                Section = "Submission Checklist",
                Content =
                    "1) .pbix file with all Power Query queries included." & lf &
                    "2) Source folder (all Excel files) zipped or shared." & lf &
                    "3) README.pdf summarizing sources, transformations, and challenges (use this query export or copy content into Word/PDF)." & lf &
                    "4) Short note on any assumptions (e.g., fiscal year start, currency locale)."
            ],

            [
                Order = 10,
                Section = "M-code Snippets (Key Recipes)",
                Content =
                    "Use these snippets inside Power Query's Advanced Editor or Custom Column when needed:" & lf & lf &
                    "- Trim & Upper:  Text.Upper(Text.Trim(Text.Clean([Customer Name])))" & lf &
                    "- Remove currency symbols:  Number.FromText(Text.Remove(Text.From([Revenue]), {\",\",\"$\",\"₹\",\"€\",\" \"}))" & lf &
                    "- Profit column:  each Number.Round([Revenue] - [Cost], 2)" & lf &
                    "- Conditional Sales Category (custom column):" & lf &
                    "    if [Revenue] >= 10000 then \"High\" else if [Revenue] >= 5000 then \"Medium\" else \"Low\"" & lf &
                    "- Fiscal month example (April start):" & lf &
                    "    let m = Date.Month([Order Date]) in if m >= 4 then m - 3 else m + 9"
            ],

            [
                Order = 11,
                Section = "Full M-code Availability",
                Content =
                    "A monolithic, portable M script was provided earlier (named 'Data Leverager - Full ETL M')." & lf &
                    "If you prefer query-per-step (Sales_Raw, Sales_Clean, Employee_Clean, Sales_Grouped, Wiki_GDP) I can split the monolithic code into separate queries for easier UI inspection."
            ],

            [
                Order = 12,
                Section = "Quality Assurance & Validation",
                Content =
                    "Validation steps to run after refresh:" & lf &
                    "- Check row counts per month match expected counts." & lf &
                    "- Check that no rows have null OrderID, Order Date, or Revenue." & lf &
                    "- Validate Revenue = Cost * (1 + margin) * Quantity for a random sample." & lf &
                    "- Review Column Profile for 'Revenue' and 'Quantity' for outliers." & lf &
                    "- Confirm merged Employee fields are populated for expected regions."
            ],

            [
                Order = 13,
                Section = "How to Export This README (three quick ways)",
                Content =
                    "1) In Power Query: Right-click the query result → 'Load To' → create a table in a new worksheet in Excel → Save as PDF." & lf &
                    "2) In Power BI Desktop: Load the query as a table visual (optional) then use 'Export data' or copy-paste to Word and Save as PDF." & lf &
                    "3) Copy this query text into a Markdown file or GitHub README.md and style with GitHub markup/badges."
            ],

            [
                Order = 14,
                Section = "Assumptions",
                Content =
                    "- Sales files share the same semantic schema (or the Transform Sample File is used to normalize differences)." & lf &
                    "- Dates are parseable when Culture parameter is set appropriately." & lf &
                    "- Employee master contains Region values that correlate with Sales Region for enrichment (or EmployeeID is available as a join key)."
            ],

            [
                Order = 15,
                Section = "Next Steps / Optional Enhancements",
                Content =
                    "- Add parameterized mapping for Product → Category to allow controlled taxonomy changes." & lf &
                    "- Implement incremental refresh (Power BI Service) if dataset grows large and you publish to workspace." & lf &
                    "- Add test-suite queries that assert key invariants (row counts, zero negative revenues, no-null keys) and fail early." & lf &
                    "- Add documentation snapshots using 'Table.Profile' outputs embedded as query tables for submission evidence."
            ],

            [
                Order = 16,
                Section = "Contact & Ownership",
                Content =
                    "Prepared for: Red & White Skill Education" & lf &
                    "Author: (Your Name) — replace with your name before submission" & lf &
                    "Date: May 2025" & lf &
                    "Notes: This README is intentionally verbose and suitable for academic submission or enterprise handover."
            ]
        },

    // Convert the list of records into a table and return sorted by Order
    README_Table = Table.FromRecords(READMERecords),
    README_Sorted = Table.Sort(README_Table, {{"Order", Order.Ascending}}),

    // Reorder columns to present Section first then Content
    README_Final = Table.SelectColumns(README_Sorted, {"Order", "Section", "Content"})
in
    README_Final
