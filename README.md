# Comparison-of-2-Linear-Regression-Models-based-on-features-and-upgrading-with-network-features-
The scenario for this project is the Hospital infection control. The main question is, if we can track transfers of patient between different wards 14-days before a studying/observation point that we set it as the time = 0, can we predict for the next 14 days, which clusters they will get infected if there are some active ones at the time = 0? We constructed two linear regression models for comparison, one with features related to the problem of the wards and then upgrading the model with network features to see if those can help with the predictions.

Disclaimer: This is a final project for complex systems and applications seminars @AUTH 2026

# Scenario
The scenario for this project is the Hospital infection control. The main question is, if we can track transfers of patient between different wards 14-days before a studying/observation point that we set it as the time = 0, can we predict for the next 14 days, which clusters they will get infected if there are some active ones at the time = 0?  

# Data
The synthetic dataset is available online @: 'https://raw.githubusercontent.com/mikeauth/leap/main/hospital'
It includes in total 120 Wards, representing as nodes for the aspect of network analysis, for those not familiar to network analysis, and 420 established direct transfers between wards as links, for the network and doesn't reflect real hospital data. It made only for educational purposes.

The details for the dataset for those interested in it are the following:
Wards: **ward_id** Unique identifier W001 to W120; for joining, never a predictor.
Wards: **occupancy_ratio** Simulated occupied-bed fraction, drawn uniformly from 0.48 to 0.96, rounded to three
decimals.
Wards: **device_use_share** Simulated fraction of patients with an invasive device, drawn uniformly from 0.07 to
0.48, rounded to three decimals.
Wards: **active_cluster_at_start** Exactly 18 uniformly chosen wards have value 1 at the observation point.
Wards: **new_cluster_later** Later binary outcome drawn only for the other 102 wards; blank for the 18 initially
affected wards. There are 39 later positives in this fixed release.
Transfers: **source_ward_id**,
**destination_ward_id**
Ordered sender and receiver for a pre-observation transfer connection.
Transfers: **transfer_count** Synthetic number of transfers on that ordered link during the recording window,
integer 1 to 9.

The generated output target vector was generated from the following equation:
-1.12 + 0.62z(occupancy_ratio_i) + 0.54z(device_use_share_i) + 0.24z(incoming_volume_i) +
0.86z(transfers_from_active_wards_i)

There are **18 active clusters early** and **39 later positive cluster** in this release.

# Model
Linear Regression Models from scikit-learn python library
1st model: Based only on important features of the wards, **occupancy_ratio** + **device_use_share**
2nd model: After the network analysis, the **calculated incoming** and **active volume **

The calculated incoming volume is based on the in-degree links, active volume is the sum of those only from active clusters spotted in wards at the observation point, time = 0. 

As preprocessing steps was constructed a **StandardScaler pipeline**.

# Evaluation
For the evaluation of the models, there were used the **Accuracy**, **F1 Score**, **ROC AUC Score**
We describe only the ROC AUC score: it is the sensitivity to different thresholds in the final probabilities output predicted by the Logistic Regression Model, it shows a more stable distinction between classes for the model.

# Limitations
This is a simple and small synthetic dataset only for educational purposes. The small dataset provides limitations to the learning for the model, and the fixed output vector can be unrealistic in respect of real-world situations.

# Results and Ethics
The results and ethics for deploying these models to real settings are provided in great detail in the notebook, that you can find in this repository. The main results were that the F1 and ROC AUC scores of the updated model with the network features improved to 0.737 and 0.904 from 0.435 and 0.693 respectively and not an important difference was showed in the true positives cases as displayed in the confusion matrices of the two models (You can find everything in the notebook)

With more time, maybe we could calculate more features acquired by network analysis, that make sense in a real-world scenario and add them meaningfully to see if there is any improvement of the models. The most important parameter for someone to target we consider is the finding of true positives, as all other cases, are not very good indicators to take measures to minimize spreads.


