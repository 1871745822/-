import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# 页面配置
st.set_page_config(
    page_title="员工绩效数据看板",
    page_icon="📊",
    layout="wide"
)

# 标题
st.title("📊 企业员工绩效与成本数据看板（Streamlit 可编辑版）")

# 1. 数据导入/上传（支持在线编辑数据）
st.sidebar.header("📤 数据管理")
uploaded_file = st.sidebar.file_uploader("上传CSV/Excel数据", type=["csv", "xlsx"])

# 示例数据（无上传时使用，可直接编辑）
default_data = {
    "一级部门": ["技术部", "市场部", "销售部", "人事部", "财务部"],
    "员工数": [25, 18, 32, 6, 8],
    "平均账单小时": [8.2, 6.8, 7.5, 5.9, 6.2],
    "平均完成率": [1.05, 0.92, 0.98, 1.10, 1.02],
    "日均加班时长": [1.8, 0.7, 1.2, 0.3, 0.5]
}
df_default = pd.DataFrame(default_data)

# 加载数据（优先使用上传的文件，否则用示例数据）
if uploaded_file:
    if uploaded_file.name.endswith(".csv"):
        df = pd.read_csv(uploaded_file)
    else:
        df = pd.read_excel(uploaded_file)
    st.sidebar.success("✅ 数据上传成功！")
else:
    df = df_default
    st.sidebar.info("使用示例数据，可上传自有CSV/Excel覆盖")

# 2. 在线编辑数据（核心功能）
st.sidebar.subheader("✏️ 在线编辑数据")
edited_df = st.sidebar.data_editor(
    df,
    num_rows="dynamic",  # 支持添加/删除行
    use_container_width=True,
    column_config={
        "一级部门": st.column_config.TextColumn("部门名称", required=True),
        "员工数": st.column_config.NumberColumn("员工数", min_value=0, step=1),
        "平均账单小时": st.column_config.NumberColumn("平均账单小时", min_value=0, step=0.1),
        "平均完成率": st.column_config.NumberColumn("完成率(倍)", min_value=0, step=0.01),
        "日均加班时长": st.column_config.NumberColumn("加班时长(小时)", min_value=0, step=0.1)
    }
)

# 3. 可视化图表（支持在线编辑样式）
tab1, tab2, tab3 = st.tabs(["📈 部门人员分布", "📊 账单小时对比", "📉 加班时长分析"])

with tab1:
    # 饼图（可在线修改颜色/标题）
    fig1 = px.pie(
        edited_df,
        values="员工数",
        names="一级部门",
        title="一级部门人员分布",
        color_discrete_sequence=px.colors.qualitative.Set2
    )
    # 在线编辑图表样式
    fig1.update_layout(
        title_font_size=18,
        legend_title="部门",
        height=500
    )
    st.plotly_chart(fig1, use_container_width=True)
    
    # 编辑图表的快捷设置
    st.subheader("⚙️ 图表编辑（饼图）")
    new_title = st.text_input("修改图表标题", value="一级部门人员分布")
    fig1.update_layout(title=new_title)
    st.plotly_chart(fig1, use_container_width=True)

with tab2:
    # 柱状图
    fig2 = px.bar(
        edited_df,
        x="一级部门",
        y="平均账单小时",
        color="平均完成率",
        title="各部门平均账单小时",
        color_continuous_scale=px.colors.sequential.Blues
    )
    st.plotly_chart(fig2, use_container_width=True)

with tab3:
    # 箱线图（加班时长）
    fig3 = px.box(
        edited_df,
        x="一级部门",
        y="日均加班时长",
        title="各部门日均加班时长分布",
        points="all"
    )
    st.plotly_chart(fig3, use_container_width=True)

# 4. 数据表格展示
st.subheader("📋 员工绩效数据表格（可编辑）")
st.dataframe(edited_df, use_container_width=True)

# 5. 导出数据/图表
st.sidebar.subheader("💾 导出")
if st.sidebar.button("导出当前数据为CSV"):
    csv = edited_df.to_csv(index=False)
    st.sidebar.download_button(
        label="下载CSV",
        data=csv,
        file_name="员工绩效数据.csv",
        mime="text/csv"
    )
