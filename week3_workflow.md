**Workflow**

This week I have successfully completed my task, which was MAUI-APP#2 on our taskboard, seen here: 

Firstly, I accepted a task from the task board, moved it into "in progress" and assigned it to myself (Fig 1). 

Next, I updated it with the rest of the functionality required from the issue (Fig 2). 

I then created my own branch as per our team workflow rules, named "feature/MAUI-APP#2 (Fig 3), and worked through the functionality required, adding a table called "rota_type" to the database, and setting the one property/column required to "name". All CRUD functionality works perfectly (Fig 4).

I then tested it manually with all use cases, for example; inputting empty strings, deleting and then adding in the same thing again to check it readded, deleting it again, reloading the app, checking the values etc.

When satisfied, I then followed the team workflow information to complete my PR. The workflow we agreed upon being seen here: [<LINK>](https://github.com/Software-Engineering-Red/MAUI-APP/blob/master/Documentation/workflow.md)

So, I pulled all changes from the remote develop branch into mine, before merging develop into my branch and resolving any conflicts, before moving my issue to "In Review" (Fig 5) and beginning a PR, assigning two people to check my request (Fig 6). There were no constructive comments outside of "looks good to me", so nothing for me to edit (Fig 7), so I squashed and merged as per our workflow: [<LINK>](https://github.com/Software-Engineering-Red/MAUI-APP/pull/25)

Finally, I  checked my work against the team Defintion of Done, seen in the same workflow in the repo; 
[<LINK>](https://github.com/Software-Engineering-Red/MAUI-APP/blob/master/Documentation/workflow.md)

I then updated the issue with all comments and relevant information, and moved my issue to done (Fig 8). 

**Reflection**

Whilst completing the task I got too weighed down with the UI, and how to design it. I realised I needed first to decide on a structure for the project itself, so I discussed this with one of my colleagues/teammates, and we worked together on the MVVM structure we eventually landed on. I also struggled with naming the table exactly as it should be in the issue, but as I looked around online I realised it is just one line above the class as a decorator (Fig 9) which was needed. 

With regards to the workflow/process, I think it's clear. The main areas I think it could be improved in future iterations are in the "In Review" section, as it isn't stated when that should be done, and nor is it explicity mentioned that the issue should only be moved to "Done" after it has been "deployed", as the criteria could be met after simply showing a teammate the code locally. Additionally, I think having a template of commit comments and PR comments would be a good thing.

The task board is clear and succinct, but personally I would appreciate it if the tasks were already filled out with all the requirements before being selected, as otherwise we are essentially working off the "issues" folder in the module repo, as well as the taskboard itself. 

Fig 1: 
![1](/images/updatedBoard.png)  
 ‎ 
Fig 2 :
![2](/images/extraInformation.png)
 ‎ 
Fig 3 :
  ![3](/images/myBranch.png)
 ‎ 
Fig 4 :
  ![4](/images/workingCrug.png)
 ‎ 
Fig 5 :
![5](/images/review.png)
 ‎ 
Fig 6 :
  ![6](/images/PRtoDev.png)

 ‎ 
Fig 7 :
  ![7](/images/cleanComments.png)

 ‎ 
Fig 8 :
  ![8](/images/done.png)
 ‎ 
Fig 9 :
  ![9](/images/rotaType.png)
