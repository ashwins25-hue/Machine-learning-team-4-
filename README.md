SafeHabitat AI
Chennai Multi-Hazard Disaster Risk Prediction, Red-Zone Identification and Relocation Priority Assessment
SafeHabitat AI is a machine-learning micro-project designed as a prototype decision-support workflow for assessing disaster-risk scenarios in Chennai. It uses environmental features to classify habitation records into Low, Moderate, or High risk categories, flags potential red-zone records, calculates illustrative relocation-priority scores, and checks candidate relocation sites against basic suitability criteria.
Important: The two input datasets are synthetic demonstration datasets. Results are not official Chennai disaster-risk classifications or relocation recommendations. The prototype must not be used for real-world evacuation, relocation, or public-safety decisions without validated data, domain-expert review, and approval from relevant authorities.

Objectives
- Explore and clean multi-hazard scenario data.
- Preprocess numerical and categorical features.
- Train and evaluate a Random Forest classifier.
- Predict risk levels and identify potential red-zone records.
- Assess habitation vulnerability and calculate a relocation-priority score.
- Screen candidate relocation sites for capacity, water, roads, healthcare access, hazard screening, and land-use approval.
- Visualize results and export prediction, evaluation, and site-assessment files.
Technology Stack
- Language: Python
- Environment: Google Colab / Jupyter Notebook
- Data handling: pandas, NumPy
- Machine learning: scikit-learn
- Visualization: Matplotlib, Seaborn
- Model persistence: joblib
- Notebook upload/download: google.colab.files
Project Workflow
1. Upload the two required CSV datasets.
2. Inspect dataset structure, missing values, duplicate record IDs, and class distributions.
3. Remove duplicate record_id values and rows with unsupported risk_level labels.
4. Select environmental features and the risk_level target.
5. Split the data into training (80%) and test (20%) sets using stratification.
6. Build a preprocessing pipeline:
   - Impute missing numerical values with the median.
   - Impute missing categorical values with the most frequent value.
   - One-hot encode categorical features.
7. Train a Random Forest classifier with 200 trees and balanced class weights.
8. Evaluate predictions using accuracy, weighted precision, weighted recall, weighted F1 score, high-risk recall, a classification report, and a confusion matrix.
9. Calculate feature importance and predict risk for all habitation records.
10. Flag records predicted as High as potential red zones.
11. Calculate vulnerability indicators and an illustrative relocation-priority score.
12. Evaluate candidate relocation sites and generate visualizations.
13. Save the trained model and export results as CSV files.
Input Datasets
Upload both files with these exact filenames when running the notebook:
1. safehabitat_chennai_multihazard_synthetic.csv
The notebook expects columns including:
- record_id, locality, dominant_hazard, risk_level
- Environmental model features:
  - rainfall_mm_7d
  - max_wind_kmph
  - max_temperature_c
  - elevation_m
  - slope_degrees
  - soil_moisture_pct
  - distance_to_river_km
  - distance_to_coast_km
  - water_scarcity_index
  - previous_disaster_events
  - soil_type
- Vulnerability and prioritization fields:
  - population
  - housing_condition
  - road_access
  - healthcare_access_km
The target column risk_level must contain Low, Moderate, or High labels.
2. safehabitat_chennai_candidate_sites_synthetic.csv
The notebook expects fields including:
- site_name
- available_capacity_persons
- water_available
- road_access
- healthcare_distance_km
- independent_hazard_screening_passed
- land_use_approval_confirmed
The code interprets the binary fields water_available, independent_hazard_screening_passed, and land_use_approval_confirmed as 1 when the relevant check passes.

Dataset note: These filenames and columns are required by the notebook. The notebook does not create the input datasets; prepare or obtain the two CSV files before running it.
Installation and Setup
Option A: Google Colab
1. Open Google Colab.
2. Upload ML_MP_T_4.ipynb or open the notebook in Colab.
3. Run the cells in order.
4. When prompted, upload both required CSV datasets.
5. Allow the notebook to train the model, display evaluation results and charts, and save outputs.
6. The notebook will prompt downloads for the output CSV files and the trained model.
The notebook imports google.colab.files, so Google Colab is the intended environment. In a local Jupyter environment, that import and the Colab upload/download steps will need to be replaced.
Option B: Install Python Packages
For a compatible Python environment, install the main libraries:
pip install pandas numpy matplotlib seaborn scikit-learn joblib
Then open the notebook in Jupyter. Note that the notebook currently uses Google Colab's file-upload and file-download API, so those lines must be adapted for local execution.
Running the Project
1. Ensure both input CSV files use the exact filenames shown above.
2. Run the notebook from top to bottom.
3. Review the printed dataset checks and model evaluation metrics.
4. Review the confusion matrix, feature-importance chart, risk distribution, hazard distribution, and relocation-priority chart.
5. Review the candidate-site results carefully; a site passes all prototype checks only if all six checks pass.
6. Retrieve the exported files listed below.
Risk and Priority Logic
Risk classification
The Random Forest model predicts one of three labels: Low, Moderate, or High. Records predicted as High are marked as potential red zones.
Relocation-priority score
The notebook calculates an illustrative score from 0 to 100 using these weights:
Component	Weight
Model high-risk probability	40%
Population vulnerability	20%
Housing vulnerability	15%
Accessibility vulnerability	10%
Historical disaster exposure	15%


Priority categories are assigned as follows:
- High Priority: score ≥ 70
- Medium Priority: score ≥ 40 and < 70
- Low Priority: score < 40
These weights and thresholds are prototype assumptions, not validated government standards.
Candidate relocation-site checks
A candidate site is marked as passing all prototype checks only when all of the following are true:
- Available capacity is at least the selected habitation's population.
- Water availability is marked as 1.
- Road access is Good.
- Healthcare distance is 5 km or less.
- Independent hazard screening is marked as passed (1).
- Land-use approval is confirmed (1).
The notebook checks candidate sites for the single highest-ranked predicted high-risk habitation. A result such as “All prototype checks passed” is not official approval or proof that a site is safe.
Outputs
The notebook saves and prompts downloads for these files:
File	Description
safehabitat_chennai_predictions.csv	Habitation records with predicted risk, high-risk score, confidence, vulnerability indicators, and priority scores
safehabitat_red_zones.csv	Records predicted as High and flagged as potential red zones
safehabitat_relocation_priority.csv	Predicted high-risk records sorted by relocation-priority score
safehabitat_candidate_site_results.csv	Candidate-site checks and suitability status
safehabitat_model_performance.csv	Accuracy, weighted precision, weighted recall, weighted F1 score, and high-risk recall
safehabitat_random_forest_model.pkl	Serialized trained preprocessing-and-classification pipeline


Model Evaluation
The notebook reports:
- Accuracy
- Weighted precision
- Weighted recall
- Weighted F1 score
- High-risk recall
- Classification report
- Confusion matrix
Actual scores depend on the uploaded dataset and the train/test split. No fixed accuracy or result is guaranteed. Because the data is synthetic, good performance on it does not establish real-world predictive reliability.
Limitations and Responsible Use
- The datasets are synthetic and may not reflect real environmental conditions or population distributions in Chennai.
- Risk labels may encode assumptions present in the synthetic data; model metrics should be interpreted in that context.
- The priority weights, thresholds, and site criteria are illustrative and require expert validation.
- The site-screening logic evaluates only the highest-ranked predicted high-risk habitation in this notebook.
- A model prediction is not a substitute for official hazard maps, field surveys, engineering assessment, community consultation, or government authorization.
- Before any real-world use, validate data quality, assess model bias and uncertainty, evaluate performance on independent real data, and involve disaster-management, urban-planning, public-health, and local-government experts.
Suggested Future Enhancements
- Integrate verified official rainfall, flood, cyclone, elevation, soil, and historical-disaster datasets.
- Add geospatial maps and locality-level visualization.
- Validate model performance on independent, real-world records.
- Include uncertainty reporting and explainability for individual predictions.
- Assess candidate sites for every high-priority habitation rather than only the top-ranked one.
- Add a dashboard for authorized users, with data provenance and audit logs.
- Review priority weights and site constraints with domain experts and affected communities.
Repository Structure
A simple project folder can be organized as:
SafeHabitat-AI/
├── ML_MP_T_4.ipynb
├── safehabitat_chennai_multihazard_synthetic.csv
├── safehabitat_chennai_candidate_sites_synthetic.csv
└── README.md
Generated CSV files and the .pkl model will appear after successful execution.
License
No license is specified in the notebook. Add a license file if you plan to publish or distribute the project.
