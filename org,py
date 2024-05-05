import streamlit as st
import pandas as pd
import seaborn as sns

# Function to upload PDF data
def upload_data():
    uploaded_file = st.file_uploader("Upload Monthly Report (CSV)", type="csv")
    if uploaded_file is not None:
        df = pd.read_csv(uploaded_file)
        return df

# Function to calculate derived fields
def calculate_fields(df, selected_fields):
    # Calculate derived fields
    if "Net Inflow/Outflow" in selected_fields:
        df['Net Inflow/Outflow'] = df['Funds Mobilized'] - df['Repurchase/Redemption']
    if "Net Asset under Management per Scheme" in selected_fields:
        df['Net AUM per Scheme'] = df['Net Assets Under Management'] / df['No. of Schemes']
    if "Net Inflow/Outflow per Scheme" in selected_fields:
        df['Net Inflow/Outflow per Scheme'] = df['Net Inflow/Outflow'] / df['No. of Schemes']
    return df

# Function to generate dynamic charts
def generate_charts(df, selected_fields):
    for field in selected_fields:
        if field != "Date":
            st.write(field)
            st.line_chart(df.pivot("Date", "Scheme", field))

# Main function to run the Streamlit app
def main():
    st.title("Mutual Fund Data Analysis App")
    
    # Data upload section
    st.header("1. Data Upload Functionality")
    df = upload_data()
    
    if df is not None:
        # Scheme selection section
        st.header("2. Scheme Selection")
        selected_schemes = st.multiselect("Select Mutual Fund Scheme(s)", df['Scheme'].unique())

        # Field selection and calculation section
        st.header("3. Field Selection and Calculation")
        selected_fields = st.multiselect("Select Data Field(s)", df.columns)
        df = calculate_fields(df, selected_fields)

        # Dynamic reporting section
        st.header("4. Dynamic Reporting")
        st.write("Selected Data:")
        st.write(df)

        # Generate dynamic charts
        if st.button("Generate Charts"):
            generate_charts(df, selected_fields)

        # Download capability section
        st.header("5. Download Capability")
        if st.button("Download Report as CSV"):
            csv = df.to_csv(index=False)
            st.download_button(label="Download CSV", data=csv, file_name='mutual_fund_report.csv', mime='text/csv')

if __name__ == "__main__":
    main()
