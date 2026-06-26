# Skills For Real Engineers — 真正工程师的技能包

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天都在用的 Agent Skills，用来做真正的工程——而不是"vibe coding"。

开发真实的应用程序很难。GSD、BMAD、Spec-Kit 这类方法论试图通过掌控整个流程来帮忙，但这样做剥夺了你的控制权，而且流程里出了 bug 很难排查。

这些 skills 的设计原则是：小巧、容易修改、可组合。它们不挑模型，基于几十年的工程经验。随便改，改成你自己的，用得开心就好。

如果你想跟进这些 skills 的更新，以及我创建的新 skills，可以加入我 newsletter 的约 6 万开发者：

[订阅 Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速上手（30 秒设置）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的 skills，以及要安装到哪些编程 Agent 上。**确保选中 `/setup-matt-pocock-skills`**。

3. 在你的 Agent 中运行 `/setup-matt-pocock-skills`。它会：
   - 询问你使用哪个 Issue 追踪工具（GitHub、Linear 或本地文件）
   - 询问你在分类 Issue 时使用什么标签（`/triage` 会用到）
   - 询问你想把生成的文档保存在哪里

4. 搞定——可以用了。

## 这些 Skills 为什么存在

我创建这些 skills 是为了修复我在 Claude Code、Codex 和其他编程 Agent 中看到的常见失败模式。

### #1：Agent 没按我的想法做

> "没有人能确切地知道自己想要什么。"
>
> —— David Thomas & Andrew Hunt，《程序员修炼之道》

**问题**：软件开发中最常见的失败模式就是认知偏差。你以为开发者知道你想要什么，然后看到他们做出来的东西——才意识到它完全没理解你。

AI 时代也是一样。你和 Agent 之间存在沟通鸿沟。解决办法是 **grilling session**（追问式访谈）——让 Agent 在动手之前，向你追问关于你要构建的东西的详细问题。

**解决办法**：使用

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — 非编码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — 和 `/grill-me` 一样，但多了额外好处（见下文）

这是我最受欢迎的 skills。它们帮助你在开始之前就和 Agent 对齐理解，深入思考你即将做出的变更。**每次**你想做什么改动的时候都用它。

### #2：Agent 太啰嗦了

> "有了统一语言之后，开发者之间的对话以及代码的表达都源自同一个领域模型。"
>
> —— Eric Evans，《领域驱动设计》

**问题**：在项目开始时，开发者和他们为之构建软件的人（领域专家）通常说着不同的语言。

我和我的 Agent 之间也感受到了同样的张力。Agent 通常被丢进一个项目，让它边做边摸索术语。于是它们用了 20 个词来表达 1 个词就能说清的事情。

**解决办法**：共享语言。就是一份文档，帮助 Agent 理解项目中使用的术语。

<details>
<summary>举个例子</summary>

这是来自我的 `course-video-manager` 仓库的一份 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md)。下面哪种说法更容易理解？

- **之前**："课程中某个章节下的某个课时被变成'真实'（即被赋予了文件系统中的位置）时出现了一个问题"
- **之后**："物化级联（materialization cascade）出了个问题"

这种简洁性在一次又一次的对话中都会带来回报。

</details>

这个能力内置在 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它是一场追问式访谈，但同时帮你和 AI 建立共享语言，并用 ADR（架构决策记录）把难以解释的决策文档化。

这个技巧有多强大很难说清楚，它可能是这个仓库里最酷的一个技术。试试就知道。

> [!TIP]
> 共享语言除了减少啰嗦之外还有很多好处：
>
> - **变量、函数和文件命名一致**，都使用共享语言
> - 因此，**代码库对 Agent 来说更容易导航**
> - Agent 还**花更少的 token 在思考上**，因为它有了更精炼的语言

### #3：代码跑不起来

> "始终迈出小而慎重的步伐。反馈的速度就是你的速度上限。绝不要承担太大的任务。"
>
> —— David Thomas & Andrew Hunt，《程序员修炼之道》

**问题**：假设你和 Agent 已经对齐了要构建什么。但如果 Agent *还是*产出了垃圾代码呢？

这时候就该审视你的反馈循环了。如果对自己产出的代码实际运行效果没有任何反馈，Agent 就等于在盲飞。

**解决办法**：你需要那套经典的反馈循环组合：静态类型、浏览器访问、自动化测试。

对于自动化测试，red-green-refactor（红-绿-重构）循环至关重要。Agent 先写一个失败的测试，然后修复代码让测试通过。这给 Agent 提供了一个持续性的反馈，最终产出好得多的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill**，可以插入任何项目。它推行 red-green-refactor，并给 Agent 大量关于什么是好的测试、什么是坏的测试的指导。

对于调试，我还构建了一个 **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)** skill，把最佳调试实践包装成一个简单的循环。

### #4：我们构建了一坨烂泥

> "每天都要投资于系统的设计。"
>
> —— Kent Beck，《解析极限编程》

> "最好的模块是深的。它们允许通过简单的接口访问大量功能。"
>
> —— John Ousterhout，《软件设计的哲学》

**问题**：大多数用 Agent 构建的应用都复杂且难以修改。因为 Agent 可以极大地加速编码，它们也加速了软件的熵增。代码库以前所未有的速度变得越来越复杂。

**解决办法**：对 AI 驱动的开发采取一种激进的新思路：**关心代码的设计**。

这内建在每一层 skills 之中：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前会询问你正在触及哪些模块

而且最重要的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已经变成一坨烂泥的代码库。我建议每隔几天运行一次。

### 总结

软件工程的基本功比以往任何时候都重要。这些 skills 是我将这些基本功浓缩成可重复实践的最大努力，帮助你交付职业生涯中最棒的应用。

## 参考目录

这些 skills 按一个维度划分——**谁能调用它们**。**User-invoked**（用户调用）的 skills 只有在你手动输入时才会被触发（如 `/grill-me`）；它们的职责是编排。**Model-invoked**（模型调用）的 skills 可以被你调用，*也*可以被 Agent 在合适时自动触发；它们承载着可复用的执行纪律。一个 user-invoked skill 可以调用 model-invoked skills，但绝不能调用另一个 user-invoked skill。

### Engineering（工程）

我每天编码工作中使用的 skills。

**用户调用**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)** — 询问哪个 skill 或流程适合你的情况。本质上是本仓库中所有 user-invoked skills 的路由器。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — 追问式访谈，同时构建项目的领域模型，打磨术语并实时更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 将 Issue 按照分类角色状态机进行流转。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 扫描代码库中的架构深化机会，以可视化 HTML 报告呈现，然后对你选中的任何一项进行追问式深入讨论。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 为本仓库配置工程类 skills（Issue 追踪器、分类标签、领域文档布局）。每个仓库在使用其他工程 skills 之前运行一次。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 将任何计划、规格说明或 PRD 拆分成可独立认领的 Issue，使用垂直切片方式。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话转化成 PRD 并发布到 Issue 追踪器。不需要访谈——直接综合你们已经讨论过的内容。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一个一次性原型来充实设计——要么是一个可运行的终端应用（用于状态/业务逻辑问题），要么是一个页面上可切换的多种截然不同的 UI 变体。

**模型调用**

- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** — 针对疑难 bug 和性能退化的规范化诊断循环：复现 → 最小化 → 假设 → 仪器化 → 修复 → 回归测试。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 带 red-green-refactor 循环的测试驱动开发。一次构建一个垂直切片的功能或修复。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** — 主动构建和打磨项目的领域模型——用术语对照表挑战术语，用边缘场景进行压力测试，并实时更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** — 设计深层模块的共享纪律和词汇：用少量接口承载大量行为，放置在清晰的接缝处，通过该接口可测试。

### Productivity（生产力）

通用工作流工具，不限于编码。

**用户调用**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 被 relentlessly（无情地）追问你的计划或设计，直到决策树的每一个分支都被解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩成一份交接文档，让另一个 Agent 可以继续工作。
- **[teach](./skills/productivity/teach/SKILL.md)** — 分多个会话教用户一个新 skill 或概念，将当前目录用作有状态的教学习作空间。
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** — 写好 skill 和编辑 skill 的参考指南：让一个 skill 变得可预测所需的词汇和原则。

**模型调用**

- **[grilling](./skills/productivity/grilling/SKILL.md)** — 无情地追问用户的计划或设计，直到决策树的每一个分支都被解决。这是 `grill-me` 和 `grill-with-docs` 背后可复用的循环。

### Misc（杂项）

保留但很少使用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code hooks 来在危险的 git 命令（push、reset --hard、clean 等）执行之前拦截它们。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件中的 `as` 类型断言迁移为 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建带章节、问题、解答和讲解的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 设置 Husky pre-commit hooks，包含 lint-staged、Prettier、类型检查和测试。
