# EV-Charging
Capstone Project for UC Berkeley's Professional Certification for Machine Language and Artificial Language
## Overview
The rise of electric vehicles is one of the most exciting shifts of our time. Adoption is accelerating, new models appear every year, and cities are beginning to reshape their infrastructure. Yet in many ways, this is still just the beginning of the journey. As more drivers make the switch, demand for charging stations grows rapidly. In some cities, the pressure is already visible—queues form at busy locations, and frustrations sometimes spill over.

This raises an important question: how do people actually use charging stations, and what can we learn to ease the strain? Not every driver behaves the same. Some “top up” just enough to get home, others charge halfway to cover their weekly needs, and some insist on filling the battery to 100%. In this project, I use terms like Casual Driver, Commuter, and Long-Distance Driver. But these labels point to something more fundamental: different patterns of human behavior.

At its heart, this study is not about the cars—it is about the people. What motivates them to charge fully or partially? How do cost, time of day, battery size, or station type influence their decisions? These are the questions guiding my capstone project.

To explore these behaviors, I built a machine learning model to classify charging sessions into these user types. The analysis and results are documented in [ev_charging_final_V2.ipynb](https://github.com/StanleyWan/EV-Charging/blob/main/ev_charging_final_V2.ipynb), which shows how data can reveal the hidden patterns behind everyday charging choices.

## 2. Business Understanding

Before diving into data, it is critical to first understand the business context. Without understanding how EV charging services are provided, data alone can be misleading. At first glance, we might assume:

- Cost is directly proportional to energy delivered.  
- Longer charging duration means more energy delivered.
- High failure rates must reflect poor charger quality.  

However, these assumptions do not always hold true. Vendors often design pricing packages, discounts, or subscription schemes that distort the direct link between cost and energy. Drivers may leave cars idle at the charger, making duration a poor predictor of energy delivered. And failures are not always technical problems; many are caused by human inattention or misoperation.  

The graphs from our preliminary analysis reinforce these insights:

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/cost%20vs%20energy%20(1).png" width="600"/><br>
  <em>Figure: Cost vs Energy Delivered</em>
</p>
**Figure Cost vs Energy Delivered: shows Cost and energy delivered are related, but the variance is large, distorting price as a predictor.**    

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/duration_vs_energy_by_station_type.png" width="600"/><br>
  <em>Figure: Duration vs Energy Delivered</em>
</p>
**Figure 2: Duration as Energy Delivered:  shows little relationship with energy delivered, reflecting idle time or station type.**  

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/outcome_counts.png" width="600"/><br>
  <em>Figure: Outcome Counts</em>
</p>
**Figure Outcome Counts: Failed sessions are common and should be considered normal in usage data, not evidence of poor equipment.**  

These findings confirm that simple assumptions are insufficient. To truly understand charging behavior, we need to capture underlying patterns of human decision-making. That is why classification modeling—categorizing drivers into Casual, Commuter, and Long-Distance types—offers a strong approach to this project.  

## 2. Data Understanding  

The dataset used in this project is [Global_EV_Charging_Behavior_2024](https://github.com/StanleyWan/EV-Charging/blob/main/data/Global_EV_Charging_Behavior_2024.csv), downloaded from [Kaggle](https://www.kaggle.com/datasets/atharvasoundankar/global-ev-charging-behavior-2024). It contains ~800 rows × 16 columns, with each row representing one EV charging session. The data is small in size but clean — there are no missing values — and it provides a realistic picture of charging patterns across different regions.  

### Key characteristics  
- **Size:** ~800 sessions, 16 attributes  
- **Scope:** Global, reflecting diverse charging environments  
- **Granularity:** Session-level data, one row per charging session  

### Important columns used  
- **Charging Start Time:** Timestamp, used to derive *Time of Day* and *Day Type*.  
- **Battery Capacity (kWh):** Vehicle’s full battery size, baseline for SOC (state of charge).  
- **Energy Delivered (kWh):** Actual energy charged; used for labeling but excluded as a feature.  
- **Charging Cost ($):** Session cost, shaped by vendor packages and incentives.  
- **Payment Method:** {Subscription, Card, App}; proxy for billing structure and package plans.  
- **Charging Station Type:** {Level 1, Level 2, DC Fast}; reflects charging speed and user patience.  
- **Session Outcome:** Complete / Abort / Fail; excluded since energy delivered already reflects results.  

### Label creation (UserType)  
- < 20% SOC → *Casual Driver*  
- 20–60% SOC → *Commuter*  
- (> 60% SOC → *Long-Distance Traveler*)  

### Early observations  
- **Cost vs. Energy:** Related but highly variable, distorted by discounts and subscriptions.  
- **Duration vs. Energy:** Weak relationship; long sessions often reflect idle time.  
- **Session outcomes:** Over 30% end in failure or abort, often due to user behavior rather than machine faults.  

## 4. Data Preprocessing / Preparation  

To prepare the dataset for modeling, several preprocessing steps were applied. The goal was to ensure data quality, avoid data leakage, and transform features into a format suitable for machine learning.  

### a. Cleaning and handling inconsistencies  
- The dataset contained **no missing values**, which reduced the need for imputation.  
- Redundant or leakage-prone features were dropped, including:  
  - **Energy Delivered (kWh):** used only for labeling user type.  
  - **SOC Change (%):** derived from energy delivered and capacity.  
  - **Charging Duration (mins):** dropped because it does not reliably correlate with energy delivered. 
  Long durations often reflect idle time or user inattention rather than actual charging behavior, 
  making it a misleading feature. 
  - **Charge Rate (kW):** redundant with station type.  
- Outliers were reviewed through exploratory analysis (e.g., unusually high cost or duration values). These were retained to preserve real-world variance.  

### b. Train-test split  
- The dataset was split into **80% training** and **20% testing**.  
- Stratification was applied on the **UserType** target to maintain class balance in both sets.  

### c. Feature engineering and encoding  
- **Derived features:**  
  - *TimeOfDay* (Morning, Office Hours, Evening, Night, Deep Night) from start time.  
  - *DayType* (Weekday / Weekend) from day of week.  
  - *Cost per kWh* to capture cost efficiency, based on session cost and energy delivered.  
- **Target variable:**  
  - *UserType* created from SOC% change thresholds (<20% Casual, 20–60% Commuter, >60% Long-Distance).  
- **Encoding:**  
  - Numerical features (e.g., Charging Cost, Battery Capacity) scaled with **StandardScaler**.  
  - Categorical features (Station Type, TimeOfDay, DayType, Payment Method) transformed using **One-Hot Encoding**.  
- **Pipeline approach:** All transformations were built into a **ColumnTransformer** inside a Scikit-learn pipeline, ensuring consistent application during both training and testing.  

Overall, these steps ensured that the dataset was clean, non-leaky, and properly structured for classification modeling.  

## 5. Modeling  

The goal of this project is to classify EV charging sessions into **Casual Driver, Commuter, or Long-Distance Driver** categories based on behavioral features. To achieve this, I began with simple baselines and then evaluated multiple supervised classification algorithms.  

### Baseline modeling  
Before training advanced models, I built baseline classifiers to test the influence of **charging cost**:  
- **Baseline 1:** Model trained without cost-related features.  
- **Baseline 2:** Model trained with cost included.  

The comparison showed a clear improvement in predictive performance when cost was included, confirming that **charging cost is a key driver of user behavior**. This reflects the business reality that many vendors provide free sessions, discounts, or subscription packages, which strongly influence charging decisions.  

### Candidate models  
- **Logistic Regression (One-vs-Rest):**  
  Selected as a baseline supervised model. It is interpretable, relatively fast to train, and provides probability estimates that can be evaluated with ROC curves and AUC metrics.  

- **Support Vector Machine (SVM):**  
  Effective for high-dimensional feature spaces created by one-hot encoding. Kernel tuning allowed the model to capture nonlinear relationships in the data.  

- **Decision Tree:**  
  Offers interpretability and feature importance, useful for understanding what drives user classifications. Overfitting was controlled with pruning parameters (`ccp_alpha`, max depth).  

- **K-Nearest Neighbors (KNN):**  
  Tested as a non-parametric method. Results were weaker compared to other models, so KNN was excluded from the final recommendation.  

### Pipeline setup  
All models were trained within a **Scikit-learn pipeline** that included:  
1. Scaling of numerical features (e.g., cost, battery capacity).  
2. One-hot encoding of categorical features (e.g., station type, time of day, day type).  
3. Cross-validation with **GridSearchCV** for hyperparameter tuning.  

### Rationale  
By starting with baselines and then testing multiple algorithms, I ensured the analysis identified both the **most important driver of behavior (cost)** and the **most effective model**. This balanced approach combined interpretability (Logistic Regression, Decision Tree) with strong performance (SVM), ensuring the final results are both accurate and explainable in a business context.  

## 6. Model Evaluation  

To evaluate the models, I used three complementary metrics:  

- **Accuracy:** Overall proportion of correctly classified sessions.  
- **Macro F1-score:** Balances precision and recall across all three user types (Casual, Commuter, Long-Distance).  
- **Macro AUC (OvR):** Measures overall ability to distinguish between classes using probability estimates.  

### Baseline results  
- **Without cost:** Accuracy and F1 were noticeably lower.  
- **With cost:** Both metrics improved significantly, confirming that **cost is a critical feature** for modeling charging behavior.

### Baseline model comparison  

| Metric       | Without Cost | With Cost |
|--------------|--------------|-----------|
| **Accuracy** | 0.450        | 0.850     |
| **Macro F1** | 0.311        | 0.812     |
| **Casual – Precision / Recall / F1** | 0.00 / 0.00 / 0.00 | 0.87 / 0.59 / 0.70 |
| **Commuter – Precision / Recall / F1** | 0.41 / 0.35 / 0.38 | 0.82 / 0.82 / 0.82 |
| **Long-Distance – Precision / Recall / F1** | 0.47 / 0.68 / 0.56 | 0.87 / 0.96 / 0.91 |

**Key takeaway:** Including cost increases accuracy from **45% → 85%** and Macro-F1 from **0.31 → 0.81**. This confirms that **charging cost is the single most important predictor of EV charging behavior.**  

### Final model performance (test set)  
| Model                | Accuracy | Macro F1 | Macro AUC (OvR) |
|----------------------|----------|----------|-----------------|
| Logistic Regression  | 0.913    | 0.916    | 0.977           |
| Support Vector Machine (SVM) | 0.894    | 0.880    | 0.981           |
| Decision Tree        | 0.838    | 0.848    | 0.900           |
| KNN (excluded)       | 0.688    | 0.651    | 0.720           |

### Key takeaways  
- Logistic Regression and SVM both performed strongly, with **Logistic Regression selected as the final model** due to its balance of performance and interpretability.  
- Decision Tree offered insights into feature importance but underperformed compared to other models.  
- KNN consistently lagged in both accuracy and stability, so it was excluded from further analysis.  
- Across all models, the inclusion of **cost** consistently improved predictive power, reinforcing its role as the primary driver of charging behavior.  

The final chosen model, Logistic Regression, provides high accuracy and F1 while remaining interpretable—an important factor for business stakeholders seeking to understand what drives EV user behavior.  

## 7. Summary of Findings  

In this project, EV drivers were classified into three categories based on how much of their battery was charged in a single session:  
- **Casual Driver:** charges less than 20% of the battery capacity.  
- **Commuter:** charges between 20% and 60% of the battery capacity.  
- **Long-Distance Driver:** charges more than 60% of the battery capacity.  

The analysis showed that charging behavior is influenced by multiple factors, including **charging station type, time of day, and weekday vs. weekend patterns**. However, the most important finding is that **charging cost is the dominant predictor**. Models trained with cost significantly outperformed those without it, providing strong evidence that vendor packages, discounts, and subscriptions play a central role in shaping charging behavior.  

## 8. Conclusion  

The results confirm that EV charging is not only a technical process but also an economic and human one. While battery size and station type matter, **pricing incentives and user habits are the strongest drivers of behavior**. Casual drivers often “top up” only a little, commuters balance convenience and cost, and long-distance drivers fully recharge to maximize range.  

This insight is valuable for city planners, EV vendors, and charging providers: understanding human charging behavior can reduce strain on stations, improve customer satisfaction, and guide smarter infrastructure investment.  

## 9. Next Steps / Future Work  

- **Expand the dataset:** Incorporate more sessions and data sources to improve generalizability.  
- **Include external features:** Such as trip distance, geographic distribution of stations, or user demographics.  
- **Study pricing packages explicitly:** Vendor plans strongly affect behavior, but detailed package data was not available in this dataset.  
- **Explore unsupervised methods:** Clustering may reveal hidden user patterns beyond the predefined categories.  

By continuing this work, future research can provide deeper insights into the interaction between human behavior, vendor incentives, and charging infrastructure.  
