###  函数变量命名 

-  通用命名规则

  -  尽可能使用遵循对应的分类、描述性的命名
  - 连接可以使用下划线“_”   例如：http_server_logs，authorize_blanceofbance

- 变量命名规则

  - const的常量使用：小写k开头

  -  普通变量命名：单词之间用下划线连接
  - 类数据：类定义大写首字母例如Key，Latest；类成员结尾使用"_",也就是下划线 例如：table_name_

- 函数命名

  - 一般来说, 函数名的每个单词首字母大写

  - 涉及数值处理的函数的命名与变量一致. 一般来说它们的名称与实际的成员变量对应, 但并不强制要求.

         1. Countnumber()
         1. Transfer()

    

###  git 命令

- 1.   远程库的连接

  -  1      git remote add origin ..........<你的地址>

  - 2.   git push origin<定义远程库的名字>   master <分支的名字>
       - 第一次推送的话：git push -u   <origin>   < master>    推送分支上所有内容

    -   push  是将暂存区的内容推送到所有远程库

    - 查看/删除远程库

      > - git remote -v                                                         //查看远程库的状态
      > - git remote rm origin<远程库的名字>                //删除远程库

 

- 2.  创建/查看分支

     >    git branch                            //查看当前分支
     >
     >   git branch   <name>          //创建分支
     >
     >   git switch   <name>           //切换分支
     >
     >   git switch -c <name>       //合并分支
     >
     >  git   branch -d  <name>    //删除分支



- 3. 推送分支

     > 1. git pull                                                              //获取  远程库/分支 最新提交
     > 2. 如果有冲突，手动恢复
     > 3. git push  <origin>   <branch - name>     //提交当前暂存区文件到远程仓库

- 4. commit的信息：详细清楚描述完成什么功能