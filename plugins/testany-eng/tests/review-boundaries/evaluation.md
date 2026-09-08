# 四个 Review Skill 的小型边界回归

这些输入验证 `guide`、`hld-reviewer`、`lld-reviewer`、`code-reviewer` 的路由与决定，
不是产品发布门禁，也不是新的评分平台。案例是去标识化的自然请求与已有材料；
`raw/` 不包含答案，`grader/expected.json` 只给评估主持者使用。

## 自动检查

在仓库根目录：

```sh
python3 -m unittest discover -s plugins/testany-eng/scripts/tests -p test_review_boundaries.py -v
python3 plugins/testany-eng/tests/review-boundaries/export_fixture.py RB-001
```

自动检查只保护 fixture 完整性、答案隔离、可解析引用与明显规则冲突；
**不证明模型选择正确、不证明漏审或加料减少**。

最近一轮的实际执行与限制见 [2026-09-08 验证记录](results-2026-09-08.md)。历史结果不代替未来修改后的复测。

## 独立前向测试

1. 主持者导出案例（RB-001 到 RB-008）。导出目录含原始 `request.md`、中性 `entry.md`、
   四个 skill 和其 references/scripts 的快照，排除 tests、grader 和缓存。不修改原仓库。
2. 给未见本说明、预期答案、先前结论或选例理由的独立 reviewer，仅提供导出的 `entry.md` 路径。
   reviewer 只使用导出材料，按问题自己选 skill；主持者不能提前指定“这应是 HLD/LLD”或提示缺陷。
3. 回收完整输出后才对照 `grader/expected.json`。按 `must`/`must_not` 逐项引用实际行为和理由，
   不按标题或关键词命中判正确。相同正确结论允许不同措辞。
4. 最小变更抽样应包括一对相反授权情形（RB-002 与 RB-003）及一个有限工程路由案例
   RB-001；涉及完整性或停止规则的改动，再加入 RB-005/RB-006。全套验收跑 8 个。
5. 记录实际跑过的案例、skill 版本/工作树边界、结论与失败原因。未跑的明确 `NOT_RUN`；
   导出成功与 unittest PASS 不能写作独立行为测试 PASS。

导出器拒绝已有非空目录，不创建 Git 提交，不联网。fixture 内的“已验证”只是案例输入事实，
reviewer 不能将其表述成自己在真实系统执行过的测试。
