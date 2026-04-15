# 检索索引：本文档收录【Git】【基础入门】知识点，内容100%来自Git官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Git 是一个免费开源的分布式版本控制系统，旨在快速高效地处理从小型到非常大型的项目的所有内容。Git 易于学习，占地小，性能极快。它通过廉价的本地分支、便捷的暂存区域和多个工作流等功能，超越了 SCM（软件配置管理）工具，如 Subversion、CVS、Perforce 和 ClearCase。

Git 的核心特性：
1. **分布式版本控制**：每个开发者都有完整的代码历史
2. **高性能**：本地操作快速，无需联网
3. **强大的分支管理**：分支创建和切换廉价高效
4. **数据完整性**：基于 SHA-1 哈希，保证数据不被篡改
5. **灵活的工作流**：支持多种开发工作流程
6. **免费开源**：完全免费，社区活跃

## 2. 核心知识点（结构化层级）

### 2.1 Git 核心概念
1. 仓库（Repository）
2. 提交（Commit）
3. 分支（Branch）
4. 标签（Tag）
5. 工作区（Working Directory）
6. 暂存区（Staging Area / Index）
7. HEAD 指针
8. 远程仓库（Remote）

### 2.2 Git 安装配置
1. Git 安装（Windows、macOS、Linux）
2. 全局用户配置
3. 文本编辑器配置
4. 换行符配置
5. SSH 密钥配置
6. 代理配置
7. 配置查看与编辑

### 2.3 Git 工作流程
1. 初始化仓库
2. 克隆仓库
3. 文件状态（未修改、已修改、已暂存、已提交）
4. 查看状态
5. 添加到暂存区
6. 提交变更
7. 推送到远程
8. 从远程拉取

### 2.4 Git 分支管理
1. 创建分支
2. 切换分支
3. 合并分支
4. 删除分支
5. 分支重命名
6. 查看分支
7. 分支工作流（Git Flow、GitHub Flow、Trunk-Based）

### 2.5 Git 提交规范
1. 提交信息格式
2. Conventional Commits 规范
3. 提交类型（feat、fix、docs、style、refactor、test、chore）
4. 提交范围
5. 提交描述
6. 提交正文
7. 页脚（BREAKING CHANGE、Closes）

### 2.6 远程仓库操作
1. 添加远程仓库
2. 查看远程仓库
3. 重命名远程仓库
4. 删除远程仓库
5. 推送到远程
6. 从远程拉取
7. 从远程获取
8. 远程分支操作

### 2.7 Git 历史查看
1. 查看提交历史
2. 查看提交详情
3. 查看分支历史
4. 查看文件历史
5. 图形化历史
6. 搜索提交历史
7. 比较差异

### 2.8 Git 撤销操作
1. 撤销工作区修改
2. 撤销暂存区修改
3. 撤销提交
4. 修改最后一次提交
5. 重置到指定提交
6. 变基操作
7. 贮藏（Stash）

### 2.9 Git 冲突解决
1. 合并冲突原因
2. 查看冲突文件
3. 手动解决冲突
4. 标记冲突已解决
5. 完成合并
6. 合并冲突最佳实践
7. 使用工具解决冲突

### 2.10 Git 标签管理
1. 创建标签
2. 查看标签
3. 推送标签
4. 删除标签
5. 检出标签
6. 轻量标签与附注标签
7. 版本号规范

### 2.11 Git 常用命令速查
1. 基础命令
2. 分支命令
3. 远程命令
4. 历史命令
5. 撤销命令
6. 其他实用命令

### 2.12 Git 最佳实践
1. 提交粒度
2. 提交信息规范
3. 分支管理策略
4. 代码审查
5. 仓库大小控制
6. 敏感信息处理
7. Gitignore 使用

## 3. 官方标准语法 / API

### Git 配置命令
```bash
# 查看配置
git config --list
git config --global --list
git config --local --list

# 用户配置
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 编辑器配置
git config --global core.editor "vim"
git config --global core.editor "code --wait"

# 查看配置项
git config user.name
git config --global user.email

# 编辑配置文件
git config --global --edit

# 换行符配置（Windows）
git config --global core.autocrlf true

# 换行符配置（macOS/Linux）
git config --global core.autocrlf input

# 别名配置
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"

# 颜色配置
git config --global color.ui true

# 查看帮助
git help config
git config --help
```

### Git 基础工作流命令
```bash
# 初始化仓库
git init
git init myproject

# 克隆仓库
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git myrepo
git clone --depth 1 https://github.com/user/repo.git  # 浅克隆

# 查看状态
git status
git status -s  # 简短输出

# 添加到暂存区
git add file.txt
git add file1.txt file2.txt
git add *.txt
git add .  # 添加所有文件
git add -u  # 添加已跟踪的文件
git add -p  # 交互式添加

# 提交
git commit
git commit -m "commit message"
git commit -a -m "commit message"  # 跳过暂存区直接提交
git commit --amend  # 修改最后一次提交
git commit --amend --no-edit  # 修改最后一次提交但不修改信息

# 查看差异
git diff
git diff --staged
git diff HEAD
git diff file.txt
git diff commit1 commit2
git diff branch1 branch2

# 查看历史
git log
git log -n 10  # 最近10条
git log --oneline  # 一行显示
git log --stat  # 显示统计信息
git log --patch  # 显示差异
git log --author="name"  # 按作者
git log --grep="keyword"  # 按关键词
git log --since="2024-01-01"  # 按日期
git log --graph --oneline --all  # 图形化显示

# 查看提交详情
git show
git show HEAD
git show commit-hash
git show commit-hash:file.txt
```

### Git 分支管理命令
```bash
# 查看分支
git branch
git branch -v  # 显示最后一次提交
git branch -a  # 显示所有分支（包括远程）
git branch -r  # 显示远程分支

# 创建分支
git branch newbranch
git branch newbranch commit-hash

# 切换分支
git checkout newbranch
git switch newbranch  # Git 2.23+
git checkout -b newbranch  # 创建并切换
git switch -c newbranch  # Git 2.23+

# 合并分支
git merge branchname
git merge --no-ff branchname  # 不使用快进合并
git merge --squash branchname  # 压缩合并
git merge --abort  # 中止合并

# 删除分支
git branch -d branchname  # 删除已合并的分支
git branch -D branchname  # 强制删除分支

# 重命名分支
git branch -m oldname newname
git branch -M oldname newname  # 强制重命名

# 查看分支合并情况
git branch --merged
git branch --no-merged
```

### Git 远程仓库命令
```bash
# 查看远程仓库
git remote
git remote -v

# 添加远程仓库
git remote add origin https://github.com/user/repo.git

# 重命名远程仓库
git remote rename oldname newname

# 删除远程仓库
git remote remove name

# 修改远程仓库 URL
git remote set-url origin https://github.com/user/newrepo.git

# 推送到远程
git push origin main
git push -u origin main  # 设置上游分支
git push origin --all  # 推送所有分支
git push origin --tags  # 推送所有标签
git push -f origin main  # 强制推送（谨慎使用）

# 从远程拉取
git pull
git pull origin main
git pull --rebase  # 使用变基

# 从远程获取（不合并）
git fetch
git fetch origin
git fetch --all
git fetch --prune  # 删除已删除的远程分支引用

# 查看远程分支
git branch -r
git ls-remote

# 创建跟踪远程分支的本地分支
git checkout -b localbranch origin/remotebranch
git switch -c localbranch origin/remotebranch
git checkout --track origin/remotebranch

# 设置上游分支
git branch -u origin/main
git branch --set-upstream-to=origin/main
```

### Git 撤销与重置命令
```bash
# 撤销工作区修改
git checkout -- file.txt
git restore file.txt  # Git 2.23+

# 撤销暂存区修改
git reset HEAD file.txt
git restore --staged file.txt  # Git 2.23+

# 撤销提交（保留修改）
git reset --soft HEAD~1

# 撤销提交（不保留修改，谨慎使用）
git reset --hard HEAD~1

# 撤销提交（保留修改在工作区）
git reset --mixed HEAD~1

# 修改最后一次提交
git commit --amend
git commit --amend -m "new message"
git commit --amend --no-edit

# 查看重置引用日志
git reflog

# 恢复到指定提交
git reset --hard commit-hash

# 变基
git rebase main
git rebase -i HEAD~3  # 交互式变基
git rebase --abort  # 中止变基
git rebase --continue  # 继续变基

# 贮藏
git stash
git stash save "message"
git stash list
git stash show
git stash apply
git stash pop
git stash drop
git stash clear
```

### Git 标签命令
```bash
# 查看标签
git tag
git tag -l "v1.*"
git tag -n

# 创建轻量标签
git tag v1.0.0

# 创建附注标签
git tag -a v1.0.0 -m "version 1.0.0"

# 查看标签详情
git show v1.0.0

# 推送标签
git push origin v1.0.0
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0

# 检出标签
git checkout v1.0.0
git checkout -b newbranch v1.0.0
```

### Git 冲突解决命令
```bash
# 合并时发生冲突
git merge feature

# 查看冲突文件
git status

# 查看冲突内容
git diff

# 手动编辑冲突文件后，标记已解决
git add file.txt

# 完成合并
git commit

# 中止合并
git merge --abort

# 使用合并工具
git mergetool

# 查看合并历史
git log --graph --oneline --all
```

### Gitignore 配置
```gitignore
# 依赖
node_modules/
vendor/
.venv/
env/

# 构建输出
dist/
build/
target/
*.class

# 日志
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# IDE
.idea/
.vscode/
*.swp
*.swo

# 操作系统
.DS_Store
Thumbs.db

# 环境变量
.env
.env.local
.env.*.local

# 临时文件
*.tmp
*.temp
.cache/
```

## 4. 官方示例代码（可运行）

### 完整的 Git 工作流程示例
```bash
# ========== 1. 配置 Git ==========
git config --global user.name "Zhang San"
git config --global user.email "zhangsan@example.com"
git config --global core.editor "vim"
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"

# 查看配置
git config --list

# ========== 2. 创建或克隆仓库 ==========
# 方式一：初始化新仓库
mkdir myproject
cd myproject
git init
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"

# 方式二：克隆现有仓库
git clone https://github.com/username/myproject.git
cd myproject

# ========== 3. 日常开发工作流 ==========
# 查看状态
git status

# 创建并切换到新分支
git checkout -b feature/login
# 或 Git 2.23+
git switch -c feature/login

# 修改文件
echo "some content" > file.txt

# 查看修改
git diff

# 添加到暂存区
git add file.txt

# 提交
git commit -m "feat: add login feature"

# 推送到远程（首次推送设置上游）
git push -u origin feature/login

# ========== 4. 合并分支 ==========
# 切换到主分支
git checkout main
# 或 Git 2.23+
git switch main

# 拉取最新代码
git pull origin main

# 合并功能分支
git merge feature/login

# 推送到远程
git push origin main

# 删除功能分支
git branch -d feature/login
git push origin --delete feature/login

# ========== 5. 查看历史 ==========
# 查看提交历史
git log
git log --oneline
git log --graph --oneline --all

# 查看指定文件的历史
git log --follow file.txt

# 查看提交详情
git show HEAD
git show commit-hash

# 比较差异
git diff HEAD~1 HEAD
git diff main feature

# ========== 6. 撤销操作 ==========
# 撤销工作区修改
git checkout -- file.txt
git restore file.txt

# 撤销暂存区修改
git reset HEAD file.txt
git restore --staged file.txt

# 修改最后一次提交
git commit --amend

# 重置到上一次提交（保留修改）
git reset --soft HEAD~1

# 贮藏修改
git stash
git stash list
git stash pop

# ========== 7. 标签管理 ==========
# 创建附注标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 推送标签
git push origin v1.0.0
git push origin --tags

# 查看标签
git tag
git show v1.0.0
```

### Conventional Commits 提交示例
```bash
# 新功能
git commit -m "feat: add user login functionality"
git commit -m "feat(auth): add JWT token support"

# 修复 bug
git commit -m "fix: correct user profile display"
git commit -m "fix(api): handle null response correctly"

# 文档变更
git commit -m "docs: update README with installation guide"
git commit -m "docs(api): add API documentation"

# 代码格式（不影响代码运行）
git commit -m "style: format code with Prettier"
git commit -m "style: fix indentation"

# 重构（既不修复bug也不添加功能）
git commit -m "refactor: extract common function"
git commit -m "refactor(user): simplify user service"

# 性能优化
git commit -m "perf: optimize database query"
git commit -m "perf: improve load time"

# 测试
git commit -m "test: add unit tests for user service"
git commit -m "test: add integration tests"

# 构建或工具相关
git commit -m "chore: update dependencies"
git commit -m "chore: add build script"

# 重大变更（在页脚标记）
git commit -m "feat: change API endpoint

BREAKING CHANGE: The /api/users endpoint has been changed to /api/v2/users"

# 关闭 issue
git commit -m "fix: handle null user data

Closes #123"
```

### Git Flow 工作流示例
```bash
# Git Flow 分支模型
# - main/master: 生产环境代码
# - develop: 开发分支
# - feature/*: 功能分支
# - release/*: 发布分支
# - hotfix/*: 热修复分支

# 1. 创建 develop 分支
git checkout -b develop main
git push -u origin develop

# 2. 创建功能分支
git checkout -b feature/user-auth develop
# 开发...
git add .
git commit -m "feat: add user authentication"
git push -u origin feature/user-auth

# 3. 完成功能分支，合并到 develop
git checkout develop
git pull origin develop
git merge --no-ff feature/user-auth
git push origin develop
git branch -d feature/user-auth
git push origin --delete feature/user-auth

# 4. 创建发布分支
git checkout -b release/1.0.0 develop
# 更新版本号，修复小 bug...
git add .
git commit -m "chore: prepare release 1.0.0"
git push -u origin release/1.0.0

# 5. 完成发布分支
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main
git push origin v1.0.0

# 合并回 develop
git checkout develop
git merge --no-ff release/1.0.0
git push origin develop
git branch -d release/1.0.0
git push origin --delete release/1.0.0

# 6. 热修复分支
git checkout -b hotfix/1.0.1 main
# 修复 bug...
git add .
git commit -m "fix: critical security issue"
git push -u origin hotfix/1.0.1

# 完成热修复
git checkout main
git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1 -m "Hotfix version 1.0.1"
git push origin main
git push origin v1.0.1

# 合并回 develop
git checkout develop
git merge --no-ff hotfix/1.0.1
git push origin develop
git branch -d hotfix/1.0.1
git push origin --delete hotfix/1.0.1
```

### GitHub Flow 工作流示例
```bash
# GitHub Flow - 简化的工作流
# 1. 从 main 创建分支
git checkout main
git pull origin main
git checkout -b feature/new-feature

# 2. 提交修改
git add .
git commit -m "feat: add new feature"
git push -u origin feature/new-feature

# 3. 创建 Pull Request
# 在 GitHub 上创建 PR，进行代码审查

# 4. 代码审查和修改
git add .
git commit -m "fix: address review comments"
git push origin feature/new-feature

# 5. 合并到 main
# 在 GitHub 上合并 PR

# 6. 部署
git checkout main
git pull origin main
# 部署到生产环境

# 7. 删除分支
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

## 5. 官方注意事项 / 最佳实践

### 提交最佳实践
1. **小粒度提交**：每个提交只做一件事
2. **清晰的提交信息**：遵循 Conventional Commits 规范
3. **提交前检查**：确保代码可以正常编译和运行
4. **不提交大文件**：使用 Git LFS 或外部存储
5. **不提交敏感信息**：密码、密钥等使用环境变量
6. **使用 .gitignore**：排除不需要提交的文件
7. **写有意义的提交说明**：说明做了什么和为什么

### 分支管理最佳实践
1. **选择合适的工作流**：Git Flow、GitHub Flow、Trunk-Based
2. **分支命名清晰**：feature/*、hotfix/*、release/*
3. **及时删除分支**：合并后及时删除已完成的分支
4. **定期同步主分支**：避免分支过时太久
5. **保护主分支**：限制直接推送到 main/master
6. **代码审查**：合并前进行代码审查
7. **CI/CD 集成**：自动化测试和部署

### 远程仓库最佳实践
1. **定期推送**：不要只在本地工作，定期推送到远程
2. **拉取前先提交**：避免工作区混乱
3. **使用 SSH 密钥**：更安全便捷的认证方式
4. **配置多个远程**：fork 项目时配置上游仓库
5. **定期清理远程分支**：删除已合并的远程分支
6. **使用 Pull Request**：通过 PR 进行代码审查
7. **保护重要分支**：设置分支保护规则

### 历史与撤销最佳实践
1. **不要修改已推送的历史**：会影响其他协作者
2. **使用 reflog**：恢复误操作
3. **谨慎使用 --hard**：可能丢失工作
4. **使用 stash**：临时保存修改
5. **变基 vs 合并**：理解两者区别，合理使用
6. **交互式变基**：整理提交历史
7. **定期备份**：重要仓库定期备份

### 冲突解决最佳实践
1. **频繁提交**：减少冲突概率
2. **及时同步**：定期从主分支拉取更新
3. **小分支**：分支存活时间不要太长
4. **理解冲突**：仔细阅读冲突标记
5. **手动解决**：不要盲目接受一方
6. **测试后合并**：解决冲突后测试确保功能正常
7. **使用工具**：配置合适的合并工具
8. **记录解决方案**：复杂冲突可以记录解决方法

### 标签与版本最佳实践
1. **使用语义化版本**：vMAJOR.MINOR.PATCH
2. **创建附注标签**：包含版本信息和说明
3. **及时推送标签**：推送到远程仓库
4. **从标签创建分支**：需要修复旧版本时
5. **版本号规范**：遵循 Semantic Versioning
6. **记录变更**：维护 CHANGELOG
7. **发布流程**：制定规范的发布流程

### 安全最佳实践
1. **不要提交敏感信息**：密码、密钥、API Key
2. **使用 .gitignore**：排除敏感配置文件
3. **使用 Git Secret**：管理敏感数据
4. **仓库权限管理**：合理设置访问权限
5. **定期轮换密钥**：泄露后立即更换
6. **使用 SSH**：优先使用 SSH 而不是 HTTPS
7. **签名提交**：GPG 签名提交和标签
8. **审计历史**：定期检查仓库访问历史

### Git 命令速查表

#### 基础命令
| 命令 | 说明 |
|------|------|
| `git init` | 初始化仓库 |
| `git clone <url>` | 克隆仓库 |
| `git status` | 查看状态 |
| `git add <file>` | 添加到暂存区 |
| `git commit -m <msg>` | 提交 |
| `git log` | 查看历史 |
| `git diff` | 查看差异 |

#### 分支命令
| 命令 | 说明 |
|------|------|
| `git branch` | 查看分支 |
| `git branch <name>` | 创建分支 |
| `git checkout <name>` | 切换分支 |
| `git merge <name>` | 合并分支 |
| `git branch -d <name>` | 删除分支 |

#### 远程命令
| 命令 | 说明 |
|------|------|
| `git remote -v` | 查看远程 |
| `git push` | 推送到远程 |
| `git pull` | 从远程拉取 |
| `git fetch` | 从远程获取 |

#### 撤销命令
| 命令 | 说明 |
|------|------|
| `git restore <file>` | 撤销工作区 |
| `git restore --staged <file>` | 撤销暂存区 |
| `git reset --soft HEAD~1` | 撤销提交（保留修改） |
| `git reset --hard HEAD~1` | 撤销提交（丢弃修改） |
| `git stash` | 贮藏 |

## 6. 官方文档参考链接

- Git 官方文档：https://git-scm.com/doc
- Git 官方书籍：https://git-scm.com/book/zh/v2
- Git 命令参考：https://git-scm.com/docs
- Git 入门指南：https://git-scm.com/docs/gittutorial
- Conventional Commits：https://www.conventionalcommits.org/
- Semantic Versioning：https://semver.org/lang/zh-CN/
- Git Flow：https://nvie.com/posts/a-successful-git-branching-model/
- GitHub Flow：https://docs.github.com/en/get-started/quickstart/github-flow
- Git GitHub：https://github.com/git/git
