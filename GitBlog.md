1.Git作为一个版本管理工具越来越受很多coder的喜爱。我觉得主要原因可能是因为这几方面吧

（1） git是一个分布式的版本管理工具，也可以是单机版，在没有网络的情况下也可以提交代码到本地仓库。正因为是分布式的版本工具在给它带来优势的同时，而带来了一些弊端，如分布式带来的数据不一致问题

（2）  git从一个分支想另一个分支合并代码的时候，会把要合并的分支所有提交一个一个 的合并到分支上，合并后能看到版本变更记录

（3） git分支切换便捷，其衍生出来的很多工具能帮助我们更好的管理和进行coding，如github/gitlab的wiki fork pull request，issue以及Face Book的code review 工具Phabaritor 的arc diff 和arc land（非常好用墙裂推荐）

（4）有很多非常好用的功能，例如 万能提交的git push –f（滑稽脸）



2.Git的强大还在于其衍生出来强大的工作流体系，能迅速适应不同需求的任务开发

先介绍个人非常喜欢的Github Flow（http://scottchacon.com/2011/08/31/github-flow.html）

（1）       每个开发人员先先Fork官方库到自己的库，也间接的关联了两个库，一个官方库，一个自己的线上的库

（2）       将线上库clone到本地进行功能开发，开发完push到自己的线上仓库

（3）       向官方库发起发起pull request，官方库管理员进行code review，通过之后合并到官方仓库

个人感觉这种工作流再配合Phabricator工具进行code review特别适合中小型团队的功能开发和一个小的微服务团队的coding工作，Phabricator绑定钉钉机器人，coder开发完代码后只需要 arc diff这个简单地命令即可发起code review，机器人会发送信息给Team Leader， Teamleader通过review回复accept，发起人再需一步arc land即可将代码合并，Team Leader 则负责整个项目的代码质量和整体走向。而这个工作流的缺陷则是和运行环境没有联系在一起，而微服务团队已经大量使用容器化技术，这个问题似乎已经迎刃而解，解决了环境不一致的问题

Gitlab Flow（https://about.gitlab.com/2014/09/29/gitlab-flow/）

Gitlab Flow可以说是在容器技术未大行其道之时，弥补github flow的一种补偿方式。

![img](file:///C:\Users\dell\AppData\Local\Temp\msohtmlclip1\01\clip_image002.png)

 

其中引入环境分支解决方案，如上图所示包含一个预发布和生产的分支。当有不同的版本发布需求之时，可以release出不同的。如下图所示

![img](file:///C:\Users\dell\AppData\Local\Temp\msohtmlclip1\01\clip_image004.png)

Master分只是一个主线分支，之后一旦分支稳定了，就建立稳定版的分支，如2.3和2.4，也可以从不同的独立分支检出commit，并和稳定分支合并，这也就是cherry-pick

Gitlab flow的优势或许在于其解决了环境和版本和代码分支对应的问题，缺点也是因为代码分支太多带来的分支维护的开销，不过讲不通的分支分发给不同的生产线负责，有一个总的类似zookeeper的Leader来进行进行统一维护也很好的解决了这个带来的部分问题。

Git Flow

![img](file:///C:\Users\dell\AppData\Local\Temp\msohtmlclip1\01\clip_image006.png)

如上图所示，GitFlow可以说是最为经典的一种代码分支管理

Master分支：主干分支，用作发布环境，这个分支随时都可以打包发布

Feature分支：功能分支，用作功能开发，是Developer分支的一个子分支

Developer分支：Developer集成测试环境，功能开发之后，通过测试合并到该分支之上

Release分支：当Developer分支达到发布需求的时候，开出一个Release分支，当做是预发布环境，与此同时开发可以继续开发，如果不开这个分支，开发便被Blocking在此。当release分支达到发布要求之时，Release分支变向Master分支和Developer分支合并，保证了代码一直，如果其中产生了某些变化，这个分支直接废弃掉。

Hot-Fix分支用于处理处理线上的Bugfix，每一个master分支发生了bug，便开一个hotfix分支，分支开发完之后合入Developer分支，图中画的也合入了master分支，个人感觉不是很推荐这种做法，然后删除Hotfix分支，建议先和入Developer分支，达到developer分支要求之后开一个bug-fix 的Release版本，回归测试通过之后再合入master版本，同时删除bug-fix的release版本。

GitFlow的优点在于其版本控制管理严格，能适应几乎所有公司的开发流程，包括瀑布和快速迭代。缺点也是显而易见，代码分支非常之多，需要长期维护master和developer两个分支，release分之合bugfix分支需要向两个分支分支合并，没有好的工具支撑容易产生版本问题。

三种工作流都介绍完了，就coding而言，只有最合适的没有万金油的workflow，或许像jvm中的G1垃圾收集器一样，把各种GC垃圾收集器结合使用往往会产生最佳的工作效果。