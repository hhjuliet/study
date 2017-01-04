分支合并有三种方法：cherry-pick - 妈妈，我也要， merge - 求合体，rebase-合体更彻底
http://pinkyjie.com/2014/08/10/git-notes-part-3/



merge 是一个合并操作，会将两个分支的修改合并在一起，默认操作的情况下会提交合并中修改的内容
merge 的提交历史忠实地记录了实际发生过什么，关注点在真实的提交历史上面
rebase 并没有进行合并操作，只是提取了当前分支的修改，将其复制在了目标分支的最新提交后面
rebase 的提交历史反映了项目过程中发生了什么，关注点在开发过程上面
merge 与 rebase 都是非常强大的分支整合命令，没有优劣之分，使用哪一个应由项目和团队的开发需求决定
merge 和 rebase 还有很多强大的选项，可以使用 git help <command> 查看


rebase和merge的区别


http://blog.isming.me/2014/09/26/git-rebase-merge/



git合并commit


git rebase –i HEAD~2//2表示前两条记录合并



后面根据
http://www.jianshu.com/p/964de879904a步骤