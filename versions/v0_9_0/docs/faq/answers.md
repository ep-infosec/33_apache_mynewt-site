### How do I submit a bug?

<br>

If you do not have a JIRA account sign up for an account on [JIRA](https://issues.apache.org/jira/secure/Signup!default.jspa).

Submit a request to the @dev mailing list for your JIRA username to be added to the Apache Mynewt (MYNEWT) project. You can view the issues on JIRA for the MYNEWT project without an account but you need to log in for reporting a bug. 

Log in. Choose the "MYNEWT" project. Click on the "Create" button to create a ticket. Choose "Bug" as the Issue Type. Fill in the bug description, how it is triggered, and other details. 

### How do I request a feature?

<br>

If you do not have a JIRA account sign up for an account on [JIRA](https://issues.apache.org/jira/secure/Signup!default.jspa).

Submit a request to the @dev mailing list for your JIRA username to be added to the Apache Mynewt (MYNEWT) project. You can view the issues on JIRA for the MYNEWT project without an account but you need to log in for reporting a bug. 

Log in. Choose the "MYNEWT" project. Click on the "Create" button to create a ticket. Choose "Wish" as the Issue Type. Fill in the feature description,  benefits, and any other implementation details. Note in the description whether you want to work on it yourself. 

If you are not a committer and you wish to work on it, someone who is on the committer list will have to review your request and assign it to you. You will have to refer to this JIRA ticket in your pull request.

### I am not on the committer list. How do I submit a patch? 

<br>

**You submit your proposed changes for your peers with committer status to review and merge.**

The "develop" branch on Mynewt's repository contains the most recent changes made by the community of developers. Contributions from you need to go into this branch. The essential steps to setting up your project space for working with the latest code from "develop" and make pull requests into "develop" to get your code added are the following:


**Step 1:** Create a fork of the entire Mynewt repository on github.com.
**Step 2:** Setup repository on your laptop to use code in “develop” branch. You then create a new branch “mybranch” using “git checkout –b”. You also add a remote handle named “fork” that points to the github fork you created in Step 1.
```    $ newt new devproject    $ cd devproject    $ vi project.yml        # change version to 0-dev for repository.apache-mynewt-core    $ newt install    $ cd repos/apache-mynewt-core    $ git status        On branch develop        Your branch is up-to-date with 'origin/develop'.        nothing to commit, working directory clean    $ git checkout –b mybranch    $ git remote -v        origin https://github.com/apache/incubator-mynewt-core.git (fetch) 
        origin https://github.com/apache/incubator-mynewt-core.git (push)    $ git remote add fork https://github.com/<user>/incubator-mynewt-core 
    $ git remote -v        origin https://github.com/apache/incubator-mynewt-core.git (fetch) 
        origin https://github.com/apache/incubator-mynewt-core.git (push)        fork https://github.com/<user>/incubator-mynewt-core (fetch) 
        fork https://github.com/<user>/incubator-mynewt-core (push)
```**Step 3:** Check you are in “mybranch”. Write code. Stage and commit your changes(example shows adding all).
```   $ git checkout mybranch   $ git add .   $ git commit –m “your message about your code changes”
```
**Step 4:** Always pull the latest from develop on Apache mirror to “mybranch” before pushing any changes to remotes. If you see merge conflicts, resolve them first.
```   $ git pull --rebase origin develop
```
**Step 5:** Push your changes to “mybranch” branch on your github fork. If “mybranch” does not exist yet on your github fork, the command automa;cally creates it.
```   $ git push fork mybranch
```
**￼Step 6:** Generate a pull request from “mybranch” in your fork to “develop” in Mynewt using the "New pull request" button on github.com.


![Mynewt Dev Cycle](mynewt_dev_cycle.jpg)
    
    
### I would like to make some edits to the documentation. What do I do?

<br>

You submit your proposed changes for your peers with committer status to review and merge. 

Go to the [documentation mirror](https://github.com/apache/incubator-mynewt-site) on github.com.

Navigate to the file you wish to edit on github.com. All the technical documentation is in Markdown files under the `/docs` directory. Click on the pencil icon ("Edit the file in your fork of this project") and start making changes.

Click the green "Propose file change" button. You will be directed to the page where you can start a pull request from the branch that was created for you. The branch is gets an automatic name `patch-#` where # is a number. Click on the green "Compare & pull request" to open the pull request.

In the comment for the pull request, include a description of the changes you have made and why. Github will automatically notify everyone on the commits@mynewt.incubator.apache.org mailing list about the newly opened pull requests. You can open a pull request even if you don't think the code is ready for merging but want some discussion on the matter.

Upon receiving notification, one or more committers will review your work, ask for edits or clarifications, and merge when your proposed changes are ready.

If you want to withdraw the pull request simply go to your fork `https://github.com/<your github username>/incubator-mynewt-site` and click on "branches". You should see your branch under "Your branches". Click on the delete icon.

### I would like to make some edits to the documentation but want to use an editor on my own laptop. What do I do?

<br>

You submit your proposed changes for your peers with committer status to review and merge. 

Go to the [documentation mirror](https://github.com/apache/incubator-mynewt-site) on github.com. You need to create your own fork of the repo in github.com by clicking on the "Fork" button on the top right. Clone the forked repository into your laptop (using `git clone` from a terminal or using the download buttons on the github page)and create a local branch for the edits and switching to it (using `git checkout -b <new-branchname>` or GitHub Desktop). 

Make your changes using the editor of your choice. Push that branch to your fork on github. Then submit a pull request from that branch on your github fork.

The review and merge process is the same as other pull requests described for earlier questions.

