# BA Agent MVP Plan Checklist

## 1. MVP 目标

本 MVP 的目标是跑通一个最小闭环：

1. 用户导入业务材料、业界 PRD 模板/案例、团队历史 PRD。
2. 知识库构建 Agent 将材料整理成 Obsidian/LLM Wiki 知识卡片。
3. 系统建立基础检索索引。
4. 用户输入 PRD 目标。
5. SDD PRD 生成 Agent 检索知识库，生成 Evidence Pack、Template Guidance、Team Style Pack。
6. Agent 提出澄清问题。
7. Agent 生成 OpenSpec 风格产物和 PRD 初稿。
8. 用户反馈 PRD 内容。
9. 反馈写回知识库 draft，用于后续优化。

MVP 不追求完整替代 BA，而是验证“知识沉淀 -> 证据检索 -> SDD PRD 生成 -> 用户反馈优化”这条主链路。

## 2. 技术栈选择

| 模块 | MVP 选择 | 原因 |
|---|---|---|
| 后端语言 | Python | Agent、文档解析、检索生态成熟。 |
| API 框架 | FastAPI | 轻量、清晰，后续方便产品化。 |
| Agent 编排 | LangGraph | 适合有状态、多步骤、可回溯的 Agent workflow。 |
| LLM 接入 | OpenAI API | 先保证生成质量和工具调用能力，后续可扩展多模型。 |
| 知识库 | Obsidian Vault + Markdown | 文件透明、Git 友好、适合人和 Agent 共用。 |
| Vault 自动化 | 自定义 `VaultTool` | 先封装文件操作，后续可接 Obsidian CLI。 |
| 文档解析 | MarkItDown、PyMuPDF、python-docx | 覆盖 Markdown、PDF、Word。 |
| 向量库 | Chroma 或 LanceDB | 本地轻量，适合 MVP。 |
| 元数据存储 | SQLite | 简单可靠，足够支撑本地 MVP。 |
| 搜索 | 向量检索 + 关键词检索 | 同时支持语义召回和精确术语匹配。 |
| 前端 | Streamlit | 快速搭交互，适合验证流程。 |
| SDD 规范 | OpenSpec 风格目录 | 管理 proposal、design、tasks、prd。 |
| 测试 | pytest | 覆盖核心工具和 workflow。 |
| 配置管理 | pydantic-settings | 管理模型、路径、检索参数。 |
| 版本管理 | Git | 追踪知识库、spec 和 PRD 变化。 |

## 3. MVP 目录结构 Checklist

- [ ] 创建 `src/`。
- [ ] 创建 `src/agents/knowledge_builder/`。
- [ ] 创建 `src/agents/prd_sdd_generator/`。
- [ ] 创建 `src/retrieval/`。
- [ ] 创建 `src/vault/`。
- [ ] 创建 `src/openspec/`。
- [ ] 创建 `src/parsers/`。
- [ ] 创建 `src/schemas/`。
- [ ] 创建 `src/evaluators/`。
- [ ] 创建 `prompts/knowledge_builder/`。
- [ ] 创建 `prompts/prd_sdd_generator/`。
- [ ] 创建 `vault/00-inbox/`。
- [ ] 创建 `vault/10-raw/`。
- [ ] 创建 `vault/20-wiki/`。
- [ ] 创建 `vault/30-spec-assets/`。
- [ ] 创建 `vault/90-archive/`。
- [ ] 创建 `openspec/project.md`。
- [ ] 创建 `openspec/specs/`。
- [ ] 创建 `openspec/changes/`。
- [ ] 创建 `evals/cases/`。
- [ ] 创建 `evals/rubrics/`。

## 4. Phase 1：项目基础设施

- [ ] 创建 Python 项目配置文件。
- [ ] 添加基础依赖。
- [ ] 配置 `.env.example`。
- [ ] 配置应用路径，例如 vault、openspec、vector store。
- [ ] 添加基础日志配置。
- [ ] 添加基础 pytest 配置。
- [ ] 创建最小 FastAPI app。
- [ ] 创建最小 Streamlit app。
- [ ] 添加本地启动说明。

验收标准：

- [ ] 可以启动 FastAPI 服务。
- [ ] 可以启动 Streamlit 页面。
- [ ] 可以读取配置。
- [ ] 可以运行 pytest。

## 5. Phase 2：Obsidian Vault 与 VaultTool

- [ ] 定义 Vault 目录规范。
- [ ] 定义 Markdown frontmatter schema。
- [ ] 定义知识卡片基础 schema。
- [ ] 定义业务知识卡片模板。
- [ ] 定义业界 PRD 实践卡片模板。
- [ ] 定义团队历史 PRD 模式卡片模板。
- [ ] 定义反馈记忆卡片模板。
- [ ] 实现 `VaultTool.init_vault()`。
- [ ] 实现 `VaultTool.create_note()`。
- [ ] 实现 `VaultTool.update_note()`。
- [ ] 实现 `VaultTool.read_note()`。
- [ ] 实现 `VaultTool.list_notes()`。
- [ ] 实现 `VaultTool.find_by_tag()`。
- [ ] 实现 `VaultTool.check_missing_links()`。
- [ ] 实现 `VaultTool.archive_note()`。

验收标准：

- [ ] 可以初始化 vault 目录。
- [ ] 可以按模板创建 Markdown 笔记。
- [ ] 笔记包含稳定 frontmatter。
- [ ] 可以读取和查询笔记。
- [ ] 可以发现缺失双链。

## 6. Phase 3：文档导入与解析

- [ ] 支持导入 Markdown 文件。
- [ ] 支持导入 PDF 文件。
- [ ] 支持导入 Word 文件。
- [ ] 支持扫描 `业界prd/模版`。
- [ ] 支持扫描 `业界prd/样例`。
- [ ] 支持用户放入 `vault/00-inbox/` 后批量导入。
- [ ] 抽取文档标题。
- [ ] 抽取来源路径。
- [ ] 抽取文档类型。
- [ ] 抽取基础摘要。
- [ ] 将原始文档记录写入 `vault/10-raw/`。

验收标准：

- [ ] 至少能成功解析 Markdown。
- [ ] 至少能成功解析 1 个 PDF。
- [ ] 至少能成功解析 1 个 Word。
- [ ] 能识别业界模板和样例目录。
- [ ] 每份导入材料都有来源记录。

## 7. Phase 4：知识库构建 Agent

- [ ] 设计 Knowledge Builder prompt。
- [ ] 设计材料分类规则。
- [ ] 区分业务事实、业界实践、团队历史 PRD。
- [ ] 抽取客户需求。
- [ ] 抽取干系人。
- [ ] 抽取功能能力。
- [ ] 抽取业务流程。
- [ ] 抽取业务规则。
- [ ] 抽取风险和约束。
- [ ] 抽取 PRD 模板结构。
- [ ] 抽取团队历史写作模式。
- [ ] 生成 Obsidian 双链。
- [ ] 生成 tags。
- [ ] 生成知识卡片。
- [ ] 检测重复卡片。
- [ ] 检测冲突知识。
- [ ] 输出知识库更新报告。

验收标准：

- [ ] 给定 1 份业务材料，能生成至少 3 类知识卡片。
- [ ] 给定业界 PRD 模板，能生成 Template Guidance。
- [ ] 给定历史 PRD，能生成 Team Style Pack。
- [ ] 每张卡片都能追溯来源。
- [ ] Agent 不把业界实践误当作业务事实。

## 8. Phase 5：检索系统

- [ ] 建立 SQLite metadata 表。
- [ ] 建立向量索引。
- [ ] 建立关键词检索。
- [ ] 支持按 `type` 过滤。
- [ ] 支持按 `source_type` 过滤。
- [ ] 支持按 `tags` 过滤。
- [ ] 实现业务事实检索。
- [ ] 实现业界实践检索。
- [ ] 实现团队历史 PRD 检索。
- [ ] 合并检索结果并去重。
- [ ] 保留来源路径和引用片段。

验收标准：

- [ ] 输入需求目标后，可以返回相关业务知识。
- [ ] 可以返回相关 PRD 模板实践。
- [ ] 可以返回相关团队历史 PRD 模式。
- [ ] 检索结果包含来源和类型标签。

## 9. Phase 6：OpenSpec SDD 工作流

- [ ] 定义 `openspec/project.md` 内容。
- [ ] 定义 `proposal.md` 模板。
- [ ] 定义 `design.md` 模板。
- [ ] 定义 `tasks.md` 模板。
- [ ] 定义 `prd.md` 模板。
- [ ] 实现 change slug 生成。
- [ ] 实现 change 目录创建。
- [ ] 实现 proposal 文件写入。
- [ ] 实现 design 文件写入。
- [ ] 实现 tasks 文件写入。
- [ ] 实现 prd 文件写入。

验收标准：

- [ ] 输入一个需求名称，可以创建 `openspec/changes/<slug>/`。
- [ ] change 目录包含 proposal、design、tasks、prd。
- [ ] 文件结构稳定，可被 Git 追踪。

## 10. Phase 7：SDD PRD 生成 Agent

- [ ] 设计 PRD Generator prompt。
- [ ] 接收用户输入的 PRD 目标。
- [ ] 判断需求类型。
- [ ] 检索 Evidence Pack。
- [ ] 检索 Template Guidance。
- [ ] 检索 Team Style Pack。
- [ ] 判断信息是否足够。
- [ ] 生成澄清问题。
- [ ] 支持用户回答澄清问题。
- [ ] 生成 OpenSpec proposal。
- [ ] 生成 OpenSpec design。
- [ ] 生成 OpenSpec tasks。
- [ ] 生成 PRD 初稿。
- [ ] 标注事实、假设、来源。
- [ ] 标注待确认问题。
- [ ] 执行 PRD 自评审。

验收标准：

- [ ] 能基于知识库生成 PRD 初稿。
- [ ] PRD 包含来源引用。
- [ ] PRD 明确区分事实、假设、待确认问题。
- [ ] PRD 包含用户故事和验收标准。
- [ ] 生成内容符合 OpenSpec 风格目录。

## 11. Phase 8：自适应反馈机制 MVP

- [ ] 支持用户对整份 PRD 评分。
- [ ] 支持用户对 section 评分。
- [ ] 支持用户标记错误。
- [ ] 支持用户标记缺失。
- [ ] 支持用户标记有用。
- [ ] 支持记录用户编辑 diff。
- [ ] 将反馈写入 `vault/20-wiki/feedback/`。
- [ ] 生成模板优化建议。
- [ ] 生成知识库更新建议。
- [ ] 将高风险更新写入 draft，不直接覆盖 stable。

验收标准：

- [ ] 用户反馈可以被保存。
- [ ] 用户编辑前后差异可以被记录。
- [ ] 反馈能关联到具体 PRD section。
- [ ] Agent 能基于反馈提出模板或知识库优化建议。

## 12. Phase 9：Streamlit 前端

- [ ] 创建首页。
- [ ] 创建材料上传页面。
- [ ] 创建知识库构建页面。
- [ ] 创建知识卡片浏览页面。
- [ ] 创建 PRD 生成页面。
- [ ] 展示检索结果。
- [ ] 展示 Evidence Pack。
- [ ] 展示 Template Guidance。
- [ ] 展示 Team Style Pack。
- [ ] 展示澄清问题。
- [ ] 支持用户填写澄清答案。
- [ ] 展示 PRD 预览。
- [ ] 支持导出 Markdown。
- [ ] 支持打开生成的 OpenSpec change 路径。

验收标准：

- [ ] 用户可以通过页面上传材料。
- [ ] 用户可以点击生成知识库。
- [ ] 用户可以输入 PRD 目标。
- [ ] 用户可以看到 PRD 初稿。
- [ ] 用户可以提交反馈。

## 13. Phase 10：测试与评估

- [ ] 准备 3 个 MVP 测试场景。
- [ ] 准备 5 到 10 份输入材料。
- [ ] 准备 1 份业界 PRD 模板测试集。
- [ ] 准备 1 份历史 PRD 测试集。
- [ ] 测试知识卡片抽取质量。
- [ ] 测试三路检索结果。
- [ ] 测试 PRD 来源引用。
- [ ] 测试澄清问题质量。
- [ ] 测试 OpenSpec 文件生成。
- [ ] 测试反馈写回。
- [ ] 编写核心单元测试。
- [ ] 编写端到端 smoke test。

验收标准：

- [ ] E2E 流程可以从材料导入跑到 PRD 生成。
- [ ] 生成 PRD 不把模板参考当作业务事实。
- [ ] 至少 1 个测试场景可以完整生成 OpenSpec change。
- [ ] 反馈机制可以写回 vault。

## 14. MVP 不做范围

- [ ] 不做完整多租户权限系统。
- [ ] 不做复杂团队协作审批。
- [ ] 不做自动训练大模型。
- [ ] 不做完整 RLHF。
- [ ] 不做 Jira/Confluence/Notion 集成。
- [ ] 不做 Word/PDF 精美导出。
- [ ] 不做生产级审计系统。
- [ ] 不做自动覆盖 stable 知识库。

## 15. MVP 成功标准

- [ ] 可以导入至少 3 类资料：业务文档、业界 PRD、历史 PRD。
- [ ] 可以生成 Obsidian Markdown 知识卡片。
- [ ] 可以按业务事实、业界实践、团队历史 PRD 三路检索。
- [ ] 可以生成 Evidence Pack、Template Guidance、Team Style Pack。
- [ ] 可以提出高质量澄清问题。
- [ ] 可以生成 OpenSpec 风格的 proposal、design、tasks、prd。
- [ ] 可以展示和导出 PRD 初稿。
- [ ] 可以记录用户反馈并写回知识库 draft。

## 16. 推荐开发顺序

1. 先做目录结构和 VaultTool。
2. 再做 Markdown 导入和知识卡片生成。
3. 再做检索系统。
4. 再做 OpenSpec 文件生成。
5. 再接 SDD PRD 生成 Agent。
6. 最后做 Streamlit 页面和反馈闭环。

这个顺序比较朴素，但很抗摔。先让知识能被稳定读写，再让 Agent 变聪明。
