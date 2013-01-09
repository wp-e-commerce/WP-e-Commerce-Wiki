GitHub provides a built-in feature to merge pull requests automatically, however it is not recommended to use that feature for 2 reasons:

* **Merge bubbles are created.** These are the commit messages that go "Merge branch xxx of xxx" right after the last commit of the branch being merged with. These commits cluster up the history.

* **The commit timeline is messed up**.

Below is an example, where `fork` doesn't rebase itself upon the latest development on `origin`.

	A____B___C___D    origin
	 \_E___F___G      fork

This is what happens when you merge `fork` into `origin`:

	A____B___C___D__H     origin
	 \_E___F___G___/      fork

Note that `H` is the merge bubble that we described above. If we do a `git log`, here's the order of the commits we now have on origin: `A E B F C G D H`.

That looks really confusing, and you can hardly `git bisect` through that if that ever becomes necessary.

# The proper way to merge pull requests

This would require you to use the git command line.

Let's say you're merging a pull request from a fork: `JustinSainton/wp-e-commerce`, branch name is `ui-improvement`. From here on we assume that you set up your local copy [using the instructions in Submitting Code](wiki/Submitting-Code#wiki-setting-up). `origin` is the official WPEC repo.

First, you need to fetch the fork to your local copy

	git remote add justin-sainton-fork git@github.com:JustinSainton/WP-e-Commerce.git
	git fetch justin-sainton-fork

Checkout a copy of the fork's `ui-improvement` branch:

	git checkout -b pull-request-ui-improvement justin-sainton-fork/ui-improvement

You'll be automatically switched to the local branch you just created off of the fork's remote branch. Now, rebase that local branch upon the WPEC official `master` (in this case, is the `master` branch):

	git rebase master

If there's any conflict, resolve it. Now switch back to `master`, and merge with `pull-request-ui-improvement`

	git checkout master
	git merge pull-request-ui-improvement

You'll notice that this time, the merge will be "fast-forward", which means there will be no merge bubble, and the time line is preserved (i.e. `A B C D E F G`, and no `H`!).

Final step is push master to origin

	git push origin master