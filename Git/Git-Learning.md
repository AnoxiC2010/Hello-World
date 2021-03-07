# Git Notes

* git init  
  在新建的本地目录总执行，初始化本地库,初始化 .git库文件夹

* git --version  
  查看git版本

* git config --global user.name "xxxx"  
  设置全局用户名为xxxx

* git config --global user.email "xxxx@xxx.com"  
  设置全局的用户邮箱

* git add xxx.md  
  提交到本地暂存区

* git commit -m "提交说明" xxx.md  
  本地暂存区提交到本地库

* git commit -m "解决合并冲突说明"  
  在合并中状态下使用，合并完成提交不能加文件名

* git status  
  查看所在分支工作区和暂存区的状态

* git log  
  查看提交记录日志，从近到远，如果记录太多左下角有冒号，空格键下一页，B键上一页，尾页左下显示END，按Q退出

* git log --pretty=oneline  
  git log --oneline  
  git reflog  
  三种简化查看日志

* git reset --hard 67c8975  
  版本回退到索引为67c8975的版本（本地库，暂存区，工作区）  
  可以用于恢复删除的文件（当前索引可以用HEAD表示）

* git reset --mixed  
  回退本地库指针时重置暂存区，改动工作区

* git reset --soft  
  本地库指针移动但是不改动暂存区和工作区

* git diff xxx.md  
  比对工作区和暂存区的xxx.md文件，不加文件名则比对所有文件

* git diff HEAD xxx.md  
  比对暂存区和本地库指针版本（用HEAD表示当前指针）的xxx.md文件差异

* git branch -v  
  查看本地库所有分支，带星号为当前分支

* git branch xxxxx  
  创建名为xxxxx的分支

* git checkout xxxxx  
  切换到名为xxxxx的分支

* git merge xxxxx  
  切换到主分支后执行此命令，合并名为xxxxx的分支到主分支，执行后如果有冲突会提示，且显示合并标签（master | MERGING)，而且查看文件会看到冲突痕迹

  ```
  <<<<<<<<HEAD
  ========
  >>>>>>>>xxxxx
  认为修改合并后add并status能看到冲突解决但仍在合并状态中
  再次git commit  -m “message you add"即可合并完成，合并标签消失，注意合并的commit命令勿加文件名
  ```

* git remote -v  
  查看远程库别名

* git remote add origin https://.....git  
  起别名

* git push origin master  
  将本地master分支向远程库推送（这时候会输入github账号密码）

* git clone https://......git  
  克隆操作，有三个作用，初始化本地库，完整克隆远程库到本地库，替我们创建远程库的别名origin

* git fetch origin master  
  将远程库内容抓取到本地，但并不更新工作区文件

* git checkout origin/master  
  查看远程库master分支

* git checkout master  
  切换到master分支

* git merge origin/master  
  把远程库master分支合并到本地库master分支，合并前先切回到本地master

* git pull origin master  
  当于fetch和merge的合并

> 免密操作
* ssh-keygen  -t rsa -C xxxx@xxx.com  
  在用户目录下执行该命令
  邮箱为github注册邮箱，执行后再三次回车确认默认值（想输入点东西也行）
  user\.ssh\目录下生成了id_rsa和id_rsa.pub文件
  id_rsa.pub里面的ssh值可复制后到github网站添加到自己的账户的SSH keys中
* git remote add origin_ssh git@github.com:…….git  
  给ssh远程地址起别名
* git push origin_ssh master  
  用ssh方式提交，不用每次都进行身份验证，但是只针对一个账号