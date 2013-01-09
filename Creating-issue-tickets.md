This document provides guidelines for creating issue tickets. In order to save time and facilitate communication between community members, please adhere to these guidelines as much as you can.

There are 3 types of issue tickets: **bug report**, **enhancement**, **feature**.

A feature ticket is a proposal for a completely new feature that was not present before.

The difference between bug report and enhancement tickets is a bit more subtle. When you encounter a feature that doesn't behave like you want it to, you should ask yourself, _"Does this feature work for me at all?"_. If the answer is a decisive "Nope!", then it is most likely a bug report that you need to file. If the answer is "Well, sort of, but I want this and that", then it is more likely an enhancement that you're requesting.

# When reporting a bug
Sometimes when your site has multiple plugins, an issue might appear to be caused by WPEC but instead caused by another plugin, or even your theme. So first let's make sure the bug is not caused by any other plugins:

1. Switch off all other plugins on your site, except from WP e-Commerce.
2. Switch your theme back to TwentyTwelve.
3. Try to reproduce your issue again. If the issue persists, then it's a bug with WP e-Commerce. Otherwise, proceed to step 4.
4. Try re-enabling other plugins one by one and attempt to reproduce the issue to pin point which plugin is causing it.

Once you make sure it's definitely a WPEC issue, please make sure your bug report answers the following questions:

1. What are the steps that would trigger the issue you're having?

2. What's the expected outcome or results?

3. What do you see instead?

4. What are your site's configuration?
	* WPEC version
	* WordPress version
	* Are you running Apache, IIS or Nginx?
	* Are you using WordPress multisite?

5. Do you have a screenshot (or better, a screencast)? Do you have a public URL where we can examine the issue?

If your bug report doesn't answer these questions, we can't guarantee that your bug report will get much traction. We need to be able to reproduce your bug under our testing environment in order to debug it, so we need as many details as possible.

# When requesting an enhancement or proposing a feature

Please make sure you explain the reason why you think this enhancement or feature would benefit the majority of WPEC users. A screenshot, mock-up, wireframe, or any illustration at all is most welcome.