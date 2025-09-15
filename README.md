# EV-Charging
Capstone Project for UC Berkeley's Professional Certification for Machine Language and Artificial Language
## Overview
The rise of electric vehicles is one of the most exciting changes of our time. Adoption is accelerating, new models appear every year, and cities are beginning to shift their infrastructure. But in many ways, this is still just the beginning of the journey. As more drivers make the switch, the demand for charging stations grows rapidly. In some cities, this pressure is already visible—queues form at busy locations, and frustrations sometimes spill over.
This raises an important question: how do people actually use charging stations, and what can we learn to ease the strain? Not every driver behaves the same. Some “top up” only a little to make it home, others charge halfway to cover the week, and some insist on filling the battery to 100%. In my project, I use terms like Casual Driver, Commuter, and Long-Distance Driver, but these labels really point to something simpler: different patterns of human behavior.
At its heart, this study is not about the car—it is about the people. What motivates them to charge fully or partially? How do cost, time of day, battery size, or station type influence their decisions? These are the questions that guide my capstone project.  

In order to explore these behaviors more deeply, I built a machine learning model to classify charging sessions into these user types. The analysis and results are documented in my notebook [ev_charging_final_V2.ipynb](https://github.com/StanleyWan/EV-Charging/blob/main/ev_charging_final_V2.ipynb), which demonstrates how data can help us uncover patterns in everyday charging decisions.

## Data Undstanding

The [dataset](https://github.com/StanleyWan/EV-Charging/blob/main/data/Global_EV_Charging_Behavior_2024.csv) used in this project was obtained from Kaggle. Although relatively small in size, it is complete — there are no missing values — which makes it convenient for exploratory analysis and model building. At first glance the data seems messy, but in fact this reflects real-world EV charging behavior rather than flaws in the dataset itself.
For example, one might assume that more energy delivered always means higher cost. However, the first graph (Cost vs. Energy Delivered) shows very high variance: at the same cost level, some drivers receive far more energy than others. This is a result of vendor incentive plans, such as free or discounted charging packages.
Similarly, one might expect that longer duration means more energy charged. But the second graph (Duration vs. Energy Delivered) shows that many sessions with long durations actually delivered little energy. This reflects idle charging sessions, vehicle limitations, or charger speed constraints.
Finally, the third graph (Charging Outcome Counts) shows that about one third of charging sessions ended in failure or were aborted. Rather than machine error, this often comes from user inattention or misoperation.
Together, these observations highlight that EV charging behavior is complex, shaped not just by technical limits but also by human choices and vendor policies
