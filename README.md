
You can download raw CSV with data here: https://github.com/michael-ostaszewski/stock_scraper_spx/tree/main/stocks

Features
	•	Return transforms: percent returns or log returns.
	•	Correlation methods: Pearson, Spearman, Kendall, and “sign” (up/down) correlations.
	•	Column ordering: original, by mean correlation, spectral ordering, hierarchical clustering, or by market cap (asc/desc).
	•	Portfolio subsetting: restrict the heatmap to a user-defined ticker list.
	•	Ticker exclusions: drop specific tickers (e.g., to fix bad market caps).
	•	High-res export: save to JPG with a hard pixel cap (e.g., 10k×10k).
	•	Top pairs: prints the 20 most negatively correlated pairs for quick inspection.

Input format (CSV)
	•	Delimiter: ; (semicolon)
	•	Required columns:
	•	Date of record (parseable date)
	•	Stock (ticker)
	•	One price column: Closing Price (preferred) or Price
	•	Optional (for market-cap ordering):
	•	Market cap clear (numeric)
	•	or Market cap with suffixes (e.g., 3.66T); the notebook includes a helper to parse these.

The script is robust to thousands separators and commas in numeric fields.

Quick Start
1.	Install dependencies:
pip install pandas numpy matplotlib scipy pillow

2. Open the notebook (or run as a script) and adjust the config block:
CSV_PATH = "/path/to/stocks_data.csv"  # semicolon-delimited
T = 150                                 # number of trading rows (window)
RET_KIND = "log"                         # "pct" | "log"
CORR_KIND = "pearson"                    # "pearson" | "spearman" | "kendall" | "sign"
ORDER_MODE = "spectral"                  # "original" | "mean-corr" | "spectral" | "hierarchical" | "mcap-desc" | "mcap-asc"
PORTFOLIO_TICKERS = None                 # e.g., ["AAPL","MSFT","NVDA"] or None for all
EXCLUDE_TICKERS = {"TSM"}                # drop problematic tickers, or set to empty set
out_path = "/path/to/correlation_heatmap.jpg"
max_px = 10000                           # pixel cap for long edge
dpi = 300


3.	Run all cells.
	•	The heatmap will display inline.
	•	A high-res JPG is saved to out_path.
	•	The notebook prints the Top 20 most negative pairs.

What the notebook does
	1.	Loads and cleans the CSV (semicolon delimiter; normalizes Stock & dates).
	2.	Picks Closing Price (fallback: Price) and pivots to a wide price frame.
	3.	Computes returns (percent or log) over the last T rows.
	4.	Calculates the chosen correlation metric with pairwise completeness.
	5.	Orders rows/columns (optional spectral/hierarchical/market-cap sorting).
	6.	Renders a Matplotlib heatmap and saves a high-res JPG.
	7.	Lists the most negatively correlated pairs.

Notes & tips
	•	Market-cap ordering: Requires Market cap clear or Market cap. If you use the latter, make sure the helper that parses K/M/B/T suffixes is present (it is included in the notebook).
	•	Hierarchical mode: Requires scipy. If unavailable, the code falls back to spectral ordering.
	•	Colorbar errors: The notebook uses imshow and wires the colorbar to the returned mappable; you shouldn’t see “No mappable found” errors anymore.
	•	Performance: For very large universes, consider reducing T, subsetting tickers (PORTFOLIO_TICKERS), or using simpler ordering.

Example outputs
	•	correlation_heatmap.jpg — high-resolution heatmap (pixel-capped).
	•	Printed table: Top 20 negatively correlated pairs (ticker A, ticker B, correlation).

