# 储流 WMS（StockFlow）仓储管理系统 · 前端

储流 WMS 的前端工程，基于 JeecgBoot-Vue3 二次开发，覆盖 WMS 全业务流程页面，并扩展了 IoT 监控大屏与 AI 智能助手对话界面。

## 技术栈

| 类别 | 技术 |
|------|------|
| 核心框架 | Vue 3.5 + TypeScript |
| 构建工具 | Vite 6 |
| UI 组件库 | Ant Design Vue 4 |
| 基础平台 | JeecgBoot-Vue3 3.8.0（动态菜单 / 按钮级权限 / 在线代码生成） |
| 可视化 | ECharts（大屏图表定制） |
| 打印 | Lodop（顺丰面单 / 库位标签打印） |

## 功能页面

- **商品基础资料**：品牌 / 类别 / 商品 / 属性 / 包装材料 / 货主 / 承运商
- **仓储管理**：仓库 / 库区 / 储位
- **入库管理**：入库单（Excel 导入）/ 收货 / 上架任务 / Lodop 打印
- **出库与库存**：出库单 / 库存查询
- **波次与拣货**：波次创建 / 拣货任务 / 分拣，含顺丰面单打印
- **发运管理**：发运单 / 打包 / 面单打印模板
- **IoT 监控**：环境监控大屏（ECharts 定制图表，WebSocket 实时推送）
- **AI 助手**：智能对话页面（对接后端 Spring AI，支持 RAG 与库存查询工具调用）

## 快速开始

```bash
# 安装依赖（Node 18+）
npm install

# 启动开发服务器（端口 3100）
npm run dev
```

> 需先启动后端服务（默认 `http://localhost:8080/jeecg-boot`），接口代理与多仓库请求头配置见 `vite.config.ts` 与 `src/utils`。

## 工程结构

```
├── src
│   ├── views
│   │   ├── goods / warehouse     # 商品、仓储
│   │   ├── inorder / inventory   # 入库、库存
│   │   ├── outorder / wave       # 出库、波次拣货
│   │   ├── shipment / printTemplate  # 发运、打印模板
│   │   ├── iot / dashboard       # IoT 监控、数据大屏
│   │   └── super/airag           # AI 智能助手
│   ├── components/chart          # ECharts 图表组件（大屏定制）
│   └── ...                       # 框架层（布局 / 权限 / 组件 / 工具）
└── vite.config.ts                # Vite 配置（代理 / 多仓库）
```

## 说明

- 本项目基于 [JeecgBoot-Vue3](https://github.com/jeecgboot/jeecgboot-vue3) 二次开发，业务页面位于 `src/views` 各业务目录，页面与 API 文件同目录组织
- 框架级定制：请求头注入 warehouseId 实现多仓库切换
