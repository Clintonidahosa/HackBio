### Task - Tumor Type Prediction
This project uses the following datasets:

1. **LUAD Normalized Data**:  [https://raw.githubusercontent.com/Clintonidahosa/HackBio-projects/refs/heads/main/luad_normalizedData.csv]
2. **TCGA LUAD Metadata**:   [https://github.com/Clintonidahosa/HackBio-projects/blob/main/TCGA_LUAD_metadata.csv]

### How to Access
* Download the 2 datasets using the links above
* move them both to desktop as this is the file path used in this code


```
 #importing necessary libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_selection import SelectKBest, chi2
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

# Load datasets
data = pd.read_csv(r"C:\Users\DELL\Desktop\luad_normalizedData.csv")
print(data.head())

metadata = pd.read_csv(r"C:\Users\DELL\Desktop\TCGA_LUAD_metadata.csv")
print(metadata.head())

# Transpose normalized dataset so I can merge with metadata
Trans_data = data.T
Trans_data.reset_index(inplace=True)

# Store transposed data as a CSV file
Trans_data.to_csv('C:/Users/DELL/Desktop/Trans_data.csv', index=True)

# Add new header row "barcode" to transposed data so I can have common column with metadata
new_header = pd.DataFrame({'index': ['barcode']})
modified_data = pd.concat([new_header, Trans_data], ignore_index=True)
print(modified_data)

# Set the first row as header
modified_data.columns = modified_data.iloc[0]
modified_data = modified_data[1:]  # Remove the first row from the data

# Remove the index row
modified_data.reset_index(drop=True, inplace=True)

# Perform the outer merge with metadata
merged_data = pd.merge(modified_data, metadata, on='barcode', how='outer')
print("Outer Merge Result:\n", merged_data.head())

# Save the merged DataFrame to CSV on the desktop
merged_data.to_csv('C:/Users/DELL/Desktop/merged_data.csv', index=False)
print("Merged data saved as merged_data.csv on Desktop.")

# Feature selection and classification
# Convert categorical data using one-hot encoding and select the target column
X = pd.get_dummies(merged_data.drop('barcode', axis=1), drop_first=True)
y = merged_data['barcode']  # Use 'barcode' as the target column

# Split the data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Feature selection using SelectKBest with chi-square test
selector = SelectKBest(chi2, k=10)  # Select top 10 features
X_train_selected = selector.fit_transform(X_train, y_train)
X_test_selected = selector.transform(X_test)

# Print selected features
selected_features_indices = selector.get_support(indices=True)
print("Selected Features Indices: ", selected_features_indices)
print("Selected Features: ", X.columns[selected_features_indices].tolist())

# Initialize and train Random Forest classifier
rf = RandomForestClassifier(n_estimators=100, random_state=42)
rf.fit(X_train_selected, y_train)

# Predict on test data
y_pred_rf = rf.predict(X_test_selected)

# Evaluate performance
print("Random Forest Accuracy: ", accuracy_score(y_test, y_pred_rf)) 
print("Classification Report:\n", classification_report(y_test, y_pred_rf))
```
