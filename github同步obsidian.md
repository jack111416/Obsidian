你需要先**切换到你的 Obsidian 笔记库根目录**，然后再执行 Git 命令。

## ✅ 解决步骤

### 1️⃣ 找到你的 Obsidian 笔记库文件夹

打开 Obsidian，随便点开一个笔记，看窗口最顶部——通常会显示当前笔记的路径，例如：

text

D:\我的笔记\Obsidian库\某个笔记.md

那么你的笔记库根目录就是 `D:\我的笔记\Obsidian库`。

### 2️⃣ 在终端里切换到该目录

在 PowerShell 中执行：

bash

cd "D:\我的笔记\Obsidian库"   # 把路径换成你的实际路径

或者直接在 Windows 文件管理器里打开那个文件夹，然后在地址栏输入 `powershell` 并回车，这样就会自动在该目录打开终端。

### 3️⃣ 初始化本地 Git 仓库

bash

git init

执行后应该会显示 `Initialized empty Git repository in ...`

### 4️⃣ 添加远程仓库地址

bash

git remote add origin git@github.com:jack111416/Obsidian.git

### 5️⃣ 完成第一次提交

bash

git add .
git commit -m "first commit"
git push -u origin main

（如果你的默认分支是 `master`，就把 `main` 换成 `master`）

### 1️⃣ 手动删除 origin 再重新添加 SSH 地址

在终端中依次执行：

bash

git remote remove origin
git remote add origin git@github.com:jack111416/Obsidian.git
git remote -v）
## ✅ 执行以下命令（在同一个终端中
### 1️⃣ 取消代理

bash

git config --global --unset http.proxy
git config --global --unset https.proxy

### 2️⃣ 尝试推送

bash

git push -u origin master

如果提示 `master` 分支不存在，试试 `main`：

bash

git push -u origin main

### 3️⃣ 如果推送被拒绝（远程已有内容）

可能需要先拉取合并：

bash

git pull origin master --allow-unrelated-histories

然后再次推送：

bash

git push origin master