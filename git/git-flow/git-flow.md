1. Git flow 常用分支

   - ### master

     - 主分支 , 产品的功能全部实现后 , 最终在master分支对外发布
     - 该分支为只读唯一分支 , 只能从其他分支(release/hotfix)合并 , 不能在此分支修改
     - 另外所有在master分支的推送应该打标签做记录,方便追溯
     - 例如release合并到master , 或hotfix合并到master

     ### develop

     - 主开发分支 , 基于master分支克隆
     - 包含所有要发布到下一个release的代码
     - 该分支为只读唯一分支 , 只能从其他分支合并
     - feature功能分支完成 , 合并到develop(不推送)
     - develop拉取release分支 , 提测
     - release/hotfix 分支上线完毕 , 合并到develop并推送

     ### feature

     - 功能开发分支 , 基于develop分支克隆 , 主要用于新需求新功能的开发
     - 功能开发完毕后合到develop分支(未正式上线之前不推送到远程中央仓库!!!)
     - feature分支可同时存在多个 , 用于团队中多个功能同时开发 , 属于临时分支 , 功能完成后可选删除

     ### release

     - 测试分支 , 基于feature分支合并到develop之后  , 从develop分支克隆
     - 主要用于提交给测试人员进行功能测试 , 测试过程中发现的BUG在本分支进行修复 , 修复完成上线后合并到develop/master分支并推送(完成功能) , 打Tag
     - 属于临时分支 , 功能上线后可选删除

     ### hotfix

     - 补丁分支 , 基于master分支克隆 , 主要用于对线上的版本进行BUG修复
     - 修复完毕后合并到develop/master分支并推送 , 打Tag
     - 属于临时分支 , 补丁修复上线后可选删除
     - 所有hotfix分支的修改会进入到下一个release