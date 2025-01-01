import pandas as pd

def analyze_demographic_data(data):
    # 1. How many people of each race are represented in this dataset?
    race_count = data['race'].value_counts()

    # 2. What is the average age of men?
    average_age_men = data[data['sex'] == 'Male']['age'].mean()

    # 3. What is the percentage of people who have a Bachelor's degree?
    percentage_bachelors = (data[data['education'] == 'Bachelors'].shape[0] / data.shape[0]) * 100

    # 4. What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?
    advanced_education = data[data['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    advanced_education_high_income = advanced_education[advanced_education['salary'] == '>50K']
    percentage_high_income_advanced = (advanced_education_high_income.shape[0] / advanced_education.shape[0]) * 100

    # 5. What percentage of people without advanced education make more than 50K?
    no_advanced_education = data[~data['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    no_advanced_education_high_income = no_advanced_education[no_advanced_education['salary'] == '>50K']
    percentage_high_income_no_advanced = (no_advanced_education_high_income.shape[0] / no_advanced_education.shape[0]) * 100

    # 6. What is the minimum number of hours a person works per week?
    min_hours_per_week = data['hours-per-week'].min()

    # 7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    min_hours_high_income = data[(data['hours-per-week'] == min_hours_per_week) & (data['salary'] == '>50K')]
    percentage_min_hours_high_income = (min_hours_high_income.shape[0] / data[data['hours-per-week'] == min_hours_per_week].shape[0]) * 100

    # 8. What country has the highest percentage of people that earn >50K and what is that percentage?
    country_income = data[data['salary'] == '>50K'].groupby('native-country').size() / data.groupby('native-country').size() * 100
    highest_percentage_country = country_income.idxmax()
    highest_percentage_value = country_income.max()

    # 9. Identify the most popular occupation for those who earn >50K in India.
    india_high_income = data[(data['native-country'] == 'India') & (data['salary'] == '>50K')]
    most_popular_occupation_india = india_high_income['occupation'].mode()[0]

    return {
        "race_count": race_count,
        "average_age_men": round(average_age_men, 1),
        "percentage_bachelors": round(percentage_bachelors, 1),
        "percentage_high_income_advanced": round(percentage_high_income_advanced, 1),
        "percentage_high_income_no_advanced": round(percentage_high_income_no_advanced, 1),
        "min_hours_per_week": min_hours_per_week,
        "percentage_min_hours_high_income": round(percentage_min_hours_high_income, 1),
        "highest_percentage_country": highest_percentage_country,
        "highest_percentage_value": round(highest_percentage_value, 1),
        "most_popular_occupation_india": most_popular_occupation_india
    }

# Example usage (if testing the function):
if __name__ == "__main__":
    try:
        # Ensure the file 'adult.csv' is in the same directory or provide the correct path
        data = pd.read_csv('adult.csv')  # Update this path as necessary
        result = analyze_demographic_data(data)
        print(result)
    except FileNotFoundError:
        print("The file 'adult.csv' was not found. Please make sure the file path is correct.")
