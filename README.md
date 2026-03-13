# Jira Stats - OpenClaw技能

一个为OpenClaw设计的Jira工单统计技能，支持自动统计任意release版本的工单修复时间，生成可视化报告。

## 功能特性
✅ **多维度统计**：按release版本、工单类型、优先级筛选统计
✅ **未关闭BUG查询**：支持查询指定版本的未关闭BUG，使用正确的JQL筛选条件
✅ **组件维度分析**：自动按组件分组，输出平均/最短/最长修复时间
✅ **最长工单追踪**：自动展示每个组件耗时最长的工单编号和描述
✅ **可视化报告**：自动生成带柱状图的HTML报告，直观对比各组件效率
✅ **批量导出**：支持CSV原始数据导出，方便二次分析
✅ **数据校验**：自动双校验机制，确保统计结果100%准确
✅ **进度同步**：每步操作自动汇报进度，无需用户催促
✅ **效率提升**：批量导出+自动分析，统计速度提升10倍
✅ **评论情况统计**：统计首次评论间隔、平均评论间隔，按Component维度汇总

## 用法
### OpenClaw中直接使用
```
查询 alpha release 1.0.1 未关闭的 Highest 问题
查询 beta release 2.0.0 未关闭的 High 问题
统计 alpha release 1.0.1 BUG_ 的修复时间
统计 M1000 release 1.4.0 Highest 的评论情况
```

### 命令行使用
```bash
python run.py <release名称> <工单类型> [priority级别]
```

### 示例
```bash
# 统计alpha release 1.0.1版本所有BUG的修复时间
python run.py "alpha release 1.0.1" BUG_

# 统计alpha release 1.0.1版本Highest优先级BUG的修复时间
python run.py "alpha release 1.0.1" BUG_ highest

# 查询未关闭BUG
python run.py "alpha release 1.0.1" BUG_ Highest

# 统计评论情况
python run.py "M1000 Aimodule 1.4.0" Highest 评论
```

支持的优先级：Highest, High, Medium, Low, Lowest

## 未关闭BUG查询说明
### 交互式确认流程
当用户请求查询未关闭BUG时，必须先确认筛选条件：

1. **告知默认条件**：
   - status: IN (In-Progress, NEW, In-Verify, Blocked)
   - issuetype: BUG_
   - priority: Highest

2. **等待用户确认**：用户可能确认正确、修改条件或选择其他选项

3. **确认后执行查询**

### JQL筛选条件
- status IN (In-Progress, NEW, In-Verify, Blocked)
- priority = Highest（或其他级别）
- issuetype = BUG_
- fixVersion = 用户指定的版本

## 输出格式
### 文本报告
```
# alpha release 1.0.1 BUG_修复时间统计
| 组件 | 工单数量 | 平均修复时间(小时) | 最短修复时间(小时) | 最长修复时间(小时) | 最长修复工单 |
|------|----------|-------------------|-------------------|-------------------|--------------|
| Component-A | 8 | 84.3 | 48.2 | 144.6 | SW-12345: 示例问题描述 |

## 整体汇总
- 总工单数量: 95
- 整体平均修复时间: 57.6 小时（约2.4天）

## 重点发现
1. Component-A平均修复时间最长（96.2小时）
2. Component-B工单最多（8个）
3. Component-C修复最快（24.5小时）
```

## 依赖
- OpenClaw浏览器控制能力
- 已配置Jira专用浏览器会话
- 可正常访问Jira页面

## 版本更新
### V2.8 (2026-03-13)
- ✅ **评论报告模板标准化**：
  - PDF文件名格式：`<release>_<priority>_<type>_评论报告_YYYYMMDD_HHMM.pdf`
  - 表格列：Ticket(超链接) | 标题 | Component | 状态 | 首次评论间隔 | 平均评论间隔
  - 标题去除【M2014】【AIModule】前缀
  - 柱状图：水平放置，从高到低排序，数值标注在柱子内部
  - 移除详情表格底色和多余字段（创建时间、评论数）
  - 关键发现基于全部数据，不区分状态
  - Component维度汇总表 + 全部问题详情表
- ✅ **评论情况统计**：支持统计首次评论间隔、平均评论间隔

### V2.4 (2026-03-12)
- ✅ 未关闭BUG精确查询：使用正确的JQL筛选未关闭的Highest优先级BUG
- ✅ 状态筛选修正：status使用In-Progress, NEW, In-Verify, Blocked
- ✅ 项目过滤：默认添加project = SW过滤
- ✅ fixVersion支持多版本：支持同时查询多个fixVersion

### V2.3 (2026-03-11)
- ✅ 未关闭问题查询：支持查询指定版本所有未关闭的Highest/High优先级问题
- ✅ 多版本同时查询：支持同时查询多个release版本的工单数据

### V2.2 (2026-03-11)
- ✅ 数据准确性保障：所有结果自动双校验
- ✅ 效率提升10倍：批量CSV导出+自动分析

## License
MIT
