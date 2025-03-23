# Project-Exploring-NYC-Public-School-Test-Result-Scores
NYC Schools Analysis
This project analyzes NYC school performance data, focusing on math scores, combined SAT scores, and borough-level statistics. The dataset includes information on school names, boroughs, building codes, average math, reading, and writing scores, and the percentage of students tested.

Data Analysis Tasks
1. Best Math Schools
Identify schools with the best math results, defined as having an average math score of at least 80% of the maximum possible score (800).

Steps:
Calculate 80% of the maximum possible math score (800): threshold = 800 * 0.80.

Filter schools with average math scores greater than or equal to the threshold.

Sort the results by average_math in descending order.

Results:
Saved in a DataFrame called best_math_schools with columns:

school_name: Name of the school.

average_math: Average math score of the school.

python
Copy
import pandas as pd

# Read in the data
schools = pd.read_csv("schools.csv")

# Calculate 80% of the maximum possible math score (800)
threshold = 800 * 0.80

# Filter schools with average math scores at least 80% of 800
best_math_schools = schools[schools['average_math'] >= threshold][['school_name', 'average_math']]

# Sort by average_math in descending order
best_math_schools = best_math_schools.sort_values(by='average_math', ascending=False)
2. Top 10 Schools Based on Combined SAT Scores
Identify the top 10 schools based on the combined SAT scores, where the combined SAT score is the sum of the average math, reading, and writing scores.

Steps:
Calculate the combined SAT score for each school: total_SAT = average_math + average_reading + average_writing.

Sort schools by total_SAT in descending order.

Select the top 10 schools.

Results:
Saved in a DataFrame called top_10_schools with columns:

school_name: Name of the school.

total_SAT: Combined SAT score (sum of math, reading, and writing scores).

python
Copy
# Calculate the combined SAT score
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']

# Identify the top 10 schools based on the combined SAT scores
top_10_schools = schools[['school_name', 'total_SAT']].sort_values(by='total_SAT', ascending=False).head(10)
3. Borough with the Largest Standard Deviation in Combined SAT Scores
Determine which NYC borough has the largest standard deviation in the combined SAT scores.

Steps:
Group the data by borough.

Calculate the number of schools, mean combined SAT score, and standard deviation of combined SAT scores for each borough.

Identify the borough with the largest standard deviation.

Round all numeric values to two decimal places.

Results:
Saved in a DataFrame called largest_std_dev with columns:

borough: Name of the borough.

num_schools: Number of schools in the borough.

average_SAT: Mean combined SAT score for the borough.

std_SAT: Standard deviation of combined SAT scores for the borough.

python
Copy
# Group by borough and calculate the required statistics
borough_stats = schools.groupby('borough').agg(
    num_schools=('school_name', 'size'),
    average_SAT=('total_SAT', 'mean'),
    std_SAT=('total_SAT', 'std')
).reset_index()

# Round the numeric values to two decimal places
borough_stats = borough_stats.round(2)

# Identify the borough with the largest standard deviation
largest_std_dev = borough_stats.loc[borough_stats['std_SAT'].idxmax()]

# Convert the result to a DataFrame
largest_std_dev = pd.DataFrame(largest_std_dev).transpose()
Summary of Results
1. Best Math Schools
The best_math_schools DataFrame contains schools with average math scores of at least 640 (80% of 800), sorted by average_math in descending order.

2. Top 10 Schools Based on Combined SAT Scores
The top_10_schools DataFrame contains the top 10 schools with the highest combined SAT scores, ordered by total_SAT in descending order.

3. Borough with the Largest Standard Deviation in Combined SAT Scores
The largest_std_dev DataFrame contains the borough with the largest standard deviation in combined SAT scores, along with the number of schools, average SAT score, and standard deviation of SAT scores, all rounded to two decimal places.

Dependencies
Python 3.x

pandas

Author
Maryem Ahmed
