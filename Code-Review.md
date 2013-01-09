Any code that gets committed needs to be reviewed by at least a core committer who is not the person submitting the code.

Issue tickets with solutions will be labeled with `s:delivered`. This label means the solution awaits a core committer's review.

If the reviewer decides that the solution needs more work, the ticket will be labeled with `s:incomplete` or `s:in-progress`.

If, on the other hand, the solution is approved, then the core committer who reviews it will proceed to make a commit for it, or merge the pull request.

A few guidelines for code reviewer:

* First start with reading the code and try to detect as many bugs as possible. If bugs are found at this stage, you can skip the testing and give your feedback right away. If no bugs are found, proceed to actually test the code.

* Make sure the code submitted adheres to our [coding standards and code quality guidelines](wiki/Coding-Standards-and-Code-Quality).

* When reviewing a pull request, use the "Files Changed" tab to comment directly on the diff. By doing so your comment will appear along with the relevant code snippet.

* If after a week and there is no response from the code submitter regarding your feedback, feel free to take over, and clone his branch to your own fork. Or you can assign another core committer to it.

* Don't use GitHub automatic merging to merge pull requests. See [Merging pull requests](Merging-Pull-Requests) for an explanation and the proper way to do so.