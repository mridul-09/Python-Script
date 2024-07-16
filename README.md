# Python-Script

import pandas as pd

# Load the CSV file
file_path = r'C:\Users\Lenovo\Downloads\competitors.csv'
df = pd.read_csv(file_path)

# Print column names to verify
print("Column names:", df.columns)

# Include and exclude keywords
include_keywords = [
    'bulk email', 'mass email', 'large volume email', 'email blasts',
    'MTA management', 'Mail Transfer Agent', 'SMTP server management', 'email server management',
    'email deliverability', 'email delivery optimization', 'deliverability services', 'inbox placement',
    'email system management', 'email infrastructure', 'email monitoring', 'email system maintenance',
    'transactional email', 'email gateway', 'high volume email service', 'email relay', 'email filtering'
]

exclude_keywords = [
    'software only', 'email marketing software', 'email server product', 'email marketing tool',
    'web hosting', 'cloud storage', 'general IT services', 'social media marketing',
    'digital marketing', 'web development', 'SEO services'
]

# Function to check if a description contains any include keywords
def contains_include_keyword(description):
    return any(keyword in str(description).lower() for keyword in include_keywords)

# Function to check if a description contains any exclude keywords
def contains_exclude_keyword(description):
    return any(keyword in str(description).lower() for keyword in exclude_keywords)

try:
    # Filter the data for relevant services
    filtered_df = df[df.apply(lambda x: contains_include_keyword(x['Service Description']) and 
                                        not contains_exclude_keyword(x['Service Description']), axis=1)]
    
    # Further filter by location
    relevant_countries = ['United States', 'United Kingdom', 'Germany', 'France', 'Netherlands', 'Canada', 'Australia']
    filtered_df = filtered_df[filtered_df['Country'].isin(relevant_countries)]
    
    # Save the filtered results to a new CSV file
    output_file_path = r'C:\Users\Lenovo\Downloads\filtered_competitors.csv'
    filtered_df.to_csv(output_file_path, index=False)
    
    print(f"Filtered results saved to '{output_file_path}'")
except KeyError as e:
    print(f"KeyError: {e}. Check if the column names match your CSV file.")
except Exception as e:
    print(f"An error occurred: {e}")
