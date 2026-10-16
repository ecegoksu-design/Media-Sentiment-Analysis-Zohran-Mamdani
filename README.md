# Sentiment Analysis of News Coverage of Zohran Mamdani

COMP 370 final project. We collected English-language North American news
articles about Zohran Mamdani's NYC mayoral campaign (NewsAPI), manually
annotated each by topic category and sentiment, and analysed them with TF-IDF
and sentiment breakdowns (including before vs. after the Nov 4 election).

The full write-up is in [`report/Sentiment_Analysis_on_Zohran_Mamdani.pdf`](report/Sentiment_Analysis_on_Zohran_Mamdani.pdf).

## Repository layout

```
scripts/   data collection, analysis and plotting code
data/      article dataset (annotated), TF-IDF results, LLM inputs/summaries
figures/   generated plots
report/    final report and original project instructions
```

## Setup

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export NEWSAPI_KEY=your_key_here   # only needed for data collection
```

Run all commands from the repository root.

## Pipeline

| Step | Command | Output |
|------|---------|--------|
| 1. Pick North American sources | `python scripts/generate_sources.py` | `data/north_american_sources.json` |
| 2. Collect articles | `python scripts/collect_articles.py --help` | `articles.json` |
| 3. Annotate manually | (done by hand) | `data/all_articles.csv` |
| 4. TF-IDF per category | `python scripts/tf_idf.py data/all_articles.csv` | `data/tf_idf_results_1grams.csv` |
| 5. Group articles for LLM summaries | `python scripts/divide_articles_for_llm.py data/all_articles.csv` | `data/articles_for_llm.csv` |
| 6. Plots | see below | `figures/` |

Plots:

```bash
python scripts/sentiment_november4th.py   # figures/sentiment_pre_post_nov4.png
python scripts/topical_sentiment.py       # figures/topic_sentiment_distribution.png
python scripts/heatmap.py data/tf_idf_results_1grams.csv
python scripts/piechart.py                # category distribution (interactive)
python scripts/tfidf_barcharts.py         # per-category TF-IDF bars (interactive)
```

`data/llm_summaries.csv` holds the LLM-generated summaries per category.

## Notes

- Collection uses the NewsAPI free plan (first page of results only).
- API keys are read from the `NEWSAPI_KEY` environment variable and must not be committed.
