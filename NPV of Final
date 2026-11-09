
import streamlit as st

# --------------------------------------------------
# PAGE CONFIGURATION
# --------------------------------------------------
st.set_page_config(
    page_title="NPV Calculator",
    page_icon="💰",
    layout="centered"
)

st.title("Net Present Value Calculator")
st.write("Calculate the NPV of an investment project.")
st.divider()

# --------------------------------------------------
# INITIAL PROJECT INPUTS
# --------------------------------------------------
st.subheader("Project Information")

col1, col2 = st.columns(2)

with col1:
    initial_cost = st.number_input(
        "Initial Investment ($)",
        min_value=0.0,
        value=100000.0,
        step=1000.0
    )

with col2:
    discount_rate = st.number_input(
        "Discount Rate (%)",
        min_value=-99.0,
        max_value=1000.0,
        value=10.0,
        step=0.5
    )

# --------------------------------------------------
# PROJECT DURATION
# --------------------------------------------------
if "years" not in st.session_state:
    st.session_state.years = 5

def add_year():
    if st.session_state.years < 50:
        st.session_state.years += 1

st.button("Add Another Year", on_click=add_year)

years = st.number_input(
    "Number of Years",
    min_value=1,
    max_value=50,
    step=1,
    key="years"
)

# --------------------------------------------------
# ANNUAL CASH FLOW INPUTS
# --------------------------------------------------
st.divider()
st.subheader("Annual Cash Flows")

cash_flows = []

col1, col2 = st.columns(2)

for year in range(1, years + 1):
    column = col1 if year % 2 == 1 else col2

    with column:
        cash_flow = st.number_input(
            f"Year {year} Cash Flow ($)",
            value=20000.0,
            step=1000.0,
            key=f"cash_flow_{year}"
        )

        cash_flows.append(cash_flow)

# --------------------------------------------------
# NPV CALCULATION
# --------------------------------------------------
rate = discount_rate / 100

present_values = []

for year, cash_flow in enumerate(cash_flows, start=1):
    present_value = cash_flow / ((1 + rate) ** year)
    present_values.append(present_value)

total_present_value = sum(present_values)

npv = total_present_value - initial_cost

# Format numbers as currency
def currency(value):
    if value < 0:
        return f"-${abs(value):,.2f}"
    return f"${value:,.2f}"

# --------------------------------------------------
# RESULTS
# --------------------------------------------------
st.divider()
st.subheader("Investment Analysis")

if npv > 0.005:
    status = "Positive"
elif npv < -0.005:
    status = "Negative"
else:
    status = "Zero"

st.metric(
    label=f"NPV of Project ({status})",
    value=currency(npv)
)

if status == "Positive":
    st.success(
        "ACCEPT PROJECT: The NPV is positive. "
        "The project is expected to create value."
    )
elif status == "Negative":
    st.error(
        "REJECT PROJECT: The NPV is negative. "
        "The project is expected to destroy value."
    )
else:
    st.info(
        "BREAK EVEN: The NPV is approximately zero."
    )

# --------------------------------------------------
# FINANCIAL SUMMARY
# --------------------------------------------------
st.subheader("Financial Summary")

col1, col2 = st.columns(2)

with col1:
    st.metric(
        "Initial Investment",
        currency(initial_cost)
    )

with col2:
    st.metric(
        "PV of Future Cash Flows",
        currency(total_present_value)
    )

st.write(f"**Project Duration:** {years} Years")
st.write(f"**Discount Rate:** {discount_rate:.2f}%")

# --------------------------------------------------
# DETAILED CALCULATIONS
# --------------------------------------------------
with st.expander("View Year-by-Year Calculations"):

    st.write(
        f"Year 0: Initial Investment = "
        f"{currency(-initial_cost)}"
    )

    for year, (cf, pv) in enumerate(
        zip(cash_flows, present_values), start=1
    ):
        st.write(
            f"Year {year}: "
            f"Cash Flow = {currency(cf)} | "
            f"Present Value = {currency(pv)}"
        )

    st.divider()

    st.write(
        f"**Total Present Value:** "
        f"{currency(total_present_value)}"
    )

    st.write(
        f"**Less Initial Investment:** "
        f"{currency(initial_cost)}"
    )

    st.write(
        f"**Net Present Value:** {currency(npv)}"
    )
