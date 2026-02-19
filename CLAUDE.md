# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个纯静态的房贷计算器 Web 应用，没有构建工具或包管理器。项目使用原生 JavaScript + Zepto.js（jQuery 的轻量级替代库）开发。

## 项目结构

```
├── index.html              # 主入口文件，包含完整的应用界面
├── css/
│   ├── my-common.min.css   # 通用样式（压缩版）
│   └── my-houseloan.min.css # 房贷计算器专用样式（压缩版）
├── js/
│   ├── zepto.min.js        # jQuery 替代库
│   ├── houseloan_calculator.js  # 核心计算逻辑
│   └── common_tpl.js       # 通用模板函数
├── images/                 # 静态图片资源
└── post/                   # 房贷知识文章页面
```

## 本地开发

由于是纯静态项目，直接用浏览器打开 `index.html` 即可，或使用任意静态服务器：

```bash
# Python 3
python -m http.server 8000

# Node.js (需要安装 http-server)
npx http-server
```

## 核心架构

### 贷款类型
应用支持三种贷款模式，通过 `loanType` 变量控制：
- `0`: 商业贷款
- `1`: 公积金贷款
- `2`: 组合贷款（商业 + 公积金）

### 还款方式
- 等额本息：每月还款额固定
- 等额本金：每月本金固定，利息递减

### 利率数据结构
历史利率数据存储在数组中：
- `businessShortRateArr6/12/36/60`: 商贷短期利率（不同期限）
- `businessLongRateArr`: 商贷长期利率（5年以上）
- `PAFShortRateArr`: 公积金短期利率
- `PAFLongRateArr`: 公积金长期利率

数组索引对应历史时期，索引 12 为"最新基准利率"。

### 视图切换
应用使用简单的显示/隐藏机制切换视图：
- `calc_input`: 计算输入页面
- `calc_result`: 计算结果页面
- `data_detail`: 数据明细页面

通过 `$("[view=xxx]")` 选择器控制。

### 关键函数
- `userInputCheck()`: 验证用户输入
- `calculate()`: 执行计算
- `businessRateUpdate()` / `PAFRateUpdate()`: 更新利率显示

## 注意事项

1. CSS 和 JS 文件使用压缩版本，直接编辑时需要注意代码格式
2. 使用 Zepto.js 而非 jQuery，API 基本兼容但功能子集
3. 利率数据硬编码在 `houseloan_calculator.js` 顶部，更新利率时需要修改这些数组
