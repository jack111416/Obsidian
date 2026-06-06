# [Aitoearn：OPC（一人公司）的AI内容营销智能体](https://aitoearn.ai/)

https://github.com/yikart/AiToEarn



# OPC-Agents — 一人公司的 AI 执行团队



https://github.com/lulin70/OPC-Agents



# OPC Skills

https://github.com/ReScienceLab/opc-skills

# OPC Team — Cross-Platform Agent Ops Framework



https://github.com/HeiGeAi/opc-team/tree/main



OPC，也就是 One-Person Company，一人公司。

它的核心不是一个人单打独斗，而是一个人借助多个 Agent，像管理一支小团队一样，去完成一个或多个复杂任务。

那么问题来了：

> - 如何构建一套可靠的多 Profile 协作架构？

今天这篇文章，我先带你梳理 [Hermes](https://www.1ai.net/tag/hermes) 多 Profile 协作的核心逻辑。

这不是简单地“多开几个 Agent”，而是要建立一套稳定、清晰、可长期运行的 Agent 工作系统。

每一个想做 OPC 的人，都应该认真思考这个问题。

**一、为什么要使用多 Profile 协作？**

一个 Agent 当然可以完成很多事情。

它可以查资料、写文章、写代码、做图、做复盘。

但如果所有任务都交给一个 Agent，长期看会出现三个问题。

\1. 幻觉问题

一个 Agent 最大的问题是：

> - 自己写，自己审，自己觉得自己没问题。

所以我们需要建立一套团队运作体系，让不同 Profile 承担不同任务，从不同角度看同一个问题。

比如：

- Researcher 负责事实；
- Writer 负责表达；
- Builder 负责实现；
- Coordinator 负责统筹。

这样系统更容易发现漏洞，也更不容易陷入单一 Agent 的自我确认。

\2. 避免记忆污染

一个 Agent 同时做所有项目、所有环节，记忆很容易混在一起。

比如它同时记住：

- 文章要讲故事；
- 代码要优先可运行；
- 产品要先做 MVP；
- 研究要标注来源。

这些经验本身没有错，但它们属于不同任务、不同环节、不同项目。

如果全部混进一个 memory，时间久了就会互相污染、互相矛盾。

最后的结果就是：

- 写代码时带着内容创作的习惯；
- 写文章时带着工程实现的思维；
- 做研究时又提前开始下结论。

这就是记忆污染。

\3. 避免角色混乱

一个 Agent 同时负责研究、规划、写作、执行、审查、复盘，很容易角色混乱。

典型表现是：

- 该研究的时候，它开始写结论；
- 该写作的时候，它又重新查资料；
- 该审查的时候，它开始替自己的输出辩护；
- 该做项目管理的时候，它沉迷于细节执行。

所以，多 Profile 的本质不是“更多 Agent”，而是更清晰的角色边界。

**二、先搞懂四个概念：Profile、Subagent、Project、Wiki**

在搭建团队之前，我们要先分清四个概念：

- Profile
- Subagent
- Project
- Wiki

这四个概念如果混在一起，后面的系统一定会乱。

\1. Profile：长期员工

Profile 可以理解成系统中的长期员工。

每个 Profile 都是一个独立 Agent，拥有自己的身份、记忆、技能和运行配置。

比如：

- Coordinator 是协调员；
- Researcher 是研究员；
- Writer 是作家；
- Builder 是构建者。

它们不是同一个 Agent 的不同聊天窗口，而是不同角色、不同职责、不同记忆边界的长期成员。

所以，多 Profile 实质上是在搭建一支 Agent 团队。

\2. Subagent：临时工

Subagent 是临时 Agent。

它更像是临时派出去的小助手，适合处理复杂任务中的局部问题。

比如你要写一篇深度文章，可以临时派出几个 Subagent：

- 一个查传统 RAG；
- 一个查 Agentic RAG；
- 一个查 LLM Wiki；
- 一个检查逻辑漏洞。

Subagent 干完就结束，不需要长期人格，也不需要长期记忆。

所以可以简单理解为：

> - Profile 是长期员工。Subagent 是临时外包。

\3. Project：项目空间

Project 是长期任务的项目空间。

如果你同时推进多个长期任务，比如：

- Twitter Growth；
- Vibe Coding；
- 内容增长系统；
- 产品 Demo。

那每个长期任务都应该有自己的项目空间。

注意：

> - 运行多个长期任务时，关键不是多建 Profile，而是先划分好项目空间。

不要为每个项目复制一套 Profile。

正确做法是：

> - 同一套 Profile 团队，服务多个 Project folder。

这样才能避免 Profile 数量爆炸，也能避免长期记忆污染。

\4. Wiki：共享记忆层

多 Profile 的记忆不相通，那复杂任务怎么推进？

答案是：

> - 共享 Wiki。

Wiki 就像一家公司的共享文档。

不同员工有不同的大脑，但他们可以通过共享文档同步：

- 项目进度；
- 任务状态；
- 决策记录；
- 知识沉淀。

在 Hermes 多 Profile 系统里，Wiki 不是普通笔记库，而是一个有组织、可维护、可长期运行的共享记忆系统。

它记录：

- 项目状态；
- 任务进度；
- 决策记录；
- 研究材料；
- 最终产出；
- 通用方法论。

所以，一个稳定的 Agent 团队，实际上是由多 Profile 和共享 Wiki 共同组成的。

**三、四角色模型：像真实公司一样组织 Agent**

多 Profile 该如何分工？

我推荐一个经过验证的四角色模型：

- Coordinator
- Researcher
- Writer
- Builder

也可以理解成：

- 项目经理
- 研究员
- 作家
- 工程师

角色 1：协调员 Coordinator

Coordinator 就是项目经理。

它的核心职责是：

1. 定义目标：明确最终要达成什么。
2. 拆分任务：把大目标拆成可执行的小任务。
3. 路由任务：判断每个任务应该交给谁。
4. 汇总结果：把不同角色的产出整合成最终结果。
5. 检查边界：避免记忆污染和文件冲突。

Coordinator 不应该沉迷于亲自查资料、写文章、写代码。

它最重要的职责是：

> - 让整个系统有序运行。

角色 2：研究专员 Researcher

Researcher 就是研究员。

它的核心职责是：

1. 收集证据：从多个来源获取信息。
2. 对比来源：交叉验证，确保信息可靠。
3. 标记不确定性：明确指出哪些信息尚未验证。
4. 提炼事实：区分事实、观点和推测。

一个好的 Researcher，可以显著降低整个系统的幻觉。

它不应该直接写最终稿，也不应该直接做最终决策。

它要做的是提供可靠的原材料。

角色 3：作家 Writer

Writer 负责把原材料转化成清晰内容。

它的核心职责是：

1. 搭建文章结构。
2. 优化表达方式。
3. 提炼主线和观点。
4. 让内容适合目标读者。
5. 把复杂概念讲清楚。

当 Writer 不需要再负责规划和查资料，它的输出质量会明显提升。

因为它可以专注于一件事：

> - 把内容讲清楚。

角色 4：构建者 Builder

Builder 更像工程师。

它的核心职责是：

1. 实现：把计划变成可运行的代码、页面或系统。
2. 调试：定位并修复问题。
3. 测试：确保输出稳定可靠。
4. 交付：生成最终可用的结果。

当 Builder 不需要再负责讲故事、做研究、定方向时，它的实现质量也会提高。

因为它可以专注于落地。

**四、完整工作流**

一套典型的多 Profile 工作流是：

1. Coordinator 拆解并规划任务；
2. Researcher 收集来源、验证主张；
3. Writer 把研究结果转化成清晰内容；
4. Builder 负责最终实现或交付；
5. Coordinator 最后检查、汇总、归档。

这个结构之所以强大，是因为它反映了真实工作流程。

现实里，一个稳定团队也不会让同一个人同时当项目经理、研究员、作家、工程师和审稿人。

Agent 团队也是一样。

**五、开始构建每个 Profile**

当角色分工确定后，就可以开始构建每个 Profile。

每个 Profile 通常包含：

soul.mdUSER.mdmemory.mdconfig.yamlskills/.env

看到这么多东西，很多人会头大。

接下来，我以“协调员 Coordinator”为例，讲一下每个文件分别负责什么。

\1. soul.md：协调员是谁

soul.md 是 Profile 的核心身份文件。

它定义这个 Profile：

- 是谁；
- 负责什么；
- 有什么边界；
- 不应该做什么。

比如 Coordinator 的 soul.md 需要说明：

- 它是协调员；
- 它负责拆分任务、规划项目、汇总结果；
- 它维护 dashboard 和 agent-log；
- 它不直接执行研究任务；
- 它不直接写最终内容；
- 它不直接实现代码。

注意：

> - 如果你同时推进多个项目，不要把具体项目内容写进 soul.md。

否则极容易造成角色污染。

soul.md 写的是：

> - 这个 Agent 是谁。

不是：

> - 这个项目现在在做什么。

\2. USER.md：协调员理解的用户是谁

USER.md 记录的是这个 Profile 对用户的理解。

比如：

- 用户偏好中文交流；
- 用户喜欢结构清晰的 Markdown；
- 用户不喜欢空泛概念；
- 用户希望输出适合复制到 Obsidian。

注意：

这里的 USER.md 是“协调员这个角色所理解的用户形象”，不是整个系统唯一的用户画像。

如果需要所有 Profile 共享用户画像，应该放到 Wiki 的：

system/user-profile.md

\3. memory.md：协调员学到的通用经验

memory.md 记录的是这个 Profile 在长期工作中总结出来的通用经验。

比如：

- 复杂任务应该先拆解；
- 不同 Profile 不要同时修改同一个正式文件；
- 中间材料先进入 inbox；
- 最终产出再进入 outputs。

但这里不应该写具体项目经验。

比如：

- Twitter 今天写了 3 条推文；
- Vibe Coding 登录页已经完成。

这些内容不属于 memory.md，而应该放到项目空间里。

memory.md 记录的是：

> - 通用经验，不是项目状态。

\4. skills：协调员的技能库

skills 是可复用的任务流程。

Coordinator 的 skills 可以包括：

- 任务拆解；
- 项目优先级判断；
- 交接单生成；
- dashboard 更新；
- weekly review；
- memory audit。

这些 skills 专门服务于协调员这个角色。

如果某个 skill 是通用技能，比如：

- project-context-loader；
- memory-routing。

可以分别加入多个 Profile 的 skills list。

但不要把所有技能都塞给所有 Profile。

否则角色边界又会乱掉。

\5. config.yaml：协调员如何运行

config.yaml 是 Profile 的运行配置。

它规定这个 Profile 如何工作，比如：

- 使用什么模型；
- 默认工作目录是什么；
- 允许读写哪些文件；
- 是否自动加载 skills；
- 哪些文件不能修改。

注意：

> - config.yaml 不是写任务内容的地方。

它回答的是：

> - 这个 Profile 怎么运行？

不是：

> - 这个项目要做什么？

\6. .env：协调员的密钥

.env 只放密钥。

比如：

- API Key；
- Token；
- SMTP 密码；
- 外部服务凭证。

不要把这些内容写进 .env：

- 项目状态；
- 用户偏好；
- 任务说明；
- 文章内容。

也不要把密钥写进：

- soul.md
- USER.md
- memory.md
- AGENTS.md
- Wiki

因为这些文件可能会进入模型上下文。

上篇小结

到这里，多 Profile 的基本搭建就告一段落了。

我们做了三件事：

1. 明确为什么不能让一个 Agent 全包；
2. 区分 Profile、Subagent、Project、Wiki 四个概念；
3. 搭建 Coordinator、Researcher、Writer、Builder 四角色模型。

但这还只是“团队层”。

一个团队要长期运行，光有员工还不够。

它还需要：

- 共享文档；
- 项目空间；
- 任务看板；
- 决策记录；
- 复盘机制。

也就是下一篇要讲的重点：

> - [如何用 Wiki 构建你的 OPC 共享记忆系统](https://www.1ai.net/52617.html)。

### 相关内容：

1. [微信接入Hermes教程，小白也能上手的实操手册：零基础教学指南](https://www.1ai.net/52444.html)
2. [Hermes教程：把Hermes打造成7×24小时在线的AI智能体](https://www.1ai.net/52607.html)
3. [OpenClaw 龙虾和Hermes，普通人怎么选](https://www.1ai.net/52442.html)
4. [快手可图大模型KOLORS的使用方法，实测快手AI产品可图KOLORS功能](https://www.1ai.net/12105.html)



上一篇我们讲了 [Hermes高级用法：用多 Profile 协作 + Wiki 共享记忆，搭建你的 OPC Agent 团队（上篇）](https://www.1ai.net/52615.html)。

核心观点是：

> - 不要让一个 Agent 同时承担所有角色。

更稳的方式是，把 Agent 组织成一支小团队：

- Coordinator 负责协调；
- Researcher 负责研究；
- Writer 负责表达；
- Builder 负责构建。

但这会带来一个新问题：

> - 多 Profile 之间记忆不相通，那它们怎么长期协作？

答案是：

> - 用 Wiki 作为共享记忆层。

一、Wiki 是什么？

在这套系统里，Wiki 不是普通笔记库。

它是整个 [OPC](https://www.1ai.net/tag/opc) 系统的：

- 共享知识库；
- 项目管理层；
- 长期记忆层。

它管理三类东西：

- 知识；
- 资料；
- 产出。

更具体地说，Wiki 负责记录：

- 项目背景；
- 任务状态；
- 推进日志；
- 重要决策；
- 中间材料；
- 最终产出；
- 跨项目方法论；
- 原始资料。

可以把它理解成一家公司的共享文档系统。

在这个系统里：

- Profile 是员工；
- Project 是项目办公室；
- Wiki 是公司资料库；
- Coordinator 是总控。

这样，多个 Profile 即使记忆不相通，也能通过 Wiki 协同工作。

二、Wiki 的整体结构

一个完整的 Wiki，建议包含 8 个部分：

index.mdschema.mdsystem/projects/pages/raw/assets/archive/

Pasted image 20260428214948.png这几个部分看起来多，但每一层都有明确作用。

它们共同解决的是一个核心问题：

> - 信息到底应该放在哪里？

如果不分层，所有东西都会混在一起。

比如：

- 项目状态会污染用户画像；
- 临时想法会污染长期知识；
- 角色经验会污染项目规则；
- 原始资料会和最终结论混在一起。

所以 Wiki 的第一原则是：

> - 分层，就是防污染。

三、index.md：Wiki 入口

index.md 是整个 Wiki 的首页。

它不存具体内容，只做导航。

它告诉人和 Agent：

- 系统区在哪里；
- 当前有哪些项目；
- 通用知识在哪里；
- 原始资料在哪里；
- 最近重点是什么。

index.md 的作用是让任何 Profile 一进入 Wiki，就知道该从哪里开始读。

它是地图，不是仓库。

四、schema.md：Wiki 规范

schema.md 是 Wiki 的宪法。

它规定：

- 文件怎么命名；
- 内容写到哪里；
- 哪些文件只读；
- 哪些文件可以改；
- 什么信息不能乱写；
- 页面状态如何标记。

有了 schema.md，整个 Wiki 才能长期稳定运行。

否则每个 Profile 都按自己的理解写文件，很快就会乱。

比如，schema.md 可以规定：

- Raw 只读不改；
- Project 文件只记录对应项目的信息；
- Pages 只记录跨项目可复用的方法论；
- System 只记录全局管理信息；
- 不确定的信息先进入 inbox，不直接进入 pages。

这就是规则层。

五、system：全局管理层

system 是 Wiki 的全局管理区。

它不是某个具体项目，而是整个系统的管理中枢。

system 里建议包含 6 个核心文件：

dashboard.mdagent-log.mdweekly-review.mdmemory-routing.mdskill-registry.mduser-profile.md

\1. dashboard.md：总控面板

dashboard.md 记录所有项目的当前状态。

它回答：

- 现在有哪些项目？
- 每个项目进展如何？
- 下一步做什么？
- 谁负责？
- 有没有阻塞？

dashboard.md 通常只应该由 Coordinator 更新。

其他 Profile 可以读取，但不要随便修改。

这样可以避免多个角色同时改总控面板，造成冲突。

\2. agent-log.md：全局 Agent 行为日志

agent-log.md 记录所有 Profile 做过什么。

比如：

- Researcher 查了什么资料；
- Writer 写了什么草稿；
- Builder 交付了什么文件；
- Coordinator 更新了哪些状态。

它的作用是让多 Profile 的行为可追踪。

当系统运行久了，你可以回头看到：

- 谁在什么时候做了什么；
- 产出在哪里；
- 任务是否完成。

这对长期 OPC 系统非常关键。

\3. weekly-review.md：周期复盘

weekly-review.md 不是单纯记录流水账。

它的作用是把一周的行动转化成策略。

它应该总结：

- 本周完成了什么；
- 哪些地方卡住了；
- 哪些经验可以复用；
- 哪些决策需要调整；
- 下周重点是什么。

真正有价值的 OPC 系统，不只是执行任务，而是要能从任务中沉淀方法。

weekly-review.md 就是做这件事的地方。

\4. memory-routing.md：记忆写入规则

memory-routing.md 是防污染的核心文件。

它规定不同信息应该写到哪里。

比如：

- 角色身份写入 soul.md；
- 用户长期偏好写入 USER.md 或 system/user-profile.md；
- 角色通用经验写入 memory.md；
- 项目规则写入 AGENTS.md；
- 项目背景写入 context.md；
- 项目任务写入 tasks.md；
- 项目过程写入 log.md；
- 项目决策写入 decisions.md；
- 临时材料写入 inbox/；
- 正式产出写入 outputs/；
- 跨项目方法论写入 pages/；
- 原始资料写入 raw/。

这套规则决定了系统能不能长期稳定运行。

\5. skill-registry.md：技能清单

skill-registry.md 记录整个系统有哪些可复用 skills，以及它们应该分配给谁。

它可以防止一个问题：

> - 所有 Profile 都装一堆技能，最后角色边界再次混乱。

比如：

- dashboard-update 只应该给 Coordinator；
- source-validation 应该给 Researcher；
- article-structure 应该给 Writer；
- code-builder 应该给 Builder。

高风险技能，比如：

- delete-files
- deploy-production
- bulk-memory-edit

需要严格限制。

skill-registry.md 的作用，就是让能力分配可控。

\6. user-profile.md：统一用户画像

user-profile.md 记录所有 Profile 都应该知道的长期用户偏好。

比如：

- 用户偏好中文交流；
- 用户喜欢直白、清晰、有判断力的解释；
- 用户常用 Obsidian；
- 用户喜欢 Markdown；
- 用户长期关注 [Hermes](https://www.1ai.net/tag/hermes)、LLM Wiki、OPC、多 Agent 协作。

这和每个 Profile 自己的 USER.md 不一样。

Profile 里的 USER.md 是单个角色对用户的理解。

system/user-profile.md 是整个系统共享的用户画像源头。

六、projects：长期项目层

system 负责全局管理。

projects 负责具体项目。

每个长期项目都应该有自己的项目空间。

比如：

projects/twitter-growth/projects/vibe-coding/

每个项目空间建议包含：

AGENTS.mdcontext.mdtasks.mdlog.mddecisions.mdinbox/outputs/

\1. AGENTS.md：项目规则

AGENTS.md 说明在这个项目下，Agent 应该如何协作。

它记录：

- 项目目标；
- 项目定位；
- 工作规则；
- 文件写入边界；
- 禁止事项。

比如 Twitter Growth 项目的 AGENTS.md，可以规定：

- 追热点时必须判断是否符合账号长期定位；
- Researcher 的中间材料写入 inbox/；
- Writer 的结构方案写入 inbox/；
- Builder 的正式产出写入 outputs/；
- Coordinator 负责更新 tasks.md 和 log.md。

AGENTS.md 回答的是：

> - 这个项目应该怎么做？

\2. context.md：项目背景

context.md 是项目说明书。

它回答：

- 这个项目是什么？
- 为什么做？
- 目标是什么？
- 当前阶段是什么？
- 核心约束是什么？

任何 Profile 进入项目之前，都应该先读 context.md。

否则它不知道自己正在参与什么项目。

\3. tasks.md：项目任务池

tasks.md 记录当前项目的任务状态。

它通常分为：

- Doing
- Todo
- Done

每个任务最好写清楚：

- 任务编号；
- 负责人；
- 当前状态；
- 依赖关系；
- 输出路径。

这样 Coordinator 才能管理项目进度，不会让任务散落在聊天记录里。

\4. log.md：项目推进记录

log.md 是单个项目的推进日志。

它记录这个项目每天或每次迭代发生了什么。

比如：

- Researcher 完成了热点研究；
- Writer 完成了文章大纲；
- Builder 完成了登录页草稿；
- Coordinator 更新了任务状态。

注意：

project log 和 agent-log 不一样。

- project log 记录某个项目的推进过程；
- agent-log 记录所有 Profile 的全局行为。

\5. decisions.md：项目决策

decisions.md 记录项目已经确定的方向。

它的作用是防止系统反复摇摆。

比如：

- 内容定位是什么；
- 产品 MVP 范围是什么；
- 技术栈选什么；
- 哪些方向已经明确不做。

长期项目最怕每次对话都重新决策。

decisions.md 就是为了避免这个问题。

\6. inbox：中间材料

inbox/ 放半成品、草稿、研究材料和未确认结论。

比如：

- 热点研究；
- 竞品笔记；
- 大纲草稿；
- 粗糙想法；
- Subagent 输出。

这些东西还没有被确认，所以不能直接进入 pages/ 或 outputs/。

inbox/ 的作用是承接不确定性。

\7. outputs：正式产出

outputs/ 放已经确认可以交付或使用的内容。

比如：

- 最终文章；
- 推文草稿；
- 代码结果；
- 产品文案；
- Demo 文件。

可以简单理解为：

> - inbox 是中间材料。outputs 是正式产出。

七、pages：通用知识层

pages/ 放跨项目可复用的方法论。

比如：

research-methods.mdwriting-methods.mdbuilding-methods.mdcoordination-methods.mdcontent-growth-system.mdproduct-building-system.md

什么内容可以进入 pages/？

必须满足三个条件：

1. 跨项目可复用；
2. 不是临时结论；
3. 经过验证或抽象。

比如：

> - 内容项目不能只追热点，还要判断热点是否符合长期定位。

这可以进入 pages/。

但：

> - 今天某个 AI Agent 话题很火，可以写一条推文。

这不能进入 pages/。

它应该进入某个具体项目的 inbox/。

pages/ 是知识层，不是临时笔记层。

八、raw：原始资料层

raw/ 放原始资料。

比如：

- 论文；
- 文章；
- 网页快照；
- 数据表；
- 会议记录。

raw/ 的原则是：

> - 只读不改。

它是事实来源，不是加工层。

研究材料可以从 raw/ 中提取，但不要直接改 raw/。

否则你会失去原始证据。

九、assets：素材附件层

assets/ 放非文本素材。

比如：

- 图片；
- 架构图；
- 截图；
- 封面图；
- 图表。

如果文章需要引用图片，就放到：

assets/images/

如果是系统架构图，就放到：

assets/diagrams/

如果是操作截图，就放到：

assets/screenshots/

十、archive：归档层

archive/ 放不活跃、过期或废弃的内容。

比如：

- 旧项目；
- 旧草稿；
- 废弃方案；
- 过期资料。

不要轻易删除重要内容。

先归档。

长期系统不是靠“删干净”维持秩序，而是靠“分层和归档”维持秩序。

十一、Profile 层和 Wiki 层的边界

到这里，我们可以清楚看到：

Profile 层和 Wiki 层有明显边界。

Profile 负责：

- Agent 是谁；
- Agent 怎么运行；
- Agent 有什么经验；
- Agent 有什么技能。

Wiki 负责：

- 所有 Agent 的项目；
- 共享知识；
- 原始资料；
- 任务状态；
- 决策记录；
- 最终产出。

一句话：

> - Profile 是员工。Wiki 是公司系统。

如果你把项目状态写进 Profile，记忆会污染。

如果你把角色经验写进项目，知识会分散。

如果你把临时材料写进 pages，长期知识会变脏。

所以，分层不是形式主义。

> - 分层就是系统稳定性的来源。

十二、实际使用时怎么运转？

讲到这里，很多人会有一个现实疑问：

> - 这套架构听起来很好，但日常用起来会不会很麻烦？

主要有两个问题：

1. 多 Profile 是不是要频繁手动切换？
2. 多 Profile 协作会不会很耗 Token？

先说第一个。

多 Profile 确实需要切换，但不是乱切。

判断标准很简单：

> - 换项目，不一定换 Profile。换角色，才需要换 Profile。

比如 Researcher 上午研究 Twitter 热点，下午研究 Vibe Coding 竞品，这两个都是研究任务，不一定要换 Profile，只需要切换 Project context。

但如果从“查资料”变成“写最终稿”，那就应该从 Researcher 切到 Writer。

所以，多 Profile 协作的关键不是频繁切窗口，而是清楚知道：

> - 当前任务该交给哪个角色。

这时，Web UI 的作用就很明显了。

如果全部靠终端来切 Profile，使用成本会比较高。

而 Web UI 更像一个控制台，你可以在里面更方便地切换：

- Coordinator
- Researcher
- Writer
- Builder

终端更适合搭系统、改配置、调试路径。

Web UI 更适合日常协作、切换 Profile、继续会话。

可以简单理解为：

> - 终端是施工现场。Web UI 是办公室。Wiki 是公司文档。

第二个问题是 Token 成本。

多 Profile 协作一定会比单 Agent 更耗 Token。

因为每个 Profile 都需要读取自己的身份、项目上下文和 Wiki 资料。

解决方式不是放弃多 Profile，而是做分层：

- 主模型负责复杂推理；
- 副模型负责总结、整理、归档；
- 简单任务可以交给本地模型；
- Wiki 负责保存长期上下文，避免每次重复粘贴。

也就是说：

> - 不要让模型记住所有东西。要让模型按需读取正确的信息。

这样，多 Profile + Wiki 才能真正跑得起来，而不是变成一个高成本玩具。

十三、这一篇的核心结论

Hermes 的高级用法，不是多开几个 Agent，而是用多 Profile 做角色分工，用 Wiki 做共享记忆。

这套系统的目标不是复杂，而是让一个人也能管理一支稳定协作的 Agent 团队。

十四、下一步我要做什么？

后续，我们来：

- 搭建一个体系；
- 用 Web UI 管理多个 Profile；
- 用主副模型和本地模型节省 Token。