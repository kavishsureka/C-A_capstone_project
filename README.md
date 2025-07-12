# Dynamic Pricing for Urban Parking Lots

This project implements a real-time **dynamic pricing engine** for urban parking lots using simulated streaming data. The solution uses a rule-based and demand-driven pricing approach to adjust parking prices based on real-time conditions like occupancy, traffic, queue length, and vehicle type. It also includes visualization using Bokeh and supports future integration with real-time data streams using Pathway.

---

## 🔧 Tech Stack

| Tool/Library  | Purpose                          |
|---------------|----------------------------------|
| Python        | Core programming language        |
| Pandas        | Data preprocessing and analysis  |
| NumPy         | Numerical computation            |
| Bokeh         | Real-time data visualization     |
| Google Colab  | Cloud-based execution environment|
| Pathway       | (Optional) Streaming data engine |

---

##  Project Models

### Model 1: Baseline Linear Pricing
Price increases linearly with occupancy ratio.

\[
P_{t+1} = P_t + \alpha \cdot \left(\frac{\text{Occupancy}}{\text{Capacity}}\right)
\]

### Model 2: Demand-Based Pricing
Price depends on multiple real-time features:

\[
\text{Demand} = \alpha \cdot \left(\frac{\text{Occupancy}}{\text{Capacity}}\right) + \beta \cdot \text{QueueLength} - \gamma \cdot \text{Traffic} + \delta \cdot \text{IsSpecialDay} + \epsilon \cdot \text{VehicleTypeWeight}
\]

Then:
\[
P_t = \text{BasePrice} \cdot \left(1 + \lambda \cdot \text{NormalizedDemand}\right)
\]

---

##  Architecture Diagram (Mermaid)

```mermaid
flowchart TD
    A[dataset.csv] --> B[Data Preprocessing (Pandas)]
    B --> C1[Model 1: Linear Pricing]
    B --> C2[Model 2: Demand-Based Pricing]
    C1 --> D[Result Table]
    C2 --> D
    D --> E[Bokeh Visualization]
    D --> F[Optional Pathway Stream Engine]
```
## 🔁 Project Workflow (Step-by-Step)

This section explains how the notebook works — from data ingestion to pricing and visualization — in a simple, human-readable format.

---

### 1.  Data Ingestion

- The notebook starts by uploading a historical dataset (`dataset.csv`) to Google Colab.
- This dataset contains parking lot data collected over 73 days at 30-minute intervals.
- Key columns include:
  - `Occupancy`: Number of parked vehicles
  - `Capacity`: Maximum allowed vehicles
  - `VehicleType`: Type of incoming vehicle (car, bike, truck)
  - `QueueLength`: Number of vehicles waiting
  - `TrafficConditionNearby`: Traffic level near the lot
  - `IsSpecialDay`: 1 if it’s a holiday or event day, otherwise 0
- A proper `Timestamp` is constructed by combining `LastUpdatedDate` and `LastUpdatedTime`.

---

### 2. Data Preprocessing

- The dataset is sorted in chronological order using the new `Timestamp`.
- Vehicle types are mapped to numeric weights:
  - Car = 1.0
  - Bike = 0.5
  - Truck = 1.5
- This helps the model calculate demand based on vehicle type.

---

### 3.  Model Execution

#### ✅ Model 1: Baseline Linear Pricing
- A simple model where the price increases linearly as occupancy rises.
- Formula:
Price_t+1 = Price_t + α × (Occupancy / Capacity)

#### ✅ Model 2: Demand-Based Pricing
- A more intelligent model that considers multiple real-time factors like:
- Occupancy rate
- Queue length
- Traffic condition
- Whether it's a special day
- Type of vehicle
- Demand is calculated and then normalized.
- Formula:

Final Price = Base Price × (1 + λ × Normalized Demand)

- The price is then capped between $5 and $20 for stability.

---

### 4.  Real-Time Visualization

- The final pricing values from both models are visualized using **Bokeh**.
- Interactive line plots show how pricing evolves over time for both models.
- Helps visually justify and validate pricing behavior.

---

### 5.  (Optional) Real-Time Simulation with Pathway

- The notebook includes a placeholder for integrating **Pathway**, a real-time data streaming framework.
- This allows future extensions where vehicle data can stream in live and pricing updates in real time.
- The final goal is to build a live pricing engine that can handle incoming data like a real-world system.

---

📌 **In Summary**:
The project moves from simple price logic to smarter, demand-driven models — all while keeping the code clean, visual, and expandable for real-time use.

