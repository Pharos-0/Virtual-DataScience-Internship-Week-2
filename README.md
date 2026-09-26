# Virtual DataScience Internship - Week 2

## Advanced Data Visualization and Storytelling with Python

Week 2 of the Virtual Data Science with Python Apprentice Internship. The cleaned
churn data from Week 1 is turned into a visual story for a non-technical reader:
who leaves, when they leave, and which customer characteristics go together with
leaving.

| Week | Repository |
|---|---|
| 1 | [Virtual-DataScience-Internship-Week-1](https://github.com/Pharos-0/Virtual-DataScience-Internship-Week-1): data acquisition, cleaning and EDA |
| **2** | **Visualization and storytelling (this repository)** |
| 3 | [Virtual-DataScience-Internship-Week-3](https://github.com/Pharos-0/Virtual-DataScience-Internship-Week-3): hypothesis testing |
| 4 | [Virtual-DataScience-Internship-Week-4](https://github.com/Pharos-0/Virtual-DataScience-Internship-Week-4): machine learning |
| 5 | [Virtual-DataScience-Internship-Week-5](https://github.com/Pharos-0/Virtual-DataScience-Internship-Week-5): final project |

## Dataset

The cleaned **IBM Telco Customer Churn** dataset produced in Week 1 (7,043 customers, 22 columns).
Its SHA-256 checksum is checked on every load, so the charts always use exactly the Week 1 output.
The original source is [IBM/telco-customer-churn-on-icp4d](https://github.com/IBM/telco-customer-churn-on-icp4d)
(Apache 2.0). See [`data/README.md`](data/README.md).

## The story in eight charts

| # | Question | Chart | Library |
|---|---|---|---|
| 1 | Where in the customer lifecycle do departures happen? | Stacked histogram of tenure | Matplotlib |
| 2 | How does the chance of leaving change with tenure? | Line chart with 95% Wilson intervals | Matplotlib |
| 3 | Is contract type just another way of measuring tenure? | Annotated heatmap (contract x tenure band) | Seaborn |
| 4 | Do leavers pay more, and is that about price or service? | Split violin plot by internet service | Seaborn |
| 5 | Which add-on services go together with staying? | Dumbbell chart (internet customers) | Matplotlib |
| A1 | Is the payment-method pattern just a contract pattern? | Small multiples with a highlighted bar | Matplotlib |
| A2 | Where are the highest-risk customer segments? | Interactive bubble chart (HTML + PNG) | Plotly |
| A3 | Are there unusual billing records? | Scatter of billing deviation vs tenure | Matplotlib |

Every chart uses a headline title that states the finding, one accent colour, and printed sample
sizes wherever rates are compared.

## Key findings (descriptive, not causal)

- 55.5% of all customers who left did so within their first 12 months. The churn rate falls from
  52.9% (0-6 months) to 5.3% (67-72 months).
- Within every tenure band, month-to-month customers churned far more often (for example 51.4% vs
  10.5% vs 0.0% in the first year), so contract type is not just a stand-in for tenure.
- Leavers pay more per month overall only because many use fibre optic (41.9% churn). *Within* each
  internet service their median charge was lower (DSL $49.25 vs $59.75; fibre $87.55 vs $94.80).
- Internet customers without online security or tech support churned at about 42%, against about 15%
  for those with them. Streaming add-ons made little difference.
- Electronic-check payers churned more within every contract type.
- One segment (month-to-month, fibre optic, electronic check) holds 18.6% of customers but 42.2% of
  all churners (60.4% churn rate).

## Repository structure

```
|-- README.md
|-- requirements.txt
|-- data/
|   |-- README.md
|   `-- processed/telco_churn_clean.csv     # Week 1 output (checksum verified)
|-- notebooks/
|   |-- Week_2_Visual_Story.ipynb           # executed notebook (same code as src/)
|   `-- build_notebook.py
|-- src/
|   |-- config.py  data_utils.py  plot_style.py
|   `-- 04_visualization.py                 # all eight charts
|-- outputs/
|   |-- figures/    # fig2_1 ... fig2_8 (PNG) + fig2_7_segment_risk_map.html (interactive)
|   |-- tables/     # the numbers behind each chart
|   `-- results/visual_story_summary.json
`-- screenshots/    # 7 screenshots (notebook cells + the interactive chart with its tooltip)
```

## How to reproduce

Tested with Python 3.11.9 (pandas 3.0.6, Matplotlib 3.11.2, Seaborn 0.13.2, Plotly 7.1.0,
Kaleido 1.4.0, statsmodels 0.15.0).

```bash
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt

python src/04_visualization.py       # all figures, tables and the JSON summary
python notebooks/build_notebook.py   # optional: rebuild and execute the notebook
```

The script is deterministic. Kaleido exports the static Plotly image through a locally
installed Chrome. The interactive HTML loads plotly.js from its CDN, so it needs an internet
connection to display.

## Notebook and screenshots

`notebooks/Week_2_Visual_Story.ipynb` is built from `src/04_visualization.py` (Jupytext percent
format) and executed, so it contains the same code. Screenshots `w2_01`-`w2_06` are crops of the
executed notebook, and `w2_07` shows the saved interactive chart in a browser with Plotly's hover
tooltip visible.

## Limitations

- These are associations in observational, single-snapshot data. They show where churn is
  concentrated, not why customers leave.
- Rates for small groups (for example two-year customers in their first year, n = 68) are less
  precise. Sample sizes are printed on the charts.
- The data describes a fictional company (per IBM).

The written report for this week is submitted separately through the internship portal.
