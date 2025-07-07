# Dynamic Parking Pricing Using Real-Time Demand Modeling

## 🌍 Overview

This project was developed as part of the **Bharatiya Antariksh Hackathon 2025**, under Problem Statement 3: *"Monitoring Air Pollution from Space using Satellite Observations, Ground-Based Measurements, Reanalysis Data, and AI/ML Techniques"*. However, this module specifically focuses on **Model 2** of dynamic parking pricing using ground-based observational data, such as parking occupancy, vehicle type, queue length, and traffic levels.

We implemented a real-time pricing algorithm that adjusts the daily parking price based on calculated demand using various influencing factors. The model mimics nonlinear market behavior and applies bounded pricing between a minimum and maximum threshold.

---

## ⚖️ Tech Stack

- **Python**: Core programming language
- [**Pathway**](https://pathway.com/): Real-time data processing and temporal windowing
- **Pandas**: CSV preprocessing
- **Bokeh**: Interactive plotting and visualizations
- **Panel**: Dashboard integration
- **Jupyter Notebook**: Development and documentation

---

## 📏 Architecture Diagram

```mermaid
graph TD
    A[CSV: datastream.csv] --> B[Pathway Schema Ingestion]
    B --> C[Data Parsing + Timestamp Conversion]
    C --> D[Daily Tumbling Window Aggregation]
    D --> E[Demand Function Computation]
    E --> F[Unbounded Price Calculation]
    F --> G[Price Bounding (5₹ - 20₹)]
    G --> H[Bokeh Plotting using Panel]
```

---

## ⚙️ Project Architecture & Workflow

### 1. **Data Ingestion**

- The raw data containing fields like `Timestamp`, `Occupancy`, `Capacity`, `QueueLength`, `VehicleType`, `IsSpecialDay`, and `TrafficConditionNearby` is streamed into the system using `pw.demo.replay_csv()` from Pathway.

### 2. **Time Conversion & Partitioning**

- The timestamp string is converted into datetime format.
- A daily key (`day_str`) is extracted for windowing purposes.

### 3. **Temporal Windowing**

- Using Pathway's `.windowby(...)`, the data is grouped by day into fixed 1-day tumbling windows.
- This allows us to compute daily summaries of metrics like occupancy, traffic, and queue length.

### 4. **Demand Modeling**

- A nonlinear demand function is calculated based on:
  - Occupancy range (occ\_max - occ\_min)
  - Occupancy spike factor (occ\_max / occ\_min)
  - QueueLength (normalized)
  - VehicleType (treated as ordinal, normalized)
  - TrafficConditionNearby (centered at zero)
  - IsSpecialDay (alters weights and curve shape)

### 5. **Price Generation**

- Demand is passed into a linear multiplier to get an `unbounded_price`.
- This value is clipped between ₹5 and ₹20 using `apply_with_type()`.

### 6. **Visualization**

- The final prices are visualized in a time-series line chart using **Bokeh** and **Panel**.
- Each dot represents a day; the line captures pricing trends.

---

## 📄 Additional Documentation

- `notebooks/`: Jupyter notebook with sample runs
- `visuals/`: Architecture diagrams and screenshots
- `report.pdf`: (Optional) Detailed project report

---

## 🚀 How to Run

1. Clone the repository
2. Install requirements: `pip install -r requirements.txt`
3. Launch Jupyter Notebook and run `model2_pricing.ipynb`
4. Run final cell to stream and visualize prices in real time

---

## 🚀 Future Scope

- Incorporate Model 3: Competition-aware pricing between nearby lots
- Add live satellite/reanalysis pollution influence
- Predictive modeling via regression for next-day price forecasts

