pipeline {
    agent any

    stages {
        stage('Fetch and Transform Data') {
            steps {
                script {
                    // Example: Check Python version
                    sh 'python3 --version'
                    
                    // Run the Python script
                    sh '''
                    python3 <<EOF
import requests
import pandas as pd

# Define API details
channels = {
    "Channel1": {
        "url": "https://api.thingspeak.com/channels/2561136/feeds.json?api_key=0LDH7ODWRLXSWTUQ",
        "fields": ["created_at", "entry_id", "field2", "field3", "field4", "field5", "field6", "field7", "field8"],
        "rename": {
            "created_at": "timestamp",
            "field2": "Body_temp",
            "field3": "flamevalue",
            "field4": "light sensitivity",
            "field5": "pressure",
            "field6": "Gas_resistance",
            "field7": "temperature",
            "field8": "humidity"
        }
    },
    "Channel2": {
        "url": "https://api.thingspeak.com/channels/2561145/feeds.json?api_key=NL5IN3EHKM65CY8M",
        "fields": ["created_at", "entry_id", "field2", "field3"],
        "rename": {
            "created_at": "timestamp",
            "field2": "heartRate",
            "field3": "SP02"
        }
    }
}

# Function to fetch data from a channel and transform it
def fetch_and_transform_data(channel_url, fields, rename_columns):
    try:
        response = requests.get(channel_url)
        response.raise_for_status()  # Ensure request was successful
        data = response.json()

        # Check if 'feeds' key is present in the response
        if 'feeds' not in data:
            print("Error: 'feeds' key not found in the API response.")
            return pd.DataFrame()  # Return an empty DataFrame

        df = pd.DataFrame(data['feeds'])

        # Select only the fields we're interested in
        df = df[fields]
        # Rename columns for clarity
        df.rename(columns=rename_columns, inplace=True)

        # Separate date and time from timestamp
        df['date'] = pd.to_datetime(df['timestamp']).dt.date
        df['time'] = pd.to_datetime(df['timestamp']).dt.time
        # Drop the original timestamp column (already separated into date and time)
        df.drop(columns=['timestamp'], inplace=True)

        # Format other fields to two decimal places
        for column in df.columns:
            if column not in ['entry_id', 'date', 'time']:
                df[column] = pd.to_numeric(df[column], errors='coerce').round(2)
        print("Channel Data Preview:")
        print(df.head())

    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from {channel_url}: {e}")
        return pd.DataFrame()
    except Exception as e:
        print(f"An error occurred: {e}")
        return pd.DataFrame()

# Fetch and transform data from Channel 1
fetch_and_transform_data(
    channels["Channel1"]["url"], 
    channels["Channel1"]["fields"],
    channels["Channel1"]["rename"]
)

# Fetch and transform data from Channel 2
fetch_and_transform_data(
    channels["Channel2"]["url"], 
    channels["Channel2"]["fields"],
    channels["Channel2"]["rename"]
)
EOF
                    '''
                }
            }
        }
    }
}
