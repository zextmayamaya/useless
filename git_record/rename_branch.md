<details><summary><h2>为分支重命名</h2></summary>

```
	//为本地分支改名
	git branch -m oldBranchName newBranchName
	//同上，分两步操作
	git checkout oldBranchName
	git branch -m newBranchName

	//为远端分支改名，先删除远端分支
	git push origin --delete old-branch
	//push 新建的远端分支
	git push -u origin new-branch
```
</details>
