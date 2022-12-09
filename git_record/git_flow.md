# git flow 的操作

1. 首先clone远端的仓库。
2. 为仓库自动生成开发流程所需的分支。
```
	//初始设置
	git flow init -d
```
3. 上一步会创建develop分支，所有的团队人员都要以这个分支为基础进行开发。
4. 开发后push到github远端，其他人再从远端获取到各自终端进行开发。
5. 对分支进行任何操作前都使用pull获取最新代码，修改后尽快push上去，保证远端的代码为最新。

<details><summary><h3>main 分支</h3></summary>
main 分支一直保持着软件可以正常运行的状态，等其他分支进展到可以发步的程度再与master分支合并，这合并只在发布成品时进行。发布时会附加版本编号，git的标签。
</details>
<details><summary><h3>develop 分支</h3></summary>
develop 分支是开发的中心分支，与main分支一样不允许直接修改和提交，程序员要以develop分支为起点新建分支。也就是说develop分支维持着开发过程中最新的源码，方便程序员新建feature分支进行自己的工作。
</details>
<details><summary><h3>feature 分支</h3></summary>
feature 分支以develop分支为起点，是开发者直接更改代码发送提交的分支。流程如下：

- 从develop分支创建feature分支
- 再feature分支中实现目标功能
- 通过github向develop分支发送pull request
- 接受其他开发者审查后，将pull request合并至develop分支
- 与develop分支合并后就完成了feature的工作，就可以根据需要删除

</details>

6. 从develop分支创建add-user名字的feature分支。
```
	git flow feature start add-user
```
7. 编写代码然后commit，反复进行此操作直到功能实现。
8. 将feature/add-user分支push到github库。
```
	git push origin feature/add-user
```

<details><summary><h3>通过代码审查提高代码质量</h3></summary>

- 由其他开发者进行代码审查，并在pull request中反馈
- 修改代码以反馈内容（在本地feature/add-user分支中)
- 将修改后的 feature/add-user 分支push到远程库（自动添加至之前的pull request）
- 重复前三步，直到确认代码没有问题
- 由其他开发者将其合并至develop分支中

-------- 如何反馈审查的pull request --------

- 没有测试或测试未通过
- 违反编码规则
- 代码品质过低（命名不明确，方法冗长等）
- 还有重构的余地
- 有重复部分

</details>

9. github端的pull request与develop合并后，要将其同步到本地的develop分支中。
- 在本地库中，切换到develop分支
- 执行 git pull (fetch & merge)
- 这样本地的develop分支就从github端获取了最新数据
- 每一次从develop分支创建feature分支时，一定要执行上面的操作，保证develop分支处于最新
10. 当develop分支的功能积攒到足以发布时就会用到release分支。
- 将develop分支设置成默认分支在github端，repository->setting->branches->default branch
```
	git checkout develop
	git pull
	//在获取了最新数据的develop分支上创建release分支
	git flow release start '1.0.0'
```
- 这个release分支只处理发布前准备的提交。如果版本编号变更等元数据的添加工作。如果部署到预演环境后测试发现bug，相关的修正也要提交给这个分支，这一阶段提交数应该限制到最低
```
	//修正所有bug后结束release分支
	git flow release finish '1.0.0'
```
- 会提示你输入合并信息，这里可以随意
- 再次提示让你输入一个tag信息，可以输入"release 1.0.0"
- 又一次提示你要合并develop的信息，可以随意
- 完成上面的操作后会看到当前状态，可以查看标签信息
```
	git tag
```
11. 更新到远程仓库
```
	//确保当前分支时develop分支
	git checkout develop
	//将develop分支push到远端
	git push origin develop
	//切换到main分支
	git checkout main
	//将main分支push到远端，刚才finish的操作已经对main更新了，所以release分支合并掉了
	git push origin main
	//push标签到远端
	git push --tags
```
