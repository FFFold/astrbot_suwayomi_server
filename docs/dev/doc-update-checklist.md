# 文档更新清单

不同类型的变更需要更新不同的文件。以下是完整清单。

## 版本发布

| 文件 | 更新内容 |
|------|---------|
| `metadata.yaml:4` | `version` 字段 |
| `pyproject.toml:3` | `version` 字段（保持同步） |
| `README.md:17` | 版本 badge URL 和 alt 文本中的版本号 |
| `CHANGELOG.md` | 在顶部新增版本条目 |

## 新增/修改命令

| 文件 | 更新内容 |
|------|---------|
| `main.py` | 命令方法 + docstring（用户帮助文本） |
| `main.py:183-207` | `/漫画 帮助` 命令的完整帮助文本 |
| `README.md:48-59` | 命令表格 |
| `README.md:63-113` | 使用示例（如有新交互流程） |
| `AGENTS.md:43` | 架构描述中的命令数量 |
| `CONTRIBUTING.md` | 添加新命令章节（如适用） |

## 新增/修改配置项

| 文件 | 更新内容 |
|------|---------|
| `_conf_schema.json` | 配置字段定义（name, description, hint, default） |
| `main.py` | `self.config.get()` 读取逻辑 |
| `README.md:164-184` | 配置表格（基本设置 / 阅读设置） |
| `docs/setup.md:178-191` | 配置参考表格 |
| `AGENTS.md:86-95` | Config Options 段落 |

## 新增/修改运行时依赖

| 文件 | 更新内容 |
|------|---------|
| `requirements.txt` | 新依赖 |
| `AGENTS.md:112` | File Conventions 中的依赖列表描述 |

## 架构变更

| 文件 | 更新内容 |
|------|---------|
| `AGENTS.md:33-47` | Architecture 段落 |
| `AGENTS.md:75-84` | Key Helper Methods 段落 |
| `docs/dev/development.md` | 架构图、模块说明、数据流 |
| `AGENTS.md:49-73` | Critical Quirks（如有新的陷阱） |

## 测试变更

| 文件 | 更新内容 |
|------|---------|
| `AGENTS.md:19` | 单元测试数量 |
| `docs/dev/development.md:23-27` | 各测试文件的测试数量 |
| `CONTRIBUTING.md` | 测试运行命令（如有新测试文件） |

## API 变更（GraphQL 查询/变更）

| 文件 | 更新内容 |
|------|---------|
| `suwayomi/client.py` | 新增/修改查询方法 |
| `suwayomi/models.py` | 对应的 `from_dict()` 方法 |
| `docs/dev/suwayomi-api.md` | API 参考文档 |

## 用户可见文本变更

| 文件 | 更新内容 |
|------|---------|
| `main.py` | 命令 docstring、帮助文本、错误提示、状态消息 |
| `metadata.yaml:3,7` | `desc` / `short_desc` 描述 |
| `README.md:11,24` | 短描述 / 完整描述 |

## 快速参考：文件 → 触发场景

| 文件 | 何时需要改 |
|------|-----------|
| `metadata.yaml` | 版本发布、描述变更 |
| `pyproject.toml` | 版本发布 |
| `README.md` | 版本发布、新命令、新配置、新功能、描述变更 |
| `CHANGELOG.md` | 版本发布 |
| `main.py` | 新命令、配置读取、用户文本变更 |
| `_conf_schema.json` | 新增/修改配置 |
| `AGENTS.md` | 架构变更、新命令、新配置、测试变更、依赖变更 |
| `docs/setup.md` | 新增/修改配置 |
| `docs/dev/development.md` | 架构变更、测试变更 |
| `docs/dev/suwayomi-api.md` | GraphQL API 变更 |
| `CONTRIBUTING.md` | 新增命令类型、测试文件变更 |
| `requirements.txt` | 新增/修改依赖 |
