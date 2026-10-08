TravelTide - Customer Segmentation

This project segments TravelTide customers into behavioral groups, allowing the marketing team to target each group with a personalized reward.

Table of contents
Project overview
Repository structure
Setup
Getting the raw data
Running the notebooks
Output
Notes and troubleshooting
Project overview

TravelTide is an online travel booking platform. The goal of this project is to better understand customer behavior and divide customers into meaningful behavioral segments. Each segment can then receive a suitable reward, such as a discount for deal seekers or an additional perk for loyal and frequent travelers.

The analysis focuses on power users: customers who started at least one session on or after 2023-01-05 and have more than 7 sessions in total. Customers who do not meet these criteria are excluded from the analysis.

The analysis follows an end-to-end pipeline:

Extract the required raw tables (users, sessions, flights, and hotels) from the TravelTide PostgreSQL database.
Clean and explore the raw data through exploratory data analysis (EDA).
Create a single row per user containing behavioral features such as number of sessions, trips, flights and hotels, booking discount rate, average seats, average nights, weekend trip ratio, total flight cost, total hotel cost, and other relevant metrics.
Analyze the resulting features and assign rule-based customer segments.
Apply K-Means clustering to customers who were not assigned to a segment by the rule-based approach.
Visualize the resulting customer segments and behavioral patterns.
Generate the final segment assignment for each customer.

The final segmentation consists of the following groups:

Rule-based segments:

Family Travelers
Discount Buyers
Young Travelers
Quick Decision Makers
VIPs
Long Distance Travelers
Long Stayers

K-Means segments:

Frequent Travelers
Budget & Occasional Travelers
Repository structure
TravelTideSep/
├── .gitignore
├── README.md
├── 01_EDA.ipynb
├── 02_user_features.ipynb
├── 03_user_features_analysis.ipynb
├── 04_clustering.ipynb
└── 05_Visualisierungen.ipynb


The notebooks contain the complete analysis pipeline, from data exploration and feature engineering to customer segmentation, clustering, and visualization.

Setup
Prerequisites
Python 3.13 or newer
Git
A PostgreSQL client to access and export the data, such as psql, DBeaver, or the Neon web SQL editor
Installation

Clone the repository and navigate into the project directory:

git clone <repo-url>
cd TravelTideSep


Create and activate a virtual environment:

python3 -m venv .venv
source .venv/bin/activate


Install the required dependencies:

pip install pandas numpy matplotlib seaborn scikit-learn airports airportsdata jupyter ipykernel


Optionally, register a dedicated Jupyter kernel for the project:

python -m ipykernel install --user --name traveltide


The notebooks use the following main libraries:

pandas
numpy
matplotlib
seaborn
scikit-learn
airports
airportsdata
jupyter
ipykernel

The versions used during development include:

pandas 3.0.5
numpy 2.5.3
matplotlib 3.11.2
seaborn 0.13.2
scikit-learn 1.9.1
airports 0.1.2
Getting the raw data

The raw TravelTide data is stored in a PostgreSQL database hosted on Neon.

Use the provided database connection string to access the database:

postgres://Test:bQNxVzJL4g6u@ep-noisy-flower-846766.us-east-2.aws.neon.tech/TravelTide


If your PostgreSQL client requires SSL, append:

?sslmode=require


The database contains the tables required for the analysis, including:

users
sessions
flights
hotels

The analysis focuses on power users and their related records. Before running the notebooks, export the required data from PostgreSQL into CSV files.

The CSV files should be available in the project data directory expected by the notebooks.

Note: The raw CSV files are not stored in the Git repository. They need to be generated from the database before running the analysis.
Running the notebooks

Start Jupyter from the project root so that all relative paths used by the notebooks work correctly:

jupyter notebook


Run the notebooks in the following order:

1. 01_EDA.ipynb

Performs the initial exploratory data analysis and data cleaning.

The notebook reads the raw TravelTide data, checks the data quality, removes or handles invalid records, and prepares the datasets for further analysis.

2. 02_user_features.ipynb

Creates the user-level feature table.

The notebook aggregates the cleaned data to one row per customer and calculates behavioral features such as sessions, trips, bookings, costs, nights, seats, discounts, and travel patterns.

3. 03_user_features_analysis.ipynb

Analyzes the generated user features and applies the rule-based segmentation logic.

Customers who meet the defined criteria are assigned to one of the rule-based groups. Customers who do not match any rule remain available for the clustering step.

4. 04_clustering.ipynb

Applies K-Means clustering to customers who were not assigned through the rule-based segmentation.

The resulting clusters are interpreted and assigned meaningful segment names. The clustering results are then combined with the rule-based segments.

5. 05_Visualisierungen.ipynb

Creates visualizations of the customer segments and their behavioral characteristics.

The notebook is used to explore and communicate differences between the identified customer groups and to support the interpretation of the final segmentation.

Each notebook builds on the results of the previous step. Therefore, the notebooks should be executed in the order listed above.

Output

The final analysis produces customer segment assignments based on both rule-based classification and K-Means clustering.

The final result contains the customer identifier and the assigned customer segment.

Example:

user_id,group
106907,Long Distance Travelers
118043,VIPs


The segment names represent the behavioral groups identified during the analysis.

Notes and troubleshooting
Always start Jupyter from the repository root so that relative file paths are resolved correctly.
The required CSV files are intentionally not stored in the repository and must be generated from the PostgreSQL database before running the notebooks.
The notebooks should be executed in the specified order because each step depends on the output of the previous analysis step.
If the PostgreSQL connection fails, check that the Neon database is available and that the connection string is correct.
The Neon database may be paused after a period of inactivity. Opening the Neon dashboard can wake the database before reconnecting.
Some flight and hotel records may not have a corresponding trip in the sessions data. This can occur because of cancellations, negative nights, or duplicate records. These cases are handled during the data-cleaning process.
If a notebook cannot find an input file, check that the expected CSV files have been exported and that they are located in the directory referenced by the notebook.