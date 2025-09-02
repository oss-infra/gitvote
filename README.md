# GitVote 自动化投票工具

GitVote 是由 CNCF（云原生计算基金会）开发的自动化投票工具，专为开源项目设计，旨在简化和加速项目治理中的投票决策过程。通过集成到 GitHub 工作流中，GitVote 让维护者能够便捷地在 issues 和 pull requests 上举行投票，确保项目决策的透明性和民主性。

更多信息请访问：https://gitvote.dev/

## 主要特性

### 🎯 GitHub 原生集成
- **无缝集成**：直接在 GitHub issues 和 pull requests 中发起投票
- **GitHub App**：通过官方 GitHub 应用的形式提供服务，安全可靠
- **权限管理**：基于 GitHub 的权限体系，确保投票权限的准确控制

### ⚙️ 灵活的配置系统
- **多配置文件**：支持通过 `.gitvote.yml` 文件进行详细配置
- **配置继承**：支持组织级别和仓库级别的配置继承
- **多种配置**：可配置投票时长、通过阈值、投票权限等多个参数

### 🔄 自动化投票流程
- **自动触发**：可根据文件变更模式自动在 PR 上创建投票
- **定期检查**：支持周期性状态检查，自动更新投票进度
- **自动关闭**：达到通过条件后可自动关闭投票

### 📊 实时状态跟踪
- **进度显示**：实时显示投票进度和当前状态
- **结果透明**：清晰展示每个投票者的选择
- **历史记录**：保留完整的投票历史和决策记录

### 📢 结果公告
- **讨论区公告**：支持在 GitHub Discussions 中自动发布投票结果
- **通知机制**：及时通知相关人员投票状态变化

## 安装

### 安装 GitHub App
1. 访问 [GitVote GitHub App](https://github.com/apps/gitvote)
2. 点击 "Install" 按钮
3. 选择要安装的组织或个人账户
4. 选择要授权的仓库：
   - **推荐**：选择 "All repositories" 以便于管理
   - **或者**：手动选择特定仓库
5. 点击 "Install" 完成安装

### 创建配置文件
在仓库根目录创建 `.gitvote.yml` 配置文件：

```yaml
# 配置文件示例
profiles:
  default:
    duration: 2w              # 投票持续时间（2周）
    pass_threshold: 50        # 通过阈值（50%）
    allowed_voters:           # 允许投票的用户和团队
      teams:
        - maintainers
      users:
        - username1
        - username2
    periodic_status_check: "5 days"  # 每5天自动检查状态
    close_on_passing: true           # 达到通过条件时自动关闭
```

### 验证安装
创建一个测试 issue，使用 `/vote` 命令验证 GitVote 是否正常工作。

## 使用方法

### 基本投票命令

#### 发起投票
在 issue 或 pull request 中使用以下命令发起投票：

```
/vote
```

#### 参与投票

投票创建后，有权限的用户可以通过以下方式参与：
- 👍（:+1:）：赞成票
- 👎（:-1:）：反对票

#### 检查投票状态

```
/check-vote
```
手动触发投票状态检查，查看当前投票进度。

#### 关闭投票

```
/close-vote
```

管理员可以手动关闭正在进行的投票。

### 高级配置

#### 自动化投票

启用自动化后，系统会根据 PR 修改的文件自动创建投票：

```yaml
automation:
  enabled: true
  rules:
    - patterns:
        - "*.md"        # Markdown 文件
        - "docs/**"     # 文档目录
      profile: documentation
    - patterns:
        - "src/**"      # 源代码
        - "*.go"
      profile: code-review
```

#### 多配置文件管理

为不同类型的投票创建专门的配置：

```yaml
profiles:
  # 快速决策配置
  quick:
    duration: 3d
    pass_threshold: 66
    close_on_passing: true

  # 重要决策配置
  important:
    duration: 4w
    pass_threshold: 75
    close_on_passing: false
    close_on_passing_min_wait: "1 week"

  # 代码审查配置
  code-review:
    duration: 1w
    pass_threshold: 50
    allowed_voters:
      teams:
        - code-reviewers
```

#### 权限控制
精确控制谁可以参与投票：

```yaml
allowed_voters:
  teams:
    - maintainers      # 维护者团队
    - core-developers  # 核心开发团队
  users:
    - project-lead     # 项目负责人
    - senior-dev       # 高级开发者
  exclude_team_maintainers: false  # 是否排除团队维护者
```

### 最佳实践

#### 1. 投票主题明确
- 在投票描述中清楚说明投票的目的和影响
- 提供充足的背景信息和相关链接
- 设定明确的决策标准

#### 2. 合理设置投票时长
- **简单决策**：3-7 天
- **重要变更**：2-4 周
- **紧急修复**：1-2 天

#### 3. 通过阈值设定
- **一般决策**：50-60%
- **重要变更**：66-75%
- **关键决策**：75-90%

## 注意事项

### ⚠️ 权限管理
- 确保 GitVote App 具有足够的仓库权限
- 定期审查 `allowed_voters` 配置，保持投票权限的时效性
- 避免给予过多用户投票权限，以免影响决策效率

### 📋 配置文件管理
- `.gitvote.yml` 文件的变更会影响后续投票，请谨慎修改
- 建议对配置文件的重要变更也进行投票
- 使用版本控制跟踪配置文件的历史变更

### 🔒 安全考虑
- 投票结果是公开的，避免在投票中包含敏感信息
- 定期检查安装的 GitHub App 权限
- 注意投票权限与实际项目贡献度的匹配

### 📊 投票质量
- 鼓励投票者在投票时提供理由和建议
- 避免频繁发起投票，以免造成"投票疲劳"
- 对于复杂议题，建议先进行讨论再发起投票

### ⏰ 时区考虑
- 投票时长以 UTC 时间计算
- 考虑团队成员的地理分布，设置合理的投票时长
- 重要投票避开节假日期间

## 常见问题

### Q1: 如何修改正在进行的投票配置？
**A**: 正在进行的投票无法修改配置。如需变更，请关闭当前投票并重新发起。配置修改只对新创建的投票生效。

### Q2: 投票权限如何确定？
**A**: 投票权限基于以下优先级：
1. `.gitvote.yml` 中明确指定的 `allowed_voters`
2. 如果未配置，则包含所有仓库协作者
3. 对于组织仓库，包括外部协作者、组织成员等

### Q3: 如何处理投票结果平局？
**A**: 当赞成票比例未达到 `pass_threshold` 时，投票视为未通过。建议在平局情况下：
- 延长讨论时间
- 寻求项目负责人的决定性投票
- 重新评估提案并进行修订

### Q4: 可以撤回已投的票吗？
**A**: 可以。投票者可以通过更改反应（emoji）来修改自己的投票选择，系统会自动更新投票统计。

### Q5: 如何查看历史投票记录？
**A**: 可以通过以下方式查看：
- 在仓库的 issues/PRs 中搜索 "vote:" 标签
- 查看 GitHub Discussions 中的投票结果公告
- 使用 GitHub API 获取详细的投票数据

### Q6: 自动化投票何时触发？
**A**: 当满足以下条件时会自动触发投票：
- `automation.enabled` 设置为 `true`
- PR 修改的文件匹配配置的 patterns
- 使用匹配规则中指定的 profile 创建投票

### Q7: 如何设置组织级别的默认配置？
**A**: 在组织的 `.github` 仓库根目录放置 `.gitvote.yml` 文件，该配置将作为组织内所有仓库的默认配置。

### Q8: 投票通知如何管理？
**A**: GitVote 会在以下情况发送通知：
- 投票创建时通知有权限的投票者
- 定期状态检查时更新进度
- 投票关闭时公布最终结果

### Q9: 如何处理投票中的争议？
**A**: 建议流程：
1. 在投票 issue/PR 中进行公开讨论
2. 如有必要，暂停投票进行更多讨论
3. 项目维护者可以行使最终决定权
4. 考虑修改提案后重新投票

### Q10: GitVote 的使用限制是什么？
**A**: 主要限制包括：
- 每天最多进行一次状态检查
- 需要 GitHub App 权限
- 依赖于 GitHub 的可用性
- 配置文件格式必须正确
