Git commit 的规范化描述对于团队协作、代码维护、版本回溯、以及自动化生成 Changelog 都至关重要。目前业界最广泛采用的规范之一是 **Conventional Commits (约定式提交)**。下面我将梳理一套基于 Conventional Commits 的规范：

**为什么需要规范的 Git Commit Message？**

* **提高可读性**：清晰的提交历史让人更容易理解代码变更。
* **方便代码审查**：审查者能快速了解每次提交的目的。
* **快速定位变更**：当排查问题或回顾特定功能时，可以快速找到相关提交。
* **自动化工具支持**：许多工具（如语义化版本控制、Changelog 生成器）依赖规范的 commit message。
* **促进团队协作**：统一的规范减少沟通成本。

**Conventional Commits 规范结构**

一个标准的 Conventional Commit Message 包含三个部分：Header (必需)、Body (可选)、Footer (可选)。

```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

**1. Header (必需)**

Header 部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和 `subject`（必需）。

* **`type` (类型)**：用于说明 commit 的类别，只允许使用如下几个固定标识：
    * **`feat`**: 新功能 (feature)
    * **`fix`**: Bug 修复
    * **`docs`**: 文档变更 (documentation)
    * **`style`**: 代码格式调整 (不影响代码逻辑的修改，例如空格、分号、格式化等)
    * **`refactor`**: 代码重构 (既不是新增功能，也不是修复 bug 的代码变动，例如重命名变量、提取方法等)
    * **`perf`**: 性能优化 (improves performance)
    * **`test`**: 测试相关 (增加测试用例、重构测试代码等)
    * **`build`**: 构建系统或外部依赖相关的变更 (例如：gulp, webpack, npm, gradle)
    * **`ci`**: 持续集成配置文件或脚本的变更 (例如：GitHub Actions, Travis, CircleCI)
    * **`chore`**: 其他不修改 `src` 或 `test` 文件的杂项提交 (例如：更新依赖库版本、修改 `.gitignore` 文件)
    * **`revert`**: 撤销之前的某个 commit

* **`scope` (范围/作用域 - 可选)**：用于说明 commit 影响的范围，例如模块名、组件名、文件名等。可以是你项目中的任何一部分。
    * 例如：`feat(parser): add ability to parse arrays`
    * 例如：`fix(login): correct password validation`
    * 如果影响范围不明确或全局性，可以省略 scope。

* **`subject` (主题/简短描述 - 必需)**：
    * **使用祈使句，动词开头，一般现在时**。例如：`Add login page` 而不是 `Added login page` 或 `Adds login page`。
    * **首字母小写** (除非 `scope` 或特定术语本身需要大写)。
    * **结尾不加句号**。
    * **简洁明了**，建议不超过 50 个字符，最多不超过 72 个字符。

**示例 Header:**
```
feat: add user authentication endpoint
fix(ui): resolve button alignment issue on mobile
docs(readme): update installation instructions
refactor(core): simplify data processing logic
```

**2. Body (可选)**

Body 部分是对本次 commit 的详细描述，可以分成多行。

* **与 Header 的 `subject` 之间必须空一行**。
* **详细解释代码变动的动机、上下文和实现思路**。说明“为什么”修改，而不仅仅是“做了什么”（“做了什么”可以通过代码 diff 看出）。
* **每行建议不超过 72 个字符**，以便在各种 Git 工具中更好地显示。
* 可以使用列表（以 `-` 或 `*` 开头）来组织信息。

**示例 Body:**
```
feat(api): implement user profile update functionality

Allows users to update their display name, email address, and profile
picture.

- Added PUT /api/users/profile endpoint.
- Implemented validation for input fields.
- Integrated with image storage service for profile pictures.
```

**3. Footer (可选)**

Footer 部分用于记录一些额外信息，通常是**不兼容的重大变更 (Breaking Changes)** 或**关闭的 Issue 编号**。

* **与 Body 之间必须空一行**。

* **Breaking Changes (重大变更)**：
    * 必须以 `BREAKING CHANGE:` (或 `BREAKING-CHANGE:`) 开头，后面是对变更的描述、理由以及迁移方法。
    * 即使 `type` 不是 `feat` 或 `fix`，如果包含重大变更，也必须在 Footer 中注明。

* **关闭 Issue (Issue Tracking)**：
    * 可以使用特定的关键词来关闭 GitHub、GitLab 等平台上的 Issue。
    * 例如：`Closes #123`, `Fixes #456, #789`, `Resolves #101`。
    * 如果只是关联某个 Issue 但不关闭它，可以使用 `Refs #111`。

**示例 Footer:**
```
BREAKING CHANGE: The `getUser` API endpoint now returns an object
instead of an array. Consumers of this API need to update their
parsing logic.

Closes #42
Fixes #113, #115
```

**完整 Commit Message 示例**

**示例 1: 简单修复**
```
fix(auth): prevent access to admin panel for non-admin users
```

**示例 2: 新功能，包含详细描述和关闭 Issue**
```
feat(cart): add item quantity adjustment in shopping cart

Users can now directly modify the quantity of items in their shopping
cart without removing and re-adding the item.

- Added +/- buttons next to each item in the cart.
- Updated cart total calculation logic.

Closes #235
```

**示例 3: 重构，包含重大变更**
```
refactor(database): switch from MySQL to PostgreSQL for better performance

This change involves a complete overhaul of the database interaction layer.
All existing database schemas and queries have been migrated.

BREAKING CHANGE: All database connection strings and configurations
must be updated to reflect the new PostgreSQL setup.
The `DB_TYPE` environment variable is no longer 'mysql' but 'postgres'.
Data migration scripts need to be run manually.

Refs #301
```

**规范实施建议**

1.  **团队共识**：确保团队所有成员都理解并同意遵守这套规范。
2.  **使用工具辅助**：
    * **Commitizen**: 一个命令行工具，可以引导你完成符合规范的 commit message 编写。
    * **Commitlint**: 用于检查你的 commit message 是否符合规范。通常与 Husky (Git Hooks 工具) 配合使用，在 `commit-msg` 钩子中进行校验，不符合规范则阻止提交。
    * **IDE 插件**: 很多 IDE (如 VS Code) 都有支持 Conventional Commits 的插件。
3.  **从小处着手**：如果团队之前没有规范，可以先从 `type` 和 `subject` 开始，逐步引入 `scope`, `body`, `footer`。
4.  **持续改进**：在实践中根据团队的具体情况进行调整和优化。

遵循这套规范，你的 Git 提交历史将会变得更加清晰、专业，并为后续的自动化流程打下坚实的基础。