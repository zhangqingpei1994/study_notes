 ## 1. 解决由于误传大文件而git push不上去问题

 问题描述:自己把一个超过100M的大文件放到了github的本地,然后进行git add .   ->git commit -m "  "之后,git push不上去,解决方法:

 a. 首先找到那个大文件,比如我一开始我的是  OpenGl超级宝典第5版.pdf,然后执行
 
    git rm --cached OpenGl超级宝典第5版.pdf

在这里需要注意一下有时候会提示:

    fatal:路径规格 OpenGl超级宝典第5版.pdf 未匹配任何文件
 
 出现这种情况是由于大文件路径没指对,在git push不上去的时候先不要在本地更改任何东西,找到大文件对应的正确路径,执行上面的git rm命令,如果文件路径对的话是可以tab出来的(由于路径不对自己在这搞了一晚上) 

b. 然后执行下面的命令:

     git commit --amend -CHEAD

c .然后重新执行git push就可以上传成功了

自己觉得自己在这耗费一晚上的主要原因就是我是在2017年年底放寒假的时候把自己的中期想放到github上,然后git push大文件push不上去,上不去之后我就直接删了本地文件,又重新git add .   git commit了,可能这样导致了大文件路径不对了,过完年回来又瞎搞了一下,然后用git rm删除的时候一直提示路径规格未匹配任何文件,在解决问题的时候自己还尝试了git reset  --hard  commit_id 的方法也不知道起啥作用没.还有一个命令可以查看push不到远程的id:    git cherry -v

参考: https://www.cnblogs.com/akiha/p/5705505.html

https://www.urlteam.org/2015/12/%e8%a7%a3%e5%86%b3gitpush%e7%9a%84%e6%97%b6%e5%80%99%e5%9b%a0%e4%b8%ba%e8%af%af%e5%8a%a0%e5%85%a5%e7%89%b9%e5%a4%a7%e6%96%87%e4%bb%b6%ef%bc%8c%e5%af%bc%e8%87%b4push%e5%a4%b1%e8%b4%a5/ 