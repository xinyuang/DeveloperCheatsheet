Manage git
`git init .`  
`git remote add origin <your origin repo>`  
`git remote -v`  
`git pull origin master`  
`git log`  

Manage branch
`git checkout -b [new_branch]` #create a new branch  
`git checkout [your_branch]` #switch to your_branch  
`git branch` #see all branches  


Edit, add and commit your files in current branch.  
`git status`  
`git add .`  
`git commit -m "comment"`  

Merge current branch into master branch  
`git checkout master`  
`git merge <your_branch>`  

Push git  
`git push origin master`  

We'd better not push current branch to origin, instead merge into master first  
`git push -u origin [your_branch] #push your branch to origin`  