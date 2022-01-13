# Git 基本配置与常用操作

## 1 推荐配置

### 1.1 基本配置

```bash
git config --global user.name "你的英文名字"
git config --global user.email 你的邮箱
git config --global push.default simple
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```

- 使用 `git config` 命令配置 Git
  - `--global` 表示该配置为全局配置
- 使用 `user.name` 和 `user.email` 配置姓名和邮箱
- 使用 `push.default` 配置默认推送规则
  - `simple` 表示将当前分支推送到同名的远程分支上
- 使用 `core.editor` 配置在提交、标记信息时默认打开的文本编辑器
  - `code --wait` 表示使用一个 VSCode 的新标签页打开并等待其关闭
- 使用 `core.autocrlf` 配置自动处理换行符规则以应对跨平台开发协作问题
  - `input` 表示在 Windows 系统上统一转换为 CRLF 换行符，在 macOS 或 Linux 系统上统一转换为 LF 换行符

### 1.2 快进合并/拉取策略相关配置

```bash
git config --global merge.ff false
git config --global pull.ff true
```

- 使用 `merge.ff` 配置合并代码时的快进（fast-forward）策略
  - `false` 表示关闭快进合并，即合并代码时创建额外的提交记录
- 使用 `pull.ff` 配置拉取代码时的快进策略
  - `true` 表示开启快进拉取，即拉取代码时默认采用快进拉取，并在需要时创建额外的合并提交记录

## 2 常用操作

### 2.1 查看及删除配置项

```bash
git config --list
git config --global --list
git config --unset user.name
git config --global --unset user.name
```

- 使用 `--list` 查看指定级别下的 Git 配置
- 使用 `--unset` 清除指定级别下的某项配置

### 2.2 代码的暂存/提交/拉取/推送

```bash
git init
git add .
git commit
git commit -m "feat(Home): 新增首页"
git commit -v
git pull
git push
```

- 使用 `init` 将在当前目录下初始化一个 Git 本地仓库
- 使用 `add .` 将当前修改的所有文件存放至暂存区
- 使用 `commit` 打开文本编辑器记录提交信息，保存并关闭后会提交暂存区内所有文件
  - 若没有输入信息，则会放弃本次提交
  - 使用 `-m` 提交暂存区内所有文件并记录一条信息
  - 使用 `-v` 打开文本编辑器提交信息，与 `git commit` 的区别是可以在编辑器中查看本次提交改动
- 使用 `pull` 将远程绑定分支代码拉取至当前本地分支
- 使用 `push` 将当前本地分支代码推送至远程绑定分支

### 2.3 切换分支

```bash
git checkout develop
git checkout -b feature/create-app
```

- 使用 `checkout` 切换当前分支至指定分支
  - 使用 `-b` 基于当前分支在本地创建并切换至指定名称的新分支

### 2.4 查看及删除分支

```bash
git branch
git branch -a
git branch -d hotfix/submit-app
git push origin -d hotfix/submit-app
```

- 使用 `branch` 查看本地分支
  - 使用 `-a` 查看全部分支（包含远程和本地分支）
  - 使用 `-d` 删除指定本地分支
- 使用 `push origin -d` 删除指定远程分支

### 2.5 合并代码

```bash
git merge feature/create-app
git cherry-pick 34dfghi
```

- 使用 `merge` 将指定分支合并至当前分支
- 使用 `cherry-pick` 将指定编号提交合并至当前分支，该操作会产生一条新的提交记录

### 2.6 解决冲突

- 使用 `git merge` 将指定分支合并至当前本地分支发生冲突时，会得到 conflict 提示
- 使用 `git status` 查看发生冲突的文件
- 依次打开相关文件并搜索“====”
- 删除冗余代码及标记（====、>>>>、<<<<）
- 最后使用 `git add .` 将解决冲突后的文件存放至暂存区
- 使用 `git commit` 提交本次合并记录

### 2.7 重置提交

```bash
git log
git log -3
git reset --hard 34dfghi
git reset --hard HEAD^
git reset --soft HEAD^^
```

- 使用 `log` 查看当前本地分支提交记录详情
  - 在 `log` 后输入 `-数量` ，查看当前本地分支最近指定数量的提交记录详情
- 使用 `reset --hard` 后输入提交编号，将当前本地分支重置为指定提交编号的状态
  - `HEAD^` 表示当前本地分支上一条提交记录
- 使用 `reset --soft` 将当前本地分支重置为指定提交记录时的状态，并将下一条记录提交的内容存放至暂存区

### 2.8 创建标记

```bash
git tag
git tag v1.0
git tag -a v1.0 -m "新增首页"
git tag -a v1.0 34dfghi
git tag -d v1.0
git show
git push origin v1.0
git push origin -d tag v1.0
```

- 使用 `tag` 查看当前本地仓库所有标记
  - 在 `tag` 后输入标记名称，将为当前本地分支最近一次提交快速创建标记
  - 使用 `-a` 将为当前本地分支最近一次提交创建标记
  - 使用 `-m` 将为即将创建的标记记录一条信息
  - 输入提交编号，将为本地指定编号的提交记录创建标记
- 使用 `show` 查看本地所有标记详情
- 使用 `push origin` 将指定标记推送到远程仓库
  - 使用 `-d tag` 将删除远程仓库中指定标记

### 2.9 移动文件

```bash
git mv src/views/Layout.vue src/views/components/Layout.vue
git mv src/views/home.vue src/views/Home.vue
```

- 使用 `mv` 将文件移动至指定的路径并将其添加至暂存区
  - 能够保留该文件的历史提交记录，常用于文件重命名

### 2.10 隐藏代码

```bash
git stash
git stash pop
git stash list
git stash clear
```

- 使用 `stash` 将当前未提交的代码存放至本地隐藏区
  - 对于新增文件可能需要先执行 `git add .`
  - 使用 `pop` 将本地隐藏区中最后一条记录还原至工作区
  - 使用 `list` 查看本地隐藏区中所有记录
  - 使用 `clear` 删除本地隐藏区中所有记录

## 3 SSH-Key 配置

### 3.1 单个 SSH-Key 配置

```bash
ssh-keygen -t rsa -b 4096 -C 你的邮箱
```

- 执行上述命令，生成公钥
- `-t rsa` 表示采用 RSA 加密算法
- `-b 4096` 表示采用长度为 4096 比特的密钥对
- `-C` 表示备注内容

```bash
cat ~/.ssh/id_rsa.pub
```

- 执行上述命令，查看公钥内容
- 复制公钥内容至 GitHub 远程仓库 New SSH Key 设置

```bash
ssh -T git@github.com
```

- 执行上述命令，测试是否连接至远程仓库
- 询问 yes/no ，输入 yes 并回车，则配对成功

### 3.2 多个 SSH-Key 配置

#### 3.2.1 使用场景

- 当需要使用多个 Git 账号时，如
  - Gitee 用于公司内部开发
  - GitHub 用于个人开发

#### 3.2.2 解决方案

1. 生成一个公钥用于公司内部开发

```bash
ssh-keygen -t rsa -C 'xxxxx@company.com' -f ~/.ssh/gitee_id_rsa
```

2. 生成一个公钥用于个人开发

```bash
ssh-keygen -t rsa -C 'xxxxx@foxmail.com' -f ~/.ssh/github_id_rsa
```

3. 在 ~/.ssh 目录下新建一个 config 文件，添加如下内容（其中 Host 和 HostName 填写 Git 服务器的域名，IdentityFile 指定私钥的路径）

```bash
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa

# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_id_rsa
```

4. 使用 `ssh -T` 测试

```bash
ssh -T git@gitee.com
ssh -T git@github.com
```

## 参考文章

- [Git - Git Configuration (git-scm.com)](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
- [How can I fix git commit error "Waiting for your editor to close the file..." with VS Code? - Stack Overflow](https://stackoverflow.com/questions/52195877/how-can-i-fix-git-commit-error-waiting-for-your-editor-to-close-the-file-wi)
- [Git - git-config Documentation (git-scm.com)](https://git-scm.com/docs/git-config)
- [Git - git-checkout Documentation (git-scm.com)](https://git-scm.com/docs/git-checkout)
- [Git 配置多个 SSH-Key - Gitee.com](https://gitee.com/help/articles/4229#article-header0)
