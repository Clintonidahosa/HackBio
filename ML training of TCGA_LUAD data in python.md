### Task - Tumor Type Prediction
This project uses the following datasets:

1. **LUAD Normalized Data**:  [https://github.com/Clintonidahosa/HackBio-projects/blob/main/luad_normalizedData.csv]
2. **TCGA LUAD Metadata**:   [https://github.com/Clintonidahosa/HackBio-projects/blob/main/TCGA_LUAD_metadata.csv]

```
#importing necessary libraries
import pandas as pd
from sklearn.ensemble import RandomForestClassifier

# Load datasets
url_normdata = "https://raw.githubusercontent.com/Clintonidahosa/HackBio-projects/refs/heads/main/luad_normalizedData.csv"
normdata = pd.read_csv(url_normdata)

url_metadata = "https://raw.githubusercontent.com/Clintonidahosa/HackBio-projects/refs/heads/main/TCGA_LUAD_metadata.csv"
metadata = pd.read_csv(url_metadata)

print("Data Loaded Successfully")
print("normdata Shape:", normdata.shape)
print("metadata Shape:", metadata.shape)

# Prepare training data
X_train = metadata.drop(columns=['tumor_type'])  # Features
y_train = metadata['tumor_type']  # Target

# Transpose normalized dataset
Trans_data = normdata.T
Trans_data.reset_index(inplace=True)

# Rename the first column to 'barcode' and drop the first row
Trans_data.columns = ['barcode'] + list(Trans_data.iloc[0, 1:])  # Ensure the first column is 'barcode'
Trans_data = Trans_data[1:]  # Remove the first row with the original column names
Trans_data.reset_index(drop=True, inplace=True)

# Prepare test data
X_test = Trans_data  # Test data after transposing

# Ensure 'barcode' is available in X_test for final predictions
if 'barcode' not in X_test.columns:
    raise ValueError("The 'barcode' column is missing from the test data.")

# Check for duplicate columns in the training set
if X_train.columns.duplicated().any():
    print("Duplicate columns found in training set.")
    X_train = X_train.loc[:, ~X_train.columns.duplicated()]

# Check for duplicate columns in the test set
if X_test.columns.duplicated().any():
    print("Duplicate columns found in test set.")
    X_test = X_test.loc[:, ~X_test.columns.duplicated()]

# One-hot encoding for categorical variables in training set
X_train_encoded = pd.get_dummies(X_train, drop_first=True)

# Train Random Forest model
model = RandomForestClassifier(random_state=42)
model.fit(X_train_encoded, y_train)

# One-hot encoding for test set
X_test_encoded = pd.get_dummies(X_test.drop(columns=['barcode']), drop_first=True)

# Align columns of test set with training set
X_test_encoded = X_test_encoded.reindex(columns=X_train_encoded.columns, fill_value=0)

# Make predictions
predictions = model.predict(X_test_encoded)

# Create a DataFrame with all barcodes and their predictions
predictions_df = pd.DataFrame({
    'barcode': X_test['barcode'].values,
    'predicted_tumor_type': predictions
})

# Ensure that all barcodes from X_test are present in the predictions
all_barcodes = X_test['barcode'].unique()
predictions_df = predictions_df[predictions_df['barcode'].isin(all_barcodes)]

# Save predictions to a CSV file
predictions_df.to_csv('predictions.csv', index=False)

print("Predictions saved to 'predictions.csv'")

```
