---
title: In defense of a force push
---

No man of scientific disposition discards something without a proper analysis.
The same holds true for the usage of `git push -f`, in this article I try to analyse the true nature of a force push.

<!--more-->

## Introduction

I recently had an argument with a colleague over the usage of `git push --force`.
I occasionally use the `--force` option and he remarked that it can be quite
destructive, this got me thinking about the dynamics of a force push.
A bit of background is needed to appreciate the arguments I am going to present,
once that is out of the way I'll present my discourse and leave you to judge
for yourself if you should force push or not.

## Our Git Workflow

I work as a front end developer in quite an eclectic team. A lot of us don't really
wish to be entangled with yet another piece of software and hence we neither have a common
workflow nor a set of git best practices. There have been a lot discussions but
nothing materializes. I love git and I love experimenting with it, on minor occasions
things did go south but they taught me to use the git effectively and also how
beautifully crafted git is. I have been a staunch proponent of a proper git
workflow and educating the masses about its utility.

### Our Git Usage

We have a branch _develop_ which is shared by the team and is used to make releases.
Everybody is free to push to it directly without any pull requests, automation
testing people, creative people, api people and front end people, all of them
cram their changes onto one repository. Features tend to be developed on their own
branch and merged later into the develop. Issues are fixed directly on the
_develop_ and merged into it directly.

### Problems with the above pattern
Doing `git pull` on local `develop` results in merges between `origin/develop`
and `develop`, not fast forwards. This results in a bad commit history.
If you just rebased a feature branch on develop, merged it with develop and pushed.
Guess what?, the remote refs won't be updated because somebody just pushed to
the remote branch. Now I have to either merge or rebase again.
Feature development takes weeks if not months and during which time both the
_fork of develop_ and _develop_ itself would have significantly diverged.
Dividing the feature branch into logical commits and rebasing on _develop_
is the one option and the other option is to merge and resolve the commits
manually. I prefer the former. If you're using git then your time spent on
resolving conflicts should be minimal, unnecessarily resolving merge conflicts manually is
not the git style.

All in all though the pattern is flawed not much is broken functionally. So, yeah,
though I dislike the pattern I can work with it. It atleast doesn't prevent me
from creating a workflow suited for myself.

## When do I use `--force`?

Before you bring out all the oft repeated advice that using `-f` option while pushing to
a public repo is a hideous abhorrent crime and the perpetrator must be hanged,
quartered and drawn, let me put up a humble plea.

I use force push when working on a feature branch and after rebasing with _develop_.
There is just no other way to rebase continually on _develop_ and not use `-f`.
I must `rebase` continually so that I don't have to deal with a slew of conflicts
later and also receive the fixes. `rebase` also gives me a very clear and simple
way of thinking about my changes, on top previous change.

But, the power of force push must be exercised with a lot of caution, you should
never force push to a remote branch which has been updated since you last fetched
it. So, to prevent myself from accidentally overwriting somebody's commits I use a
simple bash function. Before doing a force push I check if the branch ref that is
currently available and one that is fetched point to the same commit, if they
don't it means somebody has pushed to the branch, in which case I cherry pick
the newly made commit.

{% highlight bash linenos %}
function gpf() {
	## current commit pointed to by the branch ref
	c1=$(git log -1 origin/$1 --pretty=format:%H);
	## fetch the updated refs
	git fetch;
	## commit pointed to by the updated branch ref
	c2=$(git log -1 origin/$1 --pretty=format:%H);

	if [ "$c1" == "$c2" ]; then
		echo "Everything is alright. Force pushing.";
		git push origin $1;
	else
		echo "Somebody has updated the upstream branch. Rejecting!!.";
	fi
}
{% endhighlight %}

I also sometimes force push my own branch (I use it as a sort of remote backup)
when I have altered the commit history, so that they are logically ordered.
I've force pushed on shared branches, but only when the collaborator uses the same workflow as mine or knows how to handle it.
There is no precedent of me force pushing and some going haywire, not that it will stay for long, indeed there is a high chance
things breaking, but I can't think of any good reason __not to force push__ given the rules I have for it.

## Use `--force-with-lease`?

I found a very good article while researching for this post, the [article](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
obviates the need of a bash function like above. I kept the function since it
explains the purpose of the flag better.

## Notes
Note that even the git documentation agrees that if you rebase a published
branch then you do need to bypass the normal check applied by `git push`. The
text that I mention can be found in the documentation for `rebase` command and
option `--force-with-lease`.

## Resources
* [--force-with-lease](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
* [love for force pushing](http://movingfast.io/articles/git-force-pushing/)
* [git docs on rebase vs. merge](http://git-scm.com/book/en/v2/Git-Branching-Rebasing#Rebase-vs.-Merge)
