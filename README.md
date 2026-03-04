# 🐍 Snake Game - 测试仓库

贪吃蛇游戏独立测试仓库，包含 API 测试、E2E 测试、性能测试和测试用例文档。

## 📁 目录结构

```
snake-tests/
├── api/               # API 接口自动化测试
├── e2e/               # 端到端测试 (Playwright/Cypress)
├── performance/       # 压力测试 (k6/artillery)
├── test-cases/        # 测试用例文档
│   └── snake-game.md  # 主测试用例文档 (54 条用例)
├── reports/           # 测试报告
└── README.md
```

## 🧪 测试用例概览

| 模块 | 用例数 |
|------|--------|
| 用户模块 | 10 条 |
| 游戏模块 | 18 条 |
| API 测试 | 16 条 |
| 游戏流程 | 6 条 |
| 边界测试 | 4 条 |
| **总计** | **54 条** |

详细用例见 [test-cases/snake-game.md](./test-cases/snake-game.md)

## 🔗 关联仓库

- **后端:** snake-backend (NestJS)
- **前端:** snake-frontend (React)
- **测试:** snake-tests (本仓库)

## 👤 维护者

Bug (测试小精灵) 🐛

---

*创建时间: 2026-03-04*
