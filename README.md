# demographic_data_analyzer.py

import pandas as pd

def analyze_demographics(df):
    # 1. How many people of each race are represented in this dataset?
    race_counts = df['race'].value_counts()

    # 2. What is the average age of men?
    average_age_men = df[df['sex'] == 'Male']['age'].mean()

    # 3. What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = (df[df['education'] == 'Bachelors'].shape[0] / df.shape[0]) * 100

    # 4. What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?
    advanced_education = df[df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    percentage_advanced_education_more_than_50k = (advanced_education[advanced_education['salary'] == '>50K'].shape[0] / advanced_education.shape[0]) * 100

    # 5. What percentage of people without advanced education make more than 50K?
    non_advanced_education = df[~df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    percentage_non_advanced_education_more_than_50k = (non_advanced_education[non_advanced_education['salary'] == '>50K'].shape[0] / non_advanced_education.shape[0]) * 100

    # 6. What is the minimum number of hours a person works per week?
    min_hours_per_week = df['hours-per-week'].min()

    # 7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    min_hours_people = df[df['hours-per-week'] == min_hours_per_week]
    percentage_min_hours_more_than_50k = (min_hours_people[min_hours_people['salary'] == '>50K'].shape[0] / min_hours_people.shape[0]) * 100

    # 8. What country has the highest percentage of people that earn >50K and what is that percentage?
    country_salary = df.groupby('native-country').apply(lambda x: (x[x['salary'] == '>50K'].shape[0] / x.shape[0]) * 100)
    highest_country = country_salary.idxmax()
    highest_country_percentage = country_salary.max()

    # 9. Identify the most popular occupation for those who earn >50K in India.
    india_high_salary = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]
    most_popular_occupation_india = india_high_salary['occupation'].value_counts().idxmax()

    # Return all results
    return {
        'race_counts': race_counts,
        'average_age_men': average_age_men,
        'percentage_bachelors': round(percentage_bachelors, 1),
        'percentage_advanced_education_more_than_50k': round(percentage_advanced_education_more_than_50k, 1),
        'percentage_non_advanced_education_more_than_50k': round(percentage_non_advanced_education_more_than_50k, 1),
        'min_hours_per_week': min_hours_per_week,
        'percentage_min_hours_more_than_50k': round(percentage_min_hours_more_than_50k, 1),
        'highest_country': highest_country,
        'highest_country_percentage': round(highest_country_percentage, 1),
        'most_popular_occupation_india': most_popular_occupation_india
    }
