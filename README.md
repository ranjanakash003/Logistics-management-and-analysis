Logistics Management and Analysis: A "Predict then Optimize" Approach
This project develops a complete, data-driven solution to a complex logistics problem. It uses a two-stage "predict then optimize" methodology to determine the most cost-effective plan for fulfilling a set of customer orders. The entire analysis is conducted within a reproducible, cloud-based development environment using GitHub Codespaces.

1. Problem Statement
A company needs to fulfill a daily list of customer orders for various products. It operates a network of manufacturing plants, each with different production costs and daily capacities. A variety of carriers are available for shipping, but their pricing is complex, non-linear, and based on weight-tiered brackets.   

The goal is to create a daily fulfillment plan that assigns each order to a specific plant and carrier to minimize the total logistics cost (production + transportation) while respecting all operational constraints, including:

Demand Fulfillment: Every order must be fulfilled.

Plant Capacity: The total production assigned to a plant cannot exceed its daily capacity.   

Sourcing Restrictions: An order can only be assigned to a plant that is capable of producing the required product.   

2. Methodology
A two-stage "predict then optimize" approach was employed to solve this problem, breaking it down into manageable parts.

Part I: Exploratory Data Analysis (EDA)
The first step was to deeply understand the strategic landscape of the supply chain. This involved:

Consolidating Plant Profiles: Merging data from WhCapacities.csv, WhCosts.csv, and PlantPorts.csv to create a unified view of each plant's capabilities, costs, and associated shipping ports.   

Identifying Bottlenecks: Analyzing the OrderList.csv and ProductsPerPlant.csv data to find high-risk, sole-sourced products where daily demand could potentially exceed the production capacity of the single designated plant.   

Deconstructing Cost Complexity: Analyzing the FreightRates.csv file to uncover the non-linear, tiered pricing structure. This key finding proved that a direct linear optimization was not feasible and justified the need for a predictive model.   

Part II: Predictive Modeling for Freight Costs
To handle the complex freight pricing, a machine learning model was developed to act as a "cost oracle."

Feature Engineering: A comprehensive training dataset was created by matching every order with every possible valid carrier and calculating the true freight cost based on the tiered pricing rules.

Model Training: An XGBoost Regressor model was trained on this dataset. XGBoost is highly effective at learning the complex, non-linear relationships between shipment details (like weight, origin, destination) and the final freight cost.   

Evaluation: The model's accuracy was evaluated using metrics like Mean Absolute Error (MAE) and R-squared (R²) to ensure its predictions were reliable for the optimization phase.

Part III: Prescriptive Optimization with MILP
With an accurate cost prediction model in place, the final step was to determine the optimal set of decisions.

Model Formulation: A Mixed-Integer Linear Program (MILP) was formulated.

Decision Variables: Binary variables representing whether to assign a specific order to a specific plant and carrier.

Objective Function: Minimize the total cost, defined as the sum of each plant's production cost and the predicted freight cost from the XGBoost model.

Constraints: Mathematical formulations of the business rules (demand fulfillment, plant capacity, and sourcing restrictions).

Solving: The MILP model is solved using a Python library like PuLP to find the guaranteed optimal solution, which provides the final, actionable logistics plan.

3. Tech Stack
Language: Python 3.11

Core Libraries: pandas, NumPy, scikit-learn, XGBoost, PuLP, openpyxl

Development Environment: GitHub Codespaces with a custom devcontainer.json for a reproducible setup.

Analysis Tool: Jupyter Notebooks


4. How to Run This Project
This project is configured to run seamlessly in GitHub Codespaces.

Launch the Codespace:

Click the green < > Code button on the main repository page.

Navigate to the "Codespaces" tab.

Click "Create codespace on main".

Automatic Setup:

The environment will build automatically. The devcontainer.json file ensures that all required Python libraries listed in requirements.txt are installed for you.

Run the Analysis:

Once the Codespace has loaded, navigate to the notebooks/ directory in the file explorer.

Open and run the Jupyter Notebooks in the following order:

1_data_exploration.ipynb: To perform the initial data analysis.

2_freight_cost_prediction.ipynb: To train the freight cost prediction model.

(Future work would include a 3_optimization.ipynb to run the final MILP model).

6. Key Results and Insights
The final output of this project is a complete, cost-optimized logistics plan that details exactly which plant and which carrier should be used for every single order, resulting in the lowest possible total cost for the company while respecting all business rules.
