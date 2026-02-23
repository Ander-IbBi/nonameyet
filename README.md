# TSLA Sentiment Trading Bot

An algorithmic trading strategy built on **QuantConnect** that analyzes **Elon Musk's tweets** in real time using NLP sentiment analysis to make long/short decisions on **Tesla (TSLA)** stock.

---

## Strategy Overview

The bot monitors a preprocessed dataset of Elon Musk's tweets. For each tweet that mentions **Tesla** or **TSLA**, it computes a sentiment score using NLTK's VADER model. Based on that score, the algorithm takes a position on TSLA stock and exits all positions 15 minutes before market close every day.

| Sentiment Score | Action |
|---|---|
| `score > 0.5` | 🟢 Go **Long** (100% of portfolio) |
| `score < -0.5` | 🔴 Go **Short** (100% of portfolio) |
| `-0.5 ≤ score ≤ 0.5` | ⚪ No position taken |
| 15 min before close | 🔒 **Liquidate** all positions |

---

## 🗂️ Project Structure

```
├── main.py           # Main algorithm (TSLA_Sentiment_Tradeing_Bot + MuskTweet data class)
└── README.md
```

---

## ⚙️ How It Works

### 1. Custom Data Class — `MuskTweet`
- Loads a remote CSV file containing preprocessed tweets by Elon Musk
- Filters tweets that contain the keywords `tesla` or `tsla`
- Computes a **compound sentiment score** using VADER (`SentimentIntensityAnalyzer`)
- Returns the score and tweet content for each relevant data point

### 2. Trading Logic — `TSLA_Sentiment_Tradeing_Bot`
- On every tweet data event, reads the sentiment score
- Enters a **full long or short position** if the score exceeds the ±0.5 threshold
- Logs every trade signal with the corresponding score and tweet content
- **Scheduled exit**: liquidates all positions 15 minutes before market close

---

## 📊 Backtest Configuration

| Parameter | Value |
|---|---|
| **Start Date** | November 1, 2012 |
| **End Date** | January 1, 2017 |
| **Initial Capital** | $10,000 |
| **Resolution** | Minute |
| **Asset** | TSLA (Equity) |

---

## 🧰 Dependencies

| Library | Purpose |
|---|---|
| `AlgorithmImports` | QuantConnect LEAN engine base |
| `nltk` (VADER) | Sentiment analysis on tweet text |
| `PythonData` | Custom data feed integration |

> **Note:** This project runs on QuantConnect's cloud environment. The NLTK VADER lexicon is available by default on the platform.

---

## 📁 Data Source

The strategy consumes a preprocessed CSV of Elon Musk's tweets hosted on GitHub:

```
https://raw.githubusercontent.com/Ander-IbBi/nonameyet/refs/heads/main/MuskTweetsPreProcessed.csv
```

**Expected CSV format:**

```
YYYY-MM-DD HH:MM:SS, tweet content here
```

---

## 🚀 Getting Started on QuantConnect

1. Log in to [QuantConnect](https://www.quantconnect.com)
2. Create a new **Algorithm Lab** project
3. Copy the contents of `main.py` into the editor
4. Click **Build** to compile the project
5. Click **Backtest** to run the simulation

---

## 📈 Strategy Logic Diagram

```
Tweet received
      │
      ▼
Contains "tesla" or "tsla"?
      │
   YES│                NO
      ▼                ▼
Compute VADER       score = 0
  compound             │
  score                │
      │◄───────────────┘
      ▼
  score > 0.5?  ──► Long TSLA 100%
  score < -0.5? ──► Short TSLA 100%
  else          ──► No action
      │
      ▼
15 min before close ──► Liquidate all
```

---

## ⚠️ Disclaimer

This project is for **educational and research purposes only**. It does not constitute financial advice. Past backtest performance does not guarantee future results. Always do your own research before trading.

---
