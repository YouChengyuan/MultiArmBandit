# MultiArmBandit


# 多臂老虎机 (Multi-Armed Bandit) 仿真与多策略对比

本项目聚焦于多臂老虎机（Multi-Armed Bandit, MAB）问题的 Python 仿真与多种经典/创新算法的性能对比，帮助理解强化学习中的“探索-利用”权衡，目前最新版本是基于Hoeffding's inequality的原创算法，设定为200步的总步数，100000轮随机测试结果为：平均累计遗憾约为20.43，标准差7.66，为当前最优表现。

---

## 目录

- [项目简介](#项目简介)
- [主要内容与功能](#主要内容与功能)
- [运行环境](#运行环境)
- [核心类与函数说明](#核心类与函数说明)
- [多种策略实现](#多种策略实现)
    - [extinction_player 系列](#extinction_player-系列)
    - [extinction_statiscian 系列](#extinction_statiscian-系列)
- [实验与用法示例](#实验与用法示例)
- [致谢](#致谢)

---

## 项目简介

多臂老虎机问题是强化学习领域的经典问题。玩家面对一台有多个拉杆的老虎机，每个拉杆的中奖概率未知，目标是在有限的尝试次数内，最大化累计奖励。  
本项目通过 Jupyter Notebook（`slot_machine.ipynb`）实现环境搭建、策略设计、批量实验和可视化对比。

---

## 主要内容与功能

- **伯努利多臂老虎机环境搭建**  
  支持任意拉杆数、概率分布可自定义，可追踪奖励、遗憾等统计量。
- **多种策略实现与性能对比**  
  包括正反馈消退算法 (`extinction_player`)、统计型优化算法 (`extinction_statiscian`) 及其多版本改进。
- **批量实验与统计分析工具**  
  自动多轮仿真、统计均值/分位数/标准差、异常案例分析、可视化结果。
- **实验辅助函数**  
  连续奖励次数分布分析、奖励均值收敛性分析等。

---

## 运行环境

- Python 3.7+
- Jupyter Notebook
- 依赖库：
  - numpy
  - matplotlib
  - pandas
  - copy

安装依赖：
```bash
pip install numpy matplotlib pandas
```

---

## 核心类与函数说明

### BernoulliBandit

- **描述**：多臂伯努利老虎机环境。
- **主要属性**：`probs`（臂的真实概率）、`counts`（选择次数）、`values`（平均奖励）、`cumulative_reward`、`cumulative_regret`、`history` 等。
- **主要方法**：
  - `step(k)`：拉动编号为 k 的拉杆，返回奖励。
  - `reset_stats()`：重置统计量。
  - `plot_probabilities()`：可视化臂的概率分布。
  - `plot_performance()`：绘制累计奖励/遗憾。
  - `report_all()`：输出统计结果和图表。

### 实验与分析辅助函数

- `test_strategy(strategy_func, test_iter, anomalous_threshold)` 多轮批量测试策略性能。
- `experiment_1()` 分析样本均值收敛特性。
- `experiment_2()` 分析各臂连续奖励次数分布。

---

## 策略更新

### 第一阶段：extinction_player 系列

- **Ver1**：正反馈消退算法，消退后随机选择新臂。
- **Ver2**：消退后优先选样本均值最高的臂。
- **Ver3**：加入大数定律限制及“打擂台”机制，逐步淘汰非最优臂。

### 第二阶段：extinction_statiscian 系列

> **说明：**  
> extinction_statiscian 系列为在正反馈消退算法基础上加入统计特征的多臂选择算法。
> 你可以在 `slot_machine.ipynb` 中找到各版本实现、调用和性能可视化。

- **Ver1**：根据Hoeffding's inequality 计算样本均值95%置信上限，以此为消退后重新选择臂的依据，显著降低了测试结果标准差，大量减少异常值。
- **Ver2**：调整置信度为80%，进一步降低测试结果累计遗憾均值及方差，达到目前最优水平（平均累计遗憾约为20.43，标准差7.66）。
- **Ver3**：正在施工🚧🚧
- ...

---

## 实验与用法示例

1. 克隆仓库到本地并安装依赖。
2. 用 Jupyter Notebook 打开 `slot_machine.ipynb`。
3. 逐步运行单元，体验多种策略的仿真对比。
4. 可自定义策略函数，调用 `test_strategy` 自动批量测试与统计。

---

## 致谢

- 部分代码为 AI 辅助生成并参照社区公开资料，感谢开源社区的支持。
- 欢迎 issue 与 PR 进一步优化实验与算法实现。

---

**补充**：如需自定义实验、添加新策略，只需按 notebook 模板实现策略函数，并用 `test_strategy` 批量测试即可。
