# financial-Analyzer
import streamlit as st
import pandas as pd

st.set_page_config(page_title="Financial Statement Analyzer", layout="centered")

st.title("📊 Financial Statement Analyzer")
st.write("Upload your income statement or balance sheet in CSV format to get automatic analysis.")

uploaded_file = st.file_uploader("Upload CSV file", type=["csv"])

if uploaded_file is not None:
    try:
        df = pd.read_csv(uploaded_file)

        st.subheader("Preview of Uploaded Data")
        st.dataframe(df.head())

        # Extract necessary values
        revenue = df['Revenue'].sum()
        net_profit = df['Net Profit'].sum()
        total_assets = df['Total Assets'].sum()
        total_liabilities = df['Total Liabilities'].sum()

        # Calculations
        profit_margin = (net_profit / revenue) * 100 if revenue else 0
        debt_ratio = (total_liabilities / total_assets) * 100 if total_assets else 0

        st.subheader("Key Financial Metrics")
        st.metric("Total Revenue", f"${revenue:,.2f}")
        st.metric("Net Profit", f"${net_profit:,.2f}")
        st.metric("Profit Margin", f"{profit_margin:.2f}%")
        st.metric("Debt Ratio", f"{debt_ratio:.2f}%")

    except Exception as e:
        st.error(f"Error reading file: {e}")
