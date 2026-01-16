# Extension Proposal

### Current State:

With the current state of the project, it is going well as most steps/features from the previous assignments have been implemented and should be in the excellent category. Although, this wasn’t completed within the deadline weeks.

### Release-Engineering Related Shortcoming:

One of the release-engineering related shortcomings that will be discussed in this extension proposal is the **contribution process** for this project. Although we all contribute effectively, we do not finish our tasks within the assigned week. The work tends to get completed the following week, which is good, but then for the next assignment the same issue occurs. The reason why this is considered a release-engineering shortcoming is due to the following:

- The contribution process slows down integration in which many features/steps have missed deadlines, increasing the workload. As mentioned in the paper by Fowler [1], the integration hell, which is the late integration by the developers, can cause conflict to be resolved with more difficulty leading to the code being unmerged.
- This can create merge conflicts, which has happened for assignment 2. These merge conflicts can delay further work done by each person which slows work. According to Fowler, the research and industry practice show that infrequent integration increases merge conflicts compared to frequent merges.
- The contribution process can lead to low-quality or inconsistent code.
- Sometimes reviewers don’t review the PRs open, which leads to the problems mentioned above. Code review being done in an orderly manner is prominent for maintaining the code quality, as if not done so, it can slow integration and feedback loops [2].

### Extension

A proposed extension connected to the shortcoming discussed above is the following:

- Implementation of a CI checkpoint: A mini-deadline can be added every mid-week in order to have some work done and it will be automatically checked. As mentioned above, with a more frequent integration checkpoint, this aligns with the CI and CD principles in which this ensures that the “integration hell” is reduced and the feedback speed improves [1].
- Add automation such that it requests reviewers automatically every time a PR is opened.

With these extensions to improve the shortcomings, adding automation is great as in many companies automation of notifying reviewers every time a PR opens supports a good practice for efficient code reviews [2] since this ensures every reviewer knows a PR has been opened leading to them being merged and there being fewer merging issues. With more frequent integration, this reduces the delays in reviews and the feedback received will be more efficient, which leads to a much better delivery performance.

### Expected Outcome

The proposed extension will lead to a reduction of late merge integrations in which this will limit the number of merge conflicts and overall ensure no pull requests are left open for long duration. With the mid-week CI checkpoint, the code changes are integrated earlier and limit merge conflicts. The automation to make reviewers check the open PRs improves responsibility and reduces delays.

To evaluate the extension, an experiment can be conducted by comparing objective metrics before and after the added extension proposal. Some metrics can be the average pull request, number of merge conflicts in each assignment and the time it took to approve merge requests. A drawback can influence whether this experiment can be conducted or not which will be explained in the drawback section.

### Experiment Design

In order to evaluate the extension, an experiment is designed by comparing before and after the extension was proposed. This can be done by comparing one week with and without the CI checkpoint and the automated review.

The following metrics would be collected:
- Average pull request
- Number of merge conflicts
- The time it took to merge after a PR is opened.

### Drawbacks

The possible drawbacks are that the mid-week deadline can add more pressure to the developers which can already impact the code quality and workload especially with different course deadlines coming. Adding automation can increase computation usage. As mentioned above, due to more pressure from other work, this can lead to pull requests being merged without reviewing it properly to meet the midpoint deadline which reduces code quality.

### Sources

1. M. Fowler, “Continuous Integration,” martinfowler.com, Jan. 18, 2024. https://martinfowler.com/articles/continuousIntegration.html  
2. “Code Review Developer Guide,” eng-practices. https://google.github.io/eng-practices/review/
