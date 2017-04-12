# GitLab

#### 作者：胡小根

#### 邮箱：hxgqh@haomo-studio.com

## 1.1. 使用GitLab需要完成的步骤

### a. 在gitlab创建帐号，并发给我们

请在以下网址创建帐号：https://gitlab.com/。创建完账号后，将以下内容发给我的邮箱（hxgqh@126.com）：

    用户名：例如hxgqh
    邮箱：例如hxgqh@126.com

我们将把你的帐号加入到团队的项目中，并将给大家发邮件通知你的帐号已经可用。

### b. 下载、安装Git（只限开发人员）

下载Git地址：git-scm

### c. 设置本地的git信息，示例如下：（只限开发人员）

    git config --global user.name "Hu Xiaogen"
    git config --global user.email "hxgqh@126.com"

### d. 添加自己电脑的SSH Keys到服务器中（只限开发人员）

在gitlab.com网页中，点击自己帐号的"Profile settings"中的SSH Keys,添加以下方式生成的SSH Key。

1) windows下生成方式(前提是安装了git)：


    打开Git Bash
    输入命令：ssh-keygen -t rsa -C "your-gitlab-email@xx.com"
    在C:\Users\your_account\.ssh\下产生两个文件：id_rsa和id_rsa.pub
    用记事本打开id_rsa.pub文件，id_rsa.pub中的内容即为SSH Key。

2) *nix下生成方式：

    ssh-keygen -t rsa -C "your-gitlab-email@xx.com"
    cat ~/.ssh/id_rsa.pub

cat显示的信息即为SSH Key

注意：当换电脑的后，需要重新上传新电脑的SSH Key

### e. 将已有代码加入到git仓库：（只限开发人员，且只有当项目没有代码时才这样做）

    cd existing_git_repo

    git remote add origin git@gitlab.com:hxgqh/csic-system.git  # 替换成对应项目的git地址

    git push -u origin master

## 1.2 添加项目需求项步骤(由项目经理负责完成)

添加某一条需求项的步骤：

    1) 在gitlab.com上，进入某一个具体的项目，点击进入Issue
    2) 点击添加New Issue，进入Issue编辑界面
    3) 填写Issue的Title/Description，将需求点分配(Assign)给对应的开发人员。
    注意：如果由项目的里程碑(Milestone)的话，需要在之前创建里程碑。
    4) (重要)将Labels填写为：feature

## 1.3 添加项目测试项(由项目测试人员负责完成)
添加某一条测试项的步骤：

    1) 在gitlab.com上，进入某一个具体的项目，点击进入Issue
    2) 点击添加New Issue，进入Issue编辑界面
    3) 填写Issue的Title/Description，将需求点分配(Assign)给对应的开发人员。
    注意：如果由项目的里程碑(Milestone)的话，需要在之前创建里程碑。
    4) (重要)将Labels填写为：test




