## 分支流程

![image-20200513100652924](http://qa3sq0khl.bkt.clouddn.com/image-20200513100652924.png)

> 当hotfix分支紧急修复线上问题确认没问题后，需要developer在gitlab中创建四个merge request：
>
> 1、hotfix -> develop。
>
> 2、develop -> RC。
>
> 3、develop -> alpha。
>
> 4、develop -> dev。
>
> 来保证环境分支为最新代码。

## 分支命令规范

### feature 功能开发分支

> 从develop检出，并最终合并到develop分支
> 特点是生命周期长，一个功能模块完成上线后,至少保留一个月,才可删除该分支。
> 需要合并到 test rc develop pro分支
 > 例子  feature/v1-user
 > feature/版本号-功能模块

### hotfix bug修复分支
> 热修复分支，线上需要紧急修复的bug，从master检出。
hotfix的生命周期短，bug修复完成后即可删除该分支
需要合并到 test rc develop pro分支
> 例子: hotfix/v1-ordestatus
> hotfix/版本号-修复的问题
> 

## Tag规范
> RC环境测试完成后，由管理员将分支合并到master分支并基于master分支打tag。
> tag名称严格按照版本号规范执行，并在Message中写明本次上线的内容或解决的问题。
> 

