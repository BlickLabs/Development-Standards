# Development-Standards

##<a name="index"></a> Index

* [General conventions](#general-conventions)
* [Git conventions](#git-conventions)
* [Pull Requests and Reviews](#pull-requests)

----
###<a name="general-conventions"></a> General conventions.<sub><sub><sub><sub>[Index](#index)</sub></sub></sub></sub>

* Test before code.

* Code review is your right, ask for it to anyone in the team.

* All the pull request must have a successful build (Jenkins, Travis, Circle CI)

* All the pull request must be approved for at least the half of the team plus one.

* All the code is written in English, this includes wikis, commits messages, comments, etc.

* Your status must follow the next convention:
> *Last24: what I did on the last day, Next24: What I'm about to work on, HelpWanted: If you feel stuck on something and need help.*

* Master and Development branches will never be touched, just via pull request.

----

###<a name="git-conventions"></a> Git conventions.<sub><sub><sub><sub>[Index](#index)</sub></sub></sub></sub>

One of the most important things inside a project development are the conventions.
Any convention given, should be followed by any member of the team to improve the
integration and code quality.

Talking about **git** we will work considering the next rules:

* All the commit messages must have a reference to an issue: This is important because when you track an issue, you can just click on the reference and all the commits associated will appear on the github page.

* The commit messages will always be in english.

* The commit messages should be short, no more than 60 columns. If needed, you can add a description on a new line. Read the next [link](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message) for more information.

* The Pull request must not contain more than 20 files to be reviewed.
> _In case you're working on front-end and a lot of resources like images, fonts, etc. we will understand. Otherwise, the team must evaluate if the PR can be accepted._

* We encourage to follow the next branching convention.
  * Core: In case you're modifying something related with the core of the application, kinda critical and implies a test.
  > core/NumberOfIssue-InWhatIAmWorkingOn-whoIsworking

  e.g:
  > core/12-RefactoringLoginProcess-jresendiz

  * feature: Related with a user store or feature. A description must appear on the github issue, implies tests.
  > feature/NumberOfIssue-WhichUserStorieOrFeature-whoIsworking

  e.g:
  > feature/3-AdminCanLinkFacebookAccount-jresendiz

  * bugfix: When there's a bug or unexpected behavior that must be changed. Sometimes implies a test, depends on the bug.
  > bugfix/NumberOfIssue-WhatIsTheBugAbout-whoIsworking

  e.g:
  > bugfix/3-AdminCantLoginAfterFacebookAccountWasLinked-jresendiz

----
###<a name="pull-requests"></a> Pull Requests and Reviews.<sub><sub><sub><sub>[Index](#index)</sub></sub></sub></sub>

When there's a pull request, feel free to ask whatever you feel doubtful about. It's important to improve our code quality and a way to proceed it's having code reviews before and during the pull request. Ask to your partner for a review.

You can post your doubts on a pull request if needed or in the issue related in github.
