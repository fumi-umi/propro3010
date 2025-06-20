import streamlit as st

def calculate_costs(eating_out_frequency, avg_eating_out_cost, cooking_frequency, avg_cooking_cost_per_meal):
    """
    外食と自炊の年間費用を計算します。
    """
    # 年間の外食回数と費用
    annual_eating_out_count = eating_out_frequency * 52  # 週あたりの回数 * 52週
    annual_eating_out_cost = annual_eating_out_count * avg_eating_out_cost

    # 年間の自炊回数と費用
    annual_cooking_count = cooking_frequency * 52 # 週あたりの回数 * 52週
    annual_cooking_cost = annual_cooking_count * avg_cooking_cost_per_meal

    return annual_eating_out_cost, annual_cooking_cost

st.set_page_config(layout="centered", page_title="外食 vs 自炊 費用比較")

st.title("🍜 外食 vs 🍳 自炊 費用比較アプリ")
st.write("外食と自炊の年間費用を比較し、食費の傾向を把握しましょう。")

st.header("🍚 外食に関する情報")
eating_out_frequency = st.slider("週に外食する回数", 0, 21, 5) # 1日3回で週21回
avg_eating_out_cost = st.number_input("1回の外食にかかる平均費用 (円)", min_value=0, value=1000, step=100)

st.header("🧑‍🍳 自炊に関する情報")
cooking_frequency = st.slider("週に自炊する回数", 0, 21, 10)
avg_cooking_cost_per_meal = st.number_input("1回の自炊にかかる平均費用 (円)", min_value=0, value=400, step=50)

if st.button("費用を計算する"):
    annual_eating_out_cost, annual_cooking_cost = calculate_costs(
        eating_out_frequency,
        avg_eating_out_cost,
        cooking_frequency,
        avg_cooking_cost_per_meal
    )

    st.header("📊 計算結果")
    st.write(f"**年間の外食費用:** ¥{annual_eating_out_cost:,.0f}")
    st.write(f"**年間の自炊費用:** ¥{annual_cooking_cost:,.0f}")

    if annual_eating_out_cost > annual_cooking_cost:
        st.success(f"年間で自炊の方が約 ¥{annual_eating_out_cost - annual_cooking_cost:,.0f} お得です！")
    elif annual_cooking_cost > annual_eating_out_cost:
        st.info(f"年間で外食の方が約 ¥{annual_cooking_cost - annual_eating_out_cost:,.0f} お得です！（外食の回数を減らすことを検討すると良いかもしれません）")
    else:
        st.warning("外食と自炊の年間費用はほぼ同じです。")

    st.subheader("💡 ポイント")
    st.markdown("""
    * 週の外食回数を減らすことで、食費を大幅に節約できる可能性があります。
    * 自炊の材料費や手間を考慮し、現実的な平均費用を入力しましょう。
    * このアプリはあくまで目安です。実際の家計状況に合わせて調整してください。
    """)

    # 円グラフの表示 (Optional: Streamlitの機能で簡単に可視化)
    import pandas as pd
    import plotly.express as px

    data = pd.DataFrame({
        '費用タイプ': ['外食', '自炊'],
        '年間費用': [annual_eating_out_cost, annual_cooking_cost]
    })

    fig = px.pie(data, values='年間費用', names='費用タイプ', title='年間食費の内訳')
    st.plotly_chart(fig)
    