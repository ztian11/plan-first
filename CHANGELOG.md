# Changelog

本项目的显著变更记录。格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [SemVer](https://semver.org/lang/zh-CN/)。

## [0.2.0] - 2026-10-20

### Added

- Phase 3 可行性评审：规划产出后、用户确认前的强制关卡，固定检查假设冲突 / 未知当已知 / 返工缺口三项，结论落进规划文档
- 评审结果三态：通过 / 有条件通过（前置动作入任务表）/ 打回修订
- 模板新增第 7 节「可行性评审结论」，示例规划同步填充
- 轻量模式增加 1 行可行性自检

## [0.1.0] - 2026-10-12

### Added

- 初始版本：先规划后编码的四阶段工作流（澄清 → 调研 → 规划 → 确认门 → 执行）
- 实施规划文档模板（`references/plan-template.md`）
- 完整示例规划（`examples/example-plan.md`）
- 轻量模式：≤ 2 步小任务的降级流程
