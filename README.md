# EV-Charging
Capstone Project for UC Berkeley's Professional Certification for Machine Language and Artificial Language
## Overview
The rise of electric vehicles is one of the most exciting shifts of our time. Adoption is accelerating, new models appear every year, and cities are beginning to reshape their infrastructure. Yet in many ways, this is still just the beginning of the journey. As more drivers make the switch, demand for charging stations grows rapidly. In some cities, the pressure is already visible—queues form at busy locations, and frustrations sometimes spill over.

This raises an important question: how do people actually use charging stations, and what can we learn to ease the strain? Not every driver behaves the same. Some “top up” just enough to get home, others charge halfway to cover their weekly needs, and some insist on filling the battery to 100%. In this project, I use terms like Casual Driver, Commuter, and Long-Distance Driver. But these labels point to something more fundamental: different patterns of human behavior.

At its heart, this study is not about the cars—it is about the people. What motivates them to charge fully or partially? How do cost, time of day, battery size, or station type influence their decisions? These are the questions guiding my capstone project.

To explore these behaviors, I built a machine learning model to classify charging sessions into these user types. The analysis and results are documented in [ev_charging_final_V2.ipynb](https://github.com/StanleyWan/EV-Charging/blob/main/ev_charging_final_V2.ipynb), which shows how data can reveal the hidden patterns behind everyday charging choices.

2. Business Understanding

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
Figure Cost vs Energy Delivered: shows Cost and energy delivered are related, but the variance is large, distorting price as a predictor.  


 
  

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/duration_vs_energy_by_station_type.png" width="600"/><br>
  <em>Figure: Duration vs Energy Delivered</em>
</p>
Figure 2: Duration as Energy Delivered:  shows little relationship with energy delivered, reflecting idle time or station type.  



  

<p align="center">
  <img src="https://raw.githubusercontent.com/StanleyWan/EV-Charging/main/images/outcome_counts.png" width="600"/><br>
  <em>Figure: Outcome Counts</em>
</p>
Figure Outcome Counts: Failed sessions are common and should be considered normal in usage data, not evidence of poor equipment.  


 
  
These findings confirm that simple assumptions are insufficient. To truly understand charging behavior, we need to capture underlying patterns of human decision-making. That is why classification modeling—categorizing drivers into Casual, Commuter, and Long-Distance types—offers a strong approach to this project.  



