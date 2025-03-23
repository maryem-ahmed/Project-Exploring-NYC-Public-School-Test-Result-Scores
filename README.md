# Project-Exploring-NYC-Public-School-Test-Result-Scores

import pandas as pd

# Read in the data
schools = pd.read_csv("schools.csv")

# Calculate 80% of the maximum possible math score (800)
threshold = 800 * 0.80

# Filter schools with average math scores at least 80% of 800
best_math_schools = schools[schools['average_math'] >= threshold][['school_name', 'average_math']]

# Sort by average_math in descending order
best_math_schools = best_math_schools.sort_values(by='average_math', ascending=False)
# Calculate the combined SAT score
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']

# Identify the top 10 schools based on the combined SAT scores
top_10_schools = schools[['school_name', 'total_SAT']].sort_values(by='total_SAT', ascending=False).head(10)

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
