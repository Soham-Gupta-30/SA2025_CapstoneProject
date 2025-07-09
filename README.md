Dynamic Pricing for Urban Parking Lots

This project implements a real-time dynamic pricing system for urban parking lots using historical and simulated streaming data. The objective is to optimize parking prices based on demand patterns and contextual features, ensuring fair usage and revenue maximization.

Both models are built using Pathway, a stream-processing framework, and visualized using Bokeh to simulate real-time behaviour. The system is designed to be extendable to live streaming data sources in a production environment.

Detailed Project Architecture & Workflow
1. Data Ingestion
  •	The project starts with a dataset (dataset.csv) containing real-world parking lot usage data including fields like occupancy, capacity, timestamp, traffic  level, and vehicle type.
________________________________________
2. Pathway Data Pipeline
All transformations are handled using the Pathway framework.

 •	The dataset is ingested as a table with a defined schema.

 •	Each row is enhanced with parsed timestamps and additional derived features like:

   o	Hour of the day

   o	Peak hour flag

   o	Traffic condition score

   o	Vehicle weight mapping

This unified pipeline is the core of our processing logic.
________________________________________
 
3. Pricing Models
 Model 1: Occupancy-Based Pricing

      •	Computes price using:
   
            Price = BasePrice + α × (Occupancy / Capacity)
   
      •	Simple and reactive — prices increase as the lot fills up.
   
      • No other external features are considered.
   
    Key Assumptions:
   
      • Base price is ₹10.0, and α (slope) is set to 3.0.
________________________________________

 4. Model 2: Demand-Based Pricing
      •	Calculates a weighted demand score using:
    
            Demand = α × (Occupancy/Capacity) + β × QueueLength - γ × TrafficScore  + δ × IsSpecialDay + ε × VehicleWeight
    
      This raw demand is then normalized and mapped into price using:
    
            Price = BasePrice × (1 + λ × NormalizedDemand), bounded between ₹9 and ₹15
    
    Key Assumptions:
    
      •	Base price is ₹10.0.
    
      •	Normalized demand range is mapped from raw values between 0 and 10.
    
      •	Final price is clipped to stay within ₹9–₹15 for fairness and smoothness.
    
      •	Demand function components are weighted using domain-tuned coefficients.
________________________________________
5. Output Generation
   •	Model outputs are saved to CSV:
   
      o	output_model1.csv for Model 1
   
      o	output_model2.csv for Model 2
   •	These files contain:
   
      o	Parsed timestamp
   
      o	Lot ID
   
      o	Final calculated price
________________________________________

6. Visualization with Bokeh
  •	For each parking lot, Bokeh is used to create:
   
   A line + scatter plot of price vs time
      
   Interactive hover tool for inspection
  •	We can compare:
   
   o	Model 1 vs Model 2 on a shared axis
      
   o	Each lot’s price behavior individually
      
   This visualization helps explain the pricing logic to non-technical stakeholders.
________________________________________


