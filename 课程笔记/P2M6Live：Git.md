# P2M6Live：Git

Git 是指分布式版本控制系统，常用的有 Github、gitee、gitlab。

每一个单位都需要从服务器下载完整备份，包括历史修改信息。 
多人同时上传修改时，新文件会被接受，而同名文件的不同上传需要手动处理冲突。

#### 配置用户

安装 Git 完成后：  
- 全局用户名：`git config --global user.name "myName"`  
- 全局邮箱：`git config --global user.name "myName"`

全局的配置文件是用户文件夹下的 `.gitconfig` 文件

### 提交与重置

#### 相关概念

- 工作区
- 暂存区
- 版本库

#### 提交修改的主要操作

1. 托管文件夹  
   文件夹下，git bash here 进入命令行

2. 创建本地仓库  
   命令：`git init` 初始化  
   出现了 `.git` 文件夹

3. 提交到本地仓库

    1. 添加到暂存区  
        `git add ./` 添加文件夹下所有文件
    2. 把暂存区的文件提交到本地仓库  
        `git commit -m 'note'` note 为提交说明
    
    两步并作一步：`git commit -am 'note'`

##### 状态查看

`git status` 显示当前文件状态，加 `-s` 只显示简略信息  
未添加未提交：红色  
已添加未提交：绿色

#### 版本控制

##### 查看提交日志

- `git log` 详细版本日志，提交人和时间  
  加 `--oneline` 一行日志，只有版本号和提交说明

  **命令行参数：**  
  `-p` / `--patch`：显示版本修改，补丁形式  
  `-n`：显示最近的 n 次提交版本

- `git reflog` 全部操作日志，包括提交和回退

##### 版本回退

1. 暂存区回退

   - `git reset Head~N` 回退到之前的版本  
     从当前版本向旧版本从 0 开始依次排列。  
     e.g. `git reset Head~1` / `git reset Head^` 到上一个版本。  
   - `git reset VersionID` 回退到具体版本号的版本   
     e.g. `git reset 645e5c6`

   ⚠以上是默认 `--mixed` 的缺省形式。会保留历史记录，回退后日志仍可找到回退前的版本。

2. `git checkout URL` 将暂存区同步到工作区  
   同步后，文件才发生变化  
   e.g. `git checkout ./` 检查同步整个托管文件夹

###### 硬回退模式 / 强制回退 --hard

加 `--hard`，该版本以后的版本日志会被删除，工作区也会直接同步，不需再 checkout。

谨慎使用。不过强制回退后，查看操作日志里还有记录。

###### 软回退模式 --soft


### 分支

master 分支为主分支，也是默认分支。实际工作中，不允许在 master 中直接操作，修改通常进行在临时的分支，测试完成后再融合进 master。

#### 创建分支
`git branch BranchName` 创建新分支  
`git checkout BranchName` 转至某分支  
`git branch` 查看所有分支

#### 合并分支

`git merge BranchName` 当前分支合并某分支  
通常是主分支合并其他测试分支，或者说其他测试分支并入主分支，那么这时需要先切换到主分支再合并。  
加 `-d`，会在合并完成后删除被合并分支。

##### 合并冲突

不同分支的同一位置都有修改，这时需要手动选择处理。

冲突处理完成后，文件已与合并前不同，需要重新 add、commit。

### Github 在线托管平台

Git 远端操作：发生在版本库和平台之间

1. Github 上创建一个 repository
2. 连接远端仓库 `git remote add SourceName URL`  
   连接后，便能以 SourceName 指代仓库地址
   - HTTPS：
   - SSH： 
3. 克隆项目到本地 `git clone URL`
4. 下拉到本地  `git pull Source Master` 将远端的合并进本地
5. 本地仓库（分支）推送到远端仓库 `git push SourceName BranchName`  
   push 前务必再次 pull，保证使用的是最新版本

#### VSCode 可视化操作

