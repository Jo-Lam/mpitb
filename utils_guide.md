# 📦 MPI Results Utilities Guide

This guide describes the utility functions available in `utils.py`, designed to help summarize, filter, and reshape MPI estimation results.

---

## 🧾 Core Measure Extractors

### `extract_core_measures(*dfs, model_col="wgts", measures=["H", "A", "M0"])`

Extracts core MPI measures (`H`, `A`, `M0`) from one or more result DataFrames and reshapes them into a summary table by model.

**Parameters:**
- `*dfs`: one or more DataFrames (e.g., from `mpi.est()`).
- `model_col`: column identifying models (default: `"wgts"`).
- `measures`: list of MPI measures to include.

**Returns:** A single summary table with columns like `H_b`, `H_se`, `A_b`, `M0_ul`, etc.

---

## 🕒 Time-Based Utilities

### `extract_and_sort_by_time(results_dict, frame_name="myresults", measure="H", k=None)`

Filters and sorts national results over time (`subg` = time) for a given measure and `k`.

**Returns:** A time-sorted DataFrame with `t`, `measure`, `k`, and `b`.

---

### `extract_and_pivot_all_stats_by_time(results_dict, frame_name="myresults", measure="H", k=None)`

Returns a wide-format table over time, with columns:
- `{model}_b`, `{model}_se`, `{model}_ll`, `{model}_ul`

Useful for comparing multiple models over time.

---

## 🔄 Change-over-Time Summary Utilities

### `extract_cot_summary(...)`

Extracts a tidy table of MPI change-over-time results from a stored results frame (e.g. "mycot").

**Parameters:**
- `results_dict`: your `mpi.results` dictionary.
- `frame_name`: result frame name (e.g., `"mycot"`).
- `measure`: a string or list (e.g., `"H"` or `["H", "M0"]`).
- `k`: single int or list of cutoffs (e.g., `33` or `[20, 33, 50]`).
- `change_type`: `"total"` or `"insequence"`.
- `ann`: `True` = annual change, `False` = raw change.
- `verbose`: print filtered result preview.

**Returns:** A tidy, filtered DataFrame with columns like `measure`, `k`, `t0`, `t1`, `raw` or `ann`, and optionally `spec`, `subg`, etc.

---

### `pivot_cot_summary(...)`

Pivots COT results into wide format for comparison across models, specs, or subgroups.

**Parameters:**
- Same as `extract_cot_summary`, plus:
- `index`: what to show as row index (e.g. `["spec"]`, `["subg"]`, or `["t0", "t1"]`)
- `include_se`: also add standard errors as columns.
- `verbose`: show preview of pivoted table.

**Returns:** A wide-format DataFrame with columns like `H_k33_raw`, `M0_k50_se`, etc.

**Example:**

```python
pivot_cot_summary(
    results_dict=mpi.results,
    frame_name="mycot",
    measure=["H", "M0"],
    k=[33, 50],
    index=["spec"],
    include_se=True
)
```

---

## 🛠️ Tips

- Use `extract_*` functions to tidy and filter raw `mpi.results`.
- Use `pivot_*` functions to reshape results for comparisons.
- All utilities are safe to use with multiple cutoffs (`k=[33, 50]`).
- You can chain results from `mpi.est()` or `mpi.est_cot()`.

---