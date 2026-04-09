# Synthetic Dashboard Dataset (NVBench-based)

This repository contains a synthetic dataset for evaluating **automatic dashboard generation systems**. The dataset is constructed by composing natural language queries and visualization specifications derived from NVBench into **multi-chart dashboard scenarios**.

---

## Dataset File

- `NVBench_synthetic_dashboards_data.json`

---

## Purpose

This dataset is designed to support:

- Evaluation of NL2VIS systems in multi-chart settings  
- Benchmarking dashboard composition reasoning  
- Testing agent-based visualization pipelines  
- Controlled experiments on layout, chart selection, and multi-view consistency

Unlike traditional datasets that focus on **single-chart generation**, this dataset emphasizes **multi-chart dashboard construction**, which introduces additional complexity in:

- Layout composition  
- Cross-view coordination  
- Query decomposition  

---

## Methodology Overview

The dataset is constructed following the methodology described below

### 1. Source Data: NVBench

- Base queries and chart specifications are derived from **NVBench (Luo et al., 2021)** 
- Each instance originally represents a **single NL to Vega-Lite mapping**

---

### 2. Dashboard Composition Strategy

To extend single-chart tasks into dashboards:

- Multiple NVBench queries are **combined into a single test case**
- All charts within a dashboard share the **same database (`db_id`)**
- Ensures **semantic consistency across views**

---

### 3. Layout Constraints

Dashboard layouts are constructed using **Vega-Lite concatenation operators only**:

- `hconcat`: horizontal composition  
- `vconcat`: vertical composition  

The dataset intentionally excludes:
- layering  
- faceting  
- repetition  

This constraint ensures:
- Controlled evaluation  
- Reduced compositional complexity  
- Focus on core dashboard layouts  

---

### 4. Layout Templates

The dataset systematically enumerates dashboard structures:

| # Charts | Layout Type | Description |
|----------|------------|-------------|
| 1 | `single` | Single chart |
| 2 | `hconcat` / `vconcat` | Horizontal or vertical |
| 3 | `triple_h_then_v` | (2 horizontal) + (1 below) |
| 3 | `triple_v_then_h` | (2 vertical) + (1 right) |
| 4 | `grid` (2×2) | Two rows × two columns |

Additionally:
- Maximum **2 charts per row**
- Ensures consistent grid-like structure

---

### 5. Query Composition

- Each dashboard includes **multiple natural language sub-queries**
- Queries are concatenated into a **single NL input**
- Order of sub-queries is **permuted** to test:
  - layout sensitivity  
  - reasoning robustness  

---

### 6. Data Consistency

For each chart:

- Corresponding **aggregated data values** are precomputed  
- Stored explicitly in `data_values`  
- Ensures:
  - deterministic rendering  
  - no dependency on runtime SQL execution  

---

### 7. Output Representation

Each dashboard is represented as a **Vega-Lite specification**:

- Single chart: standard spec  
- Multi-chart: nested `hconcat` / `vconcat` structures  

---

## Data Schema

Each JSON entry follows this structure:

```json
{
  "test_case": "T0001",
  "db_id": "activity_1",
  "num_charts": 1,
  "layout_type": "single",
  "nl_queries": "...",
  "data_values": "...",
  "chart_types": "...",
  "vega_lite_spec": "...",
  "input_ids": "..."
}
```
---

## License

This dataset is released under the MIT License.