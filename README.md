# clean-financial-money-market-data
The function here drops duplicates, converts the date column to a datetime object, sorts the dataframe by date, drops rows with missing values, converts numerical columns to floats, and removes outliers using the z-score method. Finally, it resets the index and returns the cleaned dataframe.
import pandas as pd
import numpy as np

def clean_financial_data(df):
    # Drop duplicates
    df.drop_duplicates(inplace=True)

    # Convert date column to datetime
    df['date'] = pd.to_datetime(df['date'])

    # Sort dataframe by date
    df.sort_values(by='date', inplace=True)

    # Drop rows with missing values
    df.dropna(inplace=True)

    # Convert numerical columns to floats
    cols_to_convert = ['open', 'high', 'low', 'close', 'volume']
    df[cols_to_convert] = df[cols_to_convert].astype(float)

    # Remove outliers using z-score method
    for col in cols_to_convert:
        z_score = np.abs((df[col] - df[col].mean()) / df[col].std())
        df = df[z_score <= 3]

    # Reset index
    df.reset_index(drop=True, inplace=True)

    return df

