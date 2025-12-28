# Data Plan – Current Status

General idea:

```
itunes_es_daily  ─┐
                  ├─ feature engineering ─▶ itunes_es_weekly
itunes_ww_daily  ─┘

itunes_es_weekly ─┐
itunes_ww_weekly ─┼─ Feature View (weekly)
target_weekly    ─┘
```

---

### 1. We start from daily data

We collect **daily chart positions** for songs:
- Spain (main signal) Itunes Archive ex: (https://kworb.net/popes/archive/20251225.html)
- Worldwide (external context) Archive ex: (https://kworb.net/ww/archive/20110304.html)

Daily data is useful because it shows:
- how songs move up or down
- how stable or volatile they are

However, daily data is **too noisy** to use directly for prediction.

---

### 2. We convert daily data into weekly summaries

Since the prediction is **weekly**, we aggregate daily data by week. This also allow us to create better data.

For each song and each week, we compute:
- best rank of the week
- average rank
- how many days it appeared
- how much the rank changed during the week
- whether the song is a new entry

This creates **one row per song per week**.

---

### 3. We keep Spain and Worldwide data separate

- Spain data represents **local performance**
- Worldwide data represents **global trends**

They are stored separately and later combined.

This keeps the data clean and easier to understand.

---

### 4. We join everything only at the end

The final dataset used for training:
- uses weekly data only
- joins Spain + Worldwide features
- adds the target for the following week

This avoids data leakage and keeps timing correct.

---

### 5. Artist and song names are identifiers

- Stored as text
- Used to group and join data
- Not used directly by the model

The model learns from **behavior over time**, not from names.

---

### Key takeaway

> Daily data is used to observe behavior.  
> Weekly data is used to learn and predict.

