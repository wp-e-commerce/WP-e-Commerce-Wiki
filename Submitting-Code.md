First you need to prepare a local git copy of our repo as well as your fork.

<a name="setting-up"></a>

# Setting up your local repo

You can skip this section and proceed to [Submitting Pull Request](#pull-request) if you already got this under control.

## Clone WP e-Commerce to your local machine
Use either of these commands:

	git clone git@github.com:wp-e-commerce/WP-e-Commerce.git
	git clone https://github.com/wp-e-commerce/WP-e-Commerce.git

## Fork and fetch
Fork WP e-Commerce by clicking that little "Fork" button on the top right of this page. Now you'll have your own {your_user_name}/WP-e-Commerce repo on GitHub.

Now you need to add your fork as a "remote" source on your local copy of WP e-Commerce. Use either of these commands, replacing "your_user_name" with, well, your user name.

	git remote add my_fork git@github.com:your_user_name/WP-e-Commerce.git
	git remote add my_fork https://github.com/your_user_name/WP-e-Commerce.git

Confirm that your fork is added as a remote source by running `git remote`, you'll see `my_fork` is listed along with `origin`.

Fetch your fork's contents so that it's all present in your local index.

	git fetch fork

Confirm that all of your fork's branches and tags are all tracked by running `git remote show my_fork`.

# <a name="pull-request"></a>Submitting pull request

Once you're done preparing your local copy, you're ready to make your change and submit a pull request.

## Step 1: Make your change
If you follow the instructions in [Setting up your local repo](#setting-up) section, the "origin" (which is the official WPEC repo) and "fork" (which is your fork) should be in the same local git repo. The command line examples below continue from that setup.

Now you need to create a local master branch for your fork's `master` branch:

	git checkout remotes/my_fork/master
	git checkout -b my-fork-master
	git branch --set-upstream my-fork-master fork/master

Before making any changes, it's recommended that you create a local branch off your `master` and make your changes there. This should be repeated for each code change proposal or task you want to tackle.

	git checkout my-fork-master
	git checkout -b improve-coupon-ui

By doing that, you'll automatically be on fork-improve-coupon-ui branch. Make your changes and commit there.

When you're happy with your changes, push it to your remote fork:

	git push -u my_fork improve-coupon-ui

## Step 2: Submit a pull request.

Before submitting a pull request, please make sure there's already an issue ticket detailing the reason why you want to make this change. If there isn't, you need to create it. Basically we need to separate the comments on the issue and the comments on the pull request.

Once that's done, go to the [official WPEC GitHub page](https://github.com/wp-e-commerce/WP-e-Commerce) and click the "Pull Request" button. `base repo` should be set to the official WPEC repo, and `base branch` should be set to master. `head repo` should be your fork, and `head branch` should be the branch that you made your changes on.

When specifying the title and description of the pull request, make sure you refer to the issue ticket explaining the reason why this change needs to be made (e.g. #2611 to refer to issue number 2611).

## Step 3: Maintain the pull request

Once the pull request has been submitted, you'll need to monitor the original issue ticket and the pull request itself for comments and reviews. If you need to make further changes to your pull request, just commit as usual to your local fork's branch and push it to your fork. The new commit will show up automatically.

Also, you need to make sure your local copy as well as fork is always updated with latest changes in the official WPEC repo. Do these maintenance routines regularly:

Run these to pull updates to your origin's master branch:

	git fetch origin
	git checkout master
	git rebase origin/master

Then run these to update your fork:

	git checkout my-fork-master
	git rebase master
	git push fork master

Finally, rebase your pull request's branch upon your latest fork's master (or origin's master, either way is fine):

	git checkout improve-coupon-ui
	git rebase master