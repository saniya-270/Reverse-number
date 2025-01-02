import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
file_path = "your_dataset_path.csv"  # Replace with your dataset's path
data = pd.read_csv(file_path)

# Display the first few rows of the dataset
print("Dataset Overview:")
print(data.head())

# Check for missing values
print("\nMissing Values in Each Column:")
print(data.isnull().sum())

# Descriptive statistics
print("\nDescriptive Statistics:")
print(data.describe())

# Exploratory Data Analysis (EDA)
print("\nColumn Names:")
print(data.columns)

# Assuming the dataset has columns like 'Date', 'Unemployment Rate', 'Region'
# Convert 'Date' to datetime format if not already
if 'Date' in data.columns:
    data['Date'] = pd.to_datetime(data['Date'])

# Visualize unemployment rate over time
plt.figure(figsize=(12, 6))
if 'Date' in data.columns and 'Unemployment Rate' in data.columns:
    sns.lineplot(x=data['Date'], y=data['Unemployment Rate'], label="Unemployment Rate")
    plt.title("Unemployment Rate Over Time")
    plt.xlabel("Date")
    plt.ylabel("Unemployment Rate (%)")
    plt.legend()
    plt.grid(True)
    plt.show()
else:
    print("The dataset does not contain 'Date' or 'Unemployment Rate' columns.")

# Regional analysis if applicable
if 'Region' in data.columns:
    plt.figure(figsize=(12, 6))
    sns.boxplot(x='Region', y='Unemployment Rate', data=data)
    plt.title("Unemployment Rate by Region")
    plt.xticks(rotation=45)
    plt.xlabel("Region")
    plt.ylabel("Unemployment Rate (%)")
    plt.show()

# Correlation heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(data.corr(), annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Matrix")
plt.show()

# Optional: Group by month and analyze trends
if 'Date' in data.columns:
    data['Month'] = data['Date'].dt.to_period('M')
    monthly_trends = data.groupby('Month')['Unemployment Rate'].mean().reset_index()
    plt.figure(figsize=(12, 6))
    sns.lineplot(x='Month', y='Unemployment Rate', data=monthly_trends)
    plt.title("Monthly Unemployment Rate Trends")
    plt.xlabel("Month")
    plt.ylabel("Unemployment Rate (%)")
    plt.grid(True)
    plt.show()
