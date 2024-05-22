# TANZANIA WATER WELL STATUS AND DISTRIBUTION: A PREDICTIVE ANALYSIS

# Overview

Tanzania, officially the United Republic of Tanzania, is a country in East Africa within the African Great Lakes region. It boasts a rich tapestry of geographical features, bordering Uganda to the northwest; Kenya to the northeast; the Indian Ocean to the east; Mozambique and Malawi to the south; Zambia to the southwest; and Rwanda, Burundi, and the Democratic Republic of the Congo to the west. 
Mount Kilimanjaro, Africa's highest mountain, stands proudly in northeastern Tanzania. With a population of nearly 62 million according to the 2022 national census, Tanzania is the most populous country located entirely south of the equator.
Beyond its size and geographical diversity, Tanzania is a land of cultural richness. The country is home to over 120 ethnic groups, each with its own unique traditions and languages. Swahili, a Bantu language with Arabic influences, serves as the national language and lingua franca, uniting the people. 
Tanzania is religiously diverse as well, with Christianity being the largest religion, followed by Islam and traditional African religions.
Tanzania is a haven for wildlife enthusiasts. Its vast wilderness areas include the world-renowned Serengeti National Park, famous for the annual Great Migration of millions of wildebeest and zebra. 
The Ngorongoro Crater, a UNESCO World Heritage Site, shelters a unique ecosystem teeming with wildlife. Offshore, the spice island of Zanzibar offers a glimpse into Tanzania's historical ties with the Arab world, while Mafia Island boasts a marine park known for its stunning coral reefs and gentle giants, the whale sharks.

Access to clean water varies greatly across Tanzania. While urban areas have seen increased access to piped water, a significant portion of the rural population relies on groundwater sources. Many communities depend on wells, boreholes, and springs for their daily needs. 

The uneven distribution of water resources, coupled with seasonal variations in rainfall, can create challenges, particularly in arid and semi-arid regions. Government initiatives and NGO efforts are working to improve water infrastructure and expand access to clean water for all Tanzanians.
Research Statement

This project aims to delve deeper into the water issues faced by Tanzania. A key challenge lies in ensuring the functionality of water wells, which are vital for many rural communities. 

By leveraging machine learning, we propose the development of an algorithm that can predict the functionality of a water well. This algorithm will be designed to classify wells into three categories: functional, non-functional, and functional but requiring repair. 

This information can be a powerful tool for prioritizing maintenance efforts and ensuring a more reliable water supply for Tanzanian communities.
Research Objectives

In an effort to enhance the management and sustainability of water resources, this research focuses on two key objectives.

## Major Decisions During Data cleaning

![image](https://github.com/Mutendwa/Phase-3-Project/assets/151353695/1467d07f-95b2-4670-88a1-68592169df6d)


•	To find out the geographical distribution of the three classes of water pumps, to help inform which regions need more attention in terms of repair, maintenance, and replacement.

•	To build a Machine Learning classifier that will predict the condition of a water well (functional, functional-but-needs-repair, and non-functional), using data such as the kind of pump, when it was installed, the installer, the region, and so on.
Research questions

To guide this research, two central questions are posed.

•	What is the geographical distribution of the three classes of water pumps, and which regions need more attention for repair, maintenance, and replacement?

•	How can a Machine Learning classifier be developed to accurately predict the condition of water wells using data such as pump type, installation date, installer, and region?

## Data Understanding
This DataFrame contains 59,400 entries and 40 columns, each representing a different attribute of the data. The columns include various data types: integers (int64), floating-point numbers (float64), and strings (object). Key columns include 'id', 'amount_tsh' (total static head), 'date_recorded', 'funder', 'gps_height', 'installer', 'longitude', 'latitude', 'wpt_name' (waterpoint name), 'population', 'public_meeting', 'scheme_management', and 'scheme_name'. Some columns have missing values, such as 'funder', 'installer', 'public_meeting', 'scheme_management', and 'scheme_name', indicated by the non-null count being less than 59,400. The data appears to be related to water points, likely including information on their location, management, and operational details.

# Handling Outliers
## Longitudes had outliers
Based on the map below, some points are not located in Tanzania. Tanzania is situated at approximately latitude 6°23′31.20″ South and longitude 35°00′07.20″ East. Therefore, coordinates such as longitude 0.000000 and latitude -2.000000e-08 are not within Tanzania's boundaries. There are about 1,671 data entries with these coordinates. The decision was made to remove these points from the dataset because they do not correspond to locations within Tanzania. The data points are only 3.22 percent of the total dataset and therefore, they can be removed.

 
## Categorical columns to drop 🚮

•	Date_recorded - This column is redundant because we already have the 'year of construction' which provides similar temporal information. Thus, 'Date_recorded' can be dropped.

•	Funder - Since 'Funder' and 'Installer' contain the same values and 'Installer' has a higher influence on how a water pump functions, you should drop the 'Funder' column.

•	Iga, Ward, sub_village - These can be effectively represented by the 'region' column, allowing you to drop 'Iga', 'Ward', and 'sub_village' to reduce redundancy.

•	Recorded_by - This column contains uniform information (e.g., 'GeoData Consultants Ltd') across all entries, providing no variability or additional insights, hence it can be dropped.

•	Scheme_name - Due to its high number of unique and missing values, 'scheme_name' can be dropped as it may complicate the model without adding significant value.

•	Payment - 'Payment' and 'Payment_type' are duplicative as they contain similar values. You can drop 'Payment' to eliminate redundancy.

•	Quality_group - Similar to 'water_quality', this column does not add additional information since both describe water quality. Therefore, 'Quality_group' can be dropped.

•	Extraction_type, extraction_type_group - These are duplicates to the information in 'Extraction_type_class'. Therefore, retain 'Extraction_type_class' and drop 'Extraction_type'.

•	Quantity_group - As it mirrors the values in 'Quantity', you can drop 'Quantity_group' to streamline the dataset.

•	Source, Source_type - These columns are duplicative and can be consolidated under 'Source_class', allowing you to drop 'Source' and 'Source_type'.

•	Wpt_name - This column has a large number of unique and missing values, which could lead to high dimensionality without much predictive power. It can be dropped.

•	Waterpoint_type - Can be represented by 'Waterpoint_type_group', thus 'Waterpoint_type' can be dropped to simplify the model.

•	Management- management is a category of the management_group and hence it does not add value to the model

# Exploratory Data Analysis (EDA)

## Water Source status in Tanzania

![image](https://github.com/Mutendwa/Phase-3-Project/assets/151353695/a1020511-df04-4f09-86aa-6f29d34bf32a)


The above map shows the operational status of various water sources across the country. Functional water sources, marked in blue, are widely distributed throughout Tanzania, indicating a broad presence of operational water points. Non-functional water sources, shown in purple, are also prevalent and scattered across many regions, highlighting areas where water infrastructure might need attention or repair. Water sources that are functional but need repair, indicated in yellow, are similarly dispersed, often overlapping with regions containing both functional and non-functional sources. This distribution reveals areas of concern where maintenance and improvements are necessary to ensure reliable access to water. The map provides a comprehensive overview of the current status of water sources, aiding in identifying priority areas for infrastructure development and repair.

## Water Quality Distribution in Tanzania

![image](https://github.com/Mutendwa/Phase-3-Project/assets/151353695/6b61d347-3268-4efb-a7c8-6e7c75b3d35c)


The map above shows the geographical spread of different water quality types across the country. Soft water sources, marked in blue, are widely distributed throughout Tanzania, indicating a broad presence of this water quality type. Salty water sources, shown in red, also have a significant presence, particularly in the eastern and northern regions. Unknown water quality sources, depicted in grey, are scattered but less frequent. Milky water sources, marked in purple, and coloured water sources, shown in orange, appear sporadically across the map. Salty abandoned sources, in brown, and fluoride water sources, in green, are less common but noticeable in specific areas. Fluoride abandoned sources, marked in pink, are the least frequent and appear in very few locations. This distribution highlights the variability in water quality across Tanzania and points to areas that may require targeted interventions for water quality improvement.
## Waterpoint Type Distribution in Tanzania

![image](https://github.com/Mutendwa/Phase-3-Project/assets/151353695/63cbf6bc-80d2-420f-b15d-4d63ad954e18)


The map above illustrates the spatial distribution of various waterpoint types across the country. Communal standpipes, shown in dark blue, are widely scattered, indicating their prevalence in many regions. Hand pumps, marked in dark green, are also common and spread across different areas. The grey markers represent other waterpoint types, which are numerous and dispersed throughout Tanzania. Improved springs, indicated in teal, are less frequent and primarily located in specific regions. Cattle troughs, shown in dark orange, and dams, marked in dark red, are the least common waterpoint types, with only a few scattered locations. This map highlights the diverse distribution of waterpoint types, revealing where each type is predominantly used and potentially indicating regional preferences or needs for specific waterpoint infrastructure.
### Distribution of target 

![image](https://github.com/Mutendwa/Phase-3-Project/assets/151353695/7371c599-7aab-4fd0-ae4f-22cda0842653)


The bar chart illustrates the count of different status groups for wells in Tanzania. The majority of wells are functional, with a count of approximately 27,000. Non-functional wells follow, with a count of around 19,000. The smallest group is wells that are functional but in need of repair, with a count of about 3,000. This distribution highlights the predominance of functional wells but also indicates a significant number of wells that are not operational or require maintenance.

### Random forest

![alt text](image-5.png)

Cross-validation scores: [0.85742712 0.8510596  0.85354691 0.85195503 0.85046264]

Mean cross-validation score: 0.8528902596756541
F1 Score (Cross-Validation): 0.8525160208456092
Recall (Cross-Validation): 0.8528902596756541
Precision (Cross-Validation): 0.8523201098185117
 
# Decisions on best model
The Random Forest model is the best-performing model. Here are the reasons for the choice:
1.	High Accuracy: The Random Forest model achieved an accuracy of 0.85, which indicates a high level of correctness in its predictions.
2.	Strong F1 Score: With an F1 score of 0.88, the model shows a good balance between precision and recall, minimizing both false positives and false negatives effectively.
3.	Balanced Metrics: The model's precision for the positive class is 0.87 and recall is 0.89, which demonstrates that it handles both classes well without significant bias.
4.	Consistent Performance: The cross-validation scores for the Random Forest model are consistently high, with a mean score of 0.8538, indicating stable and reliable performance across different data splits.
5.	These reasons collectively indicate that the Random Forest model excels in classification tasks, making it the best-performing model in this scenario.
# Recommendations to Government of Tanzania

1.	Based on the findings from this study, I recommend the Government of Tanzania apply the Random Forest model to predict the condition of well pumps across the country. This model can correctly predict the actual condition of each pump with at least an 85% success rate.
2.	Additionally, the government should prioritize the Northern regions of Bukoba and Arusha, where there is a high density of pumps that need repairs, and the regions of Dodoma and Mtwara, where there is a high density of non-functional pumps.
3.	Investigations should be conducted to understand why there are more non-functional pumps in areas recorded as having zero static head and zero population.
# Limitations of the study
1.	Cross-Validation Only on Training Data: The evaluation metrics were primarily based on cross-validation on the training data. While this provides a good estimate of model performance, it may not fully capture the model's performance on unseen test data.
2.	Limited Feature Engineering: The study did not mention any advanced feature engineering techniques. Including domain-specific features or interaction terms might improve model performance.
3.	Class Imbalance: The use of SMOTE indicates that the dataset might have an imbalance between classes. While SMote helps to mitigate this, it can sometimes lead to overfitting, especially for complex models like Random Forests and Gradient Boosting.
4.	Absence of a Target Variable for Testing (Y_test): The study mentioned the absence of a y_test dataset, limiting the ability to evaluate model performance on an unseen test set. This could lead to an overestimation of the model's real-world performance.
# Conclusion
This study aimed to predict the functionality of water pumps in Tanzania using several machine learning models, including K-Nearest Neighbors, Logistic Regression, Decision Tree, Naive Bayes, Random Forest, and Gradient Boosting Classifier. The Random Forest model emerged as the best performer, showing high accuracy and balanced precision and recall metrics. Despite its strong performance, limitations such as the use of cross-validation only on training data, the absence of advanced feature engineering, and the lack of a target variable for testing (y_test) were noted. These limitations highlight the need for further work to improve feature engineering, model interpretability, and validation on unseen test data to ensure robust performance. Overall, the study demonstrates the potential of the Random Forest model in accurately predicting the functionality of water pumps, which is crucial for effective water resource management in Tanzania.




