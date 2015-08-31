This note provides an overview for how to publish your sample apps to the Kinoma GitHub repository using GitHub pull requests.

We've developed a collection of sample code to demonstrate how to build apps with KinomaJS. The sample code is available on our GitHub site and a wide variety of programming topics are covered. We encourage you to share your great ideas and experience by contributing sample apps (or improving existing samples) as well! Here, we go through step by step how to do just that.

Note: Please refer to this [GitHub article](https://help.github.com/articles/using-pull-requests/) for a more detailed explanation of Git pull requests and the fork and pull technique outlined in this note.

***
To contribute to the Kinoma samples repository, first sign into your GitHub account. If you don’t already have a GitHub account, you can sign up for one free [here](https://github.com/Kinoma/KPR-examples) on our GitHub site by clicking the “Sign up” button.

![](../images/github/signin.png)

***
Once signed in, fork the KPR-examples repository by clicking the “Fork” button located at the top-right corner of the page. The fork allows you to make changes/additions without affecting the public repository.

![](../images/github/fork.png)

***
I signed in as bfriedkin and forked the repository. The fork is identified here as bfriedkin/KPR-examples:

![](../images/github/forked.png)

***
After you’ve forked the repository, clone the repository on your computer to make changes. I typically use Git from the command line and clone with SSH, for example:

	Brian-Air-2:~ brianfriedkin$ git clone git@github.com:bfriedkin/KPR-examples.git
	
![](../images/github/clone.png)

The GitHub repository page also provides HTTPS and Subversion clone URLs, if you don’t have a SSH key associated with your GitHub account. There’s also a “Clone in Desktop” button you can use to launch the GitHub desktop application to clone the repository. Any of these methods can be used to clone a local copy of the repository to your computer.

***
Now that you have a local forked copy of the repository, you can commit and push your changes. Note that at this point you are pushing changes to your personal fork – not to the master KPR-examples branch. In the example below, I’ve committed a change to the i2c-temperature app screen shot:

	Brian-Air-2:KPR-examples brianfriedkin$ git status
		modified:   screenshots/i2c-temperature-example.jpg

	Brian-Air-2:KPR-examples brianfriedkin$ git commit . -m "Updated screen shot to limit temperature display to two decimal places"

	Brian-Air-2:KPR-examples brianfriedkin$ git push
	To git@github.com:bfriedkin/KPR-examples.git
	
![](../images/github/pull-requests.png)

***
After pushing your changes, create a “Pull request” so that the maintainer of the KPR-examples repository can review and merge your changes into the master branch. Select “Pull Requests” and then tap the “New pull request” button:

![](../images/github/new-pull-request.png)

***
After you’ve created the new pull request, you’ll likely want to review your changes before submitting. First, select the “compare across forks” link to configure the base and head forks to compare:

![](../images/github/compare-across-forks.png)

***
Then use the popup menus displayed to select Kinoma/KPR-examples as the base fork and /KPR-examples as the head fork:

![](../images/github/compare.png)

***
A summary of your changes is displayed. This is your opportunity to review changes before submitting to the project maintainer. Tapping the “Create pull request” button directs you to a page where you can enter an optional pull request title and/or comments.

> Note: If you are submitting a new sample app, we’d appreciate you also providing a description of the application along with a screen shot. Images can be attached to the pull request. All code contributed to the Kinoma samples repository must be provided under the Apache license and attach the required boilerplate notice.

![](../images/github/create-pull-request.png)

Finally, tap the “Create pull request” button to submit the request. The pull request will be sent to the project maintainer for review and you will be notified via email if there are any questions and when the changes have been accepted/merged.