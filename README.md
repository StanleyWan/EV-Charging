# EV-Charging
Capstone Project for UC Berkeley's Professional Certification for Machine Language and Artificial Language
## Overview
The rise of electric vehicles is one of the most exciting shifts of our time. Adoption is accelerating, new models appear every year, and cities are beginning to reshape their infrastructure. Yet in many ways, this is still just the beginning of the journey. As more drivers make the switch, demand for charging stations grows rapidly. In some cities, the pressure is already visible—queues form at busy locations, and frustrations sometimes spill over.

This raises an important question: how do people actually use charging stations, and what can we learn to ease the strain? Not every driver behaves the same. Some “top up” just enough to get home, others charge halfway to cover their weekly needs, and some insist on filling the battery to 100%. In this project, I use terms like Casual Driver, Commuter, and Long-Distance Driver. But these labels point to something more fundamental: different patterns of human behavior.

At its heart, this study is not about the cars—it is about the people. What motivates them to charge fully or partially? How do cost, time of day, battery size, or station type influence their decisions? These are the questions guiding my capstone project.

To explore these behaviors, I built a machine learning model to classify charging sessions into these user types. The analysis and results are documented in [ev_charging_final_V2.ipynb](https://github.com/StanleyWan/EV-Charging/blob/main/ev_charging_final_V2.ipynb), which shows how data can reveal the hidden patterns behind everyday charging choices.

## Data Undstanding

The [dataset](https://github.com/StanleyWan/EV-Charging/blob/main/data/Global_EV_Charging_Behavior_2024.csv) used in this project was obtained from [Kaggle](https://www.kaggle.com/datasets/atharvasoundankar/global-ev-charging-behavior-2024). Although relatively small in size, it is complete — there are no missing values — which makes it convenient for exploratory analysis and model building. At first glance the data seems messy, but in fact this reflects real-world EV charging behavior rather than flaws in the dataset itself.  

For example, one might assume that more energy delivered always means higher cost. However, the graph (Cost vs. Energy Delivered),   
<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/cost%20vs%20energy%20(1).png" width="600"/><br>
  <em>Figure: Cost vs Energy Delivered</em>
</p>

it shows very high variance: at the same cost level, some drivers receive far more energy than others. This is a result of vendor incentive plans, such as free or discounted charging packages.  

Similarly, one might expect that longer duration means more energy charged. But the second graph (Duration vs. Energy Delivered),

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/duration_vs_energy_by_station_type.png" width="600"/><br>
  <em>Figure: Cost vs Energy Delivered</em>
</p>

shows it could be unrelated. Many sessions with long durations actually delivered little energy. This reflects idle charging sessions, vehicle limitations, or charger speed constraints.  

Finally, the third graph (Charging Outcome Counts),  

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/outcome_counts.png" width="600"/><br>
  <em>Figure: Cost vs Energy Delivered</em>
</p>

it shows that about one third of charging sessions ended in failure or were aborted. Rather than machine error, this often comes from user inattention or misoperation.
Together, these observations highlight that EV charging behavior is complex, shaped not just by technical limits but also by human choices and vendor policies
