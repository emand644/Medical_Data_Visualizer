# Medical_Data_Visualizer
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Load the dataset
df = pd.read_csv('medical_examination.csv')
# Calculate BMI and add overweight column
df['BMI'] = df['weight'] / ((df['height'] / 100) ** 2)
df['overweight'] = df['BMI'].apply(lambda x: 1 if x > 25 else 0)
# Normalize cholesterol and glucose columns
df['cholesterol'] = df['cholesterol'].apply(lambda x: 0 if x == 1 else 1)
df['gluc'] = df['gluc'].apply(lambda x: 0 if x == 1 else 1)
# Create categorical feature value counts chart
cat_features = ['cholesterol', 'gluc', 'alco', 'active', 'smoke']
for feature in cat_features:
    sns.catplot(x=feature, hue='cardio', col='cardio', kind='count', data=df)
    plt.show()
# Filter out incorrect data segments
df = df[(df['ap_lo'] <= df['ap_hi']) &
        (df['height'] >= df['height'].quantile(0.025)) &
        (df['height'] <= df['height'].quantile(0.975)) &
        (df['weight'] >= df['weight'].quantile(0.025)) &
        (df['weight'] <= df['weight'].quantile(0.975))]
# Create correlation matrix and plot heatmap
corr_matrix = df.corr()
mask = np.triu(corr_matrix)
plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, fmt=".1f", linewidths=0.5, mask=mask)
plt.show()
