# Finance-Agent: A 股多智能体金融分析系统

Finance-Agent 是一个基于 **LangGraph** 工作流和 **Model Context Protocol (MCP)** 协议开发的智能化 A 股股票分析系统。该系统模拟了专业的投研团队，通过五个协同工作的 AI 智能体（Agents），为用户提供从基本面、技术面到实时新闻的深度投资分析与决策建议。

## 🚀 核心特性

* **多 Agent 协作**：采用 LangGraph 编排工作流，实现智能体之间的状态传递与任务解耦。
* **MCP 协议驱动**：利用 Model Context Protocol 接入工具，实时获取最新的 A 股市场数据、财务报表及新闻。
* **全方位分析**：覆盖基本面、技术面、估值及舆情四个核心维度。
* **智能化总结**：由总结 Agent 汇聚多维度数据，输出结构化的投资建议，避免信息碎片化。

---

## 🏗 系统架构

系统由五个专门的智能体组成，形成一个“并行分析-深度汇总”的拓扑结构：

| 智能体名称 | 核心职责 | 分析维度 |
| :--- | :--- | :--- |
| **基本面分析 Agent** | 评估盈利能力 | 营收、毛利、ROE、负债率等 |
| **技术分析 Agent** | 研判价格走势 | MA, KDJ, RSI, 支撑位与压力位 |
| **估值分析 Agent** | 评估价格合理性 | PE, PB, PS, 历史分位值 |
| **新闻分析 Agent** | 捕捉市场情绪 | 实时公告、行业动态、舆情监控 |
| **总结 Agent (Lead)** | 生成决策报告 | 综合上述数据，给出最终投资评级 |

### 工作流示意图

```mermaid
graph TD
    Start((开始分析)) --> Input[输入 A 股代码]
    Input --> Router{LangGraph 调度}

    subgraph Analysis_Layer [专业分析层]
        direction TB
        Router --> Fundamental[基本面分析 Agent]
        Router --> Technical[技术分析 Agent]
        Router --> Valuation[估值分析 Agent]
        Router --> News[新闻分析 Agent]
    end

    %% 数据获取层
    Fundamental <--> MCP[MCP Server: A股数据源]
    Technical <--> MCP
    Valuation <--> MCP
    News <--> MCP

    %% 汇总输出
    Fundamental --> Summary[总结 Agent]
    Technical --> Summary
    Valuation --> Summary
    News --> Summary

    Summary --> Report[生成结构化投资报告]
    Report --> End((结束))

    style MCP fill:#f9f,stroke:#333,stroke-width:2px
    style Summary fill:#dfd,stroke:#333,stroke-width:2px