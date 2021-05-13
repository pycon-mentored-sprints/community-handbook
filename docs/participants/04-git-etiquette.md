---
title: Git etiquette for contributors
---

## Pull Request Etiquette

### Why do we use a Pull Request workflow?

PRs are a great way of sharing information and can help us be aware of the changes occurring in our codebase. They are also an excellent way of getting peer review on the work that we do, without the cost of working in direct pairs.
**Ultimately, though, the primary reason we use PRs is to encourage quality in the commits that are made to our code repositories**

Done well, the commits (and their attached messages) contained within tell a story to people examining the code later. If we are not careful to ensure the quality of these commits, we silently lose this ability.
**Poor quality code can be refactored. A terrible commit lasts forever.**

### Git Flow for Pull Requests

Each project will have some specific conventions and processes. But in general the base Git flow for Pull Requests is:

1. Ensure you are currently in `main`.
2. → If working in a **fork**, fetch:

      ```sh
      git checkout main
      git fetch --all --prune
      git merge --ff-only upstream/main
      git push origin main
      ```
      → If working in a **branch**, pull:
      ```sh
      git checkout main
      git pull origin main
      ```

3. Create a new branch for your feature or bug fix

    ```sh
    git checkout -b branchName
    ```

4. Make changes with as many commits as necessary. The final commit should build and pass tests.
5. Make sure your branch sits on top of `main` (as opposed to branch off a branch). This ensures the reviewer will need only minimal effort to integrate your work by fast-forwarding `main`

    ```sh
    git rebase upstream/main.
    ```

6. If you have previously pushed your code to a remote branch, you will need to force push. If not, omit the `-f` tag.

    ```sh
    git push origin branchName -f
    ```

7. Open a pull request in GitHub from this forked branch. Once this has been merged to main, remember to clear out your branch locally

    ```sh
    git branch -D branchName
    ```

### What constitutes a good PR?

A good quality PR will have the following characteristics:

* It will be a complete piece of work that adds value in some way.
* It will have a title that reflects the work within and a summary that helps to understand the changing context.
* There will be well-written commit messages, with well-crafted commits that tell the story of the development of this work.
* Ideally, it will be small and easy to understand. Single commit PRs are usually easy to submit, review, and merge.
* The code contained within will meet the best practices set by the team wherever possible.

A PR does not end at submission, though. A code change is not made until it is merged and used in production.

A good PR should be able to flow through a peer review system easily and quickly.

## Submitting Pull Requests

### Ensure there is a solid title and summary

PRs are a Github workflow tool, so it's essential to understand that the PR title, summary and eventual discussion are not as trackable as the commit history. If we ever move away from Github, we'll likely lose this information.

That said, however, they are a handy aid in ensuring that PRs are handled quickly and effectively.

Ensure that your PR title is scannable. People will read through the list of PRs attached to a repo and must be able to distinguish between them based on the title. Include a story/issue reference, if possible, so that the reviewer can get any extra context. Include a reference to the subsystem affected if this is a large codebase.

Use keywords in the title to help people understand your intention with the PR, e.g. [WIP], to indicate that it's still in progress, so it should not be merged.

### Rebase before you make the PR if needed

Unless there is a good reason not to rebase - typically because more than one person has been working on the branch - it is often a good idea to rebase your branch to tidy up before submitting the PR.

Use `git rebase -i main # or another reference, eg HEAD~5`

For example:

* Merge `oops, fix typo/bug` into their parent commit. There is no reason to create and solve bugs within a PR **unless there is educational value in highlighting them**.
* Reword your commit messages for clarity. Once a PR is submitted, any rewording of commits will involve a rebase, which can then mess up the conversation in the PR.

### Aim for one succinct commit

In an ideal world, your PR will be one small(ish) commit, doing one thing - in which case your life will be made easier since the commit message and PR title/summary are equivalent.

If your change contains more work than can be sensibly covered in a single commit, though, **do not** try to squash it down into one. Commit history should tell a story, and if that story is long, it may require multiple commits to walk the reviewer through it.

### Describe your changes well in each commit

Commit messages are invaluable to someone reading the history of the code base and are critical for understanding why a change was made.

Try to ensure that there is enough information for a person with no context or understanding of the code to make sense of the change.

Where external information references are available - such as Issue/Story IDs, PR numbers - ensure that they are included in the commit message.

Remember that your commit message must survive the ravages of time. Try to link to something that will be preserved equally well -- another commit, for example, rather than linking to main.
**Each commit message should include the reason why this commit was made. Usually by adding a sentence completing the form 'So that we...' will give a fantastic amount of context to the history that the code change itself cannot**


!!!tip
    A properly formed Git commit subject line should always be able to complete the following sentence:
    If applied, this commit will `your subject line here`.

### Keep it small

Try only to fix one issue or add one feature within the pull request. The larger it is, the more complex it is to review, and the more likely it will be delayed. Remember that reviewing PRs is taking time from someone else's day.

If you must submit a significant PR, try to make someone else aware of this fact, and arrange for their time to review and get the PR merged. It's not fair to the team to dump large pieces of work on their laps without warning.

If you can rebase up a large PR into multiple smaller PRs, then do so.

## Reviewing Pull Requests

It's a reviewers responsibility to ensure:

* Commit history is excellent
* Good changes are propagated quickly
* Code review is performed
* They understand what is being changed from the perspective of someone examining the code in the future.

### Reviewers are the guardians of the commit history

The importance of ensuring a quality commit history cannot be stressed enough. It is the historical context of all of the work that we do and is vital for understanding why changes were made in the past. What is apparent now will not be evident 2-3 years in the future.

Without a decent commit history, we may as well be storing all our code in files ending yyyy-mm-dd. The commit history of a codebase allows people to understand **why** a change was made - the when what, and where are automatically evident.

When looking at a commit message, ask yourself the question - from the perspective of someone looking at this change without any knowledge of the codebase - 'do I understand *why* this change was made?'

**If any commit within the PR does not meet this standard, the PR should be rebased until it does. We cannot fix a commit history once it is in place, unlike our ability to refactor bad quality code or fix bugs.**

A helpful tip is to ask the submitter to add a sentence to the commit message completing the sentence 'So that we...'.

### Keep the flow going

Pull Requests are the fundamental unit of how we progress change. If PRs are getting clogged up in the system, either unreviewed or unmanaged, they are preventing a piece of work from being completed.

As PRs clog up in the system, merges become more complex as other features and fixes are applied to the same codebase. This, in turn, slows them down further and often completely blocks progress on a given codebase.

There is a balance between flow and ensuring the quality of our PRs. As a reviewer, you should decide whether a code quality issue is sufficient enough to block the PR whilst the code is improved. Possibly it is more prudent to flag that the code needs rework and raise an issue.

Any quality issue that will result in a bug should be fixed.

### We are all reviewers

To make sure PRs flow through the system speedily, we must scale the PR review process. It is not sufficient (or fair!) to expect one or two people to review all PRs to our code. For starters, it creates a blocker every time those people are busy.

Hopefully, with the above guidelines, we can all start sharing the responsibility of being a reviewer.

NB: With this in mind - if you are the first to comment on a PR, you are that PRs reviewer. If you feel that you can no longer be responsible for the subsequent merge or closure of the PR, then flag this up in the PR conversation, so someone else can take up the role.

There's no reason why multiple people cannot comment on a PR and review it, which is to be encouraged.

### Don't add to the PR yourself

It's sometimes tempting to fix a bug in a PR yourself or rework a section to meet coding standards, or make a feature better fit your needs.

If you do this, you are no longer the reviewer of the PR. You are a collaborator and so should not merge the PR.

It is possible to find a new reviewer. Still, generally, change will be speedier if you require the original submitter to fix the code themselves. Alternatively, if the original PR is 'good enough, raise the changes you'd like to see as separate stories/issues and rework in your PR.

### It is not the reviewer's responsibility to test the code

We are all busy people, and in the case of many PRs against our codebase, we are not able or time-permitted to test the new code.

We need to assume that the submitter has tested their code to the point of being happy with the work to be merged to main and subsequently released.

If you, as a reviewer, are suspicious that the work in the PR has not been tested, raise this with the submitter. Find out how they have tested it and refuse the work if they have not. They may not have a mechanism to test it, in which case you may need help.

If, as a submitter, you know that this change is not thoroughly tested, highlight this in the PR text, and talk to the reviewer.

## Don't forget to say thanks

Words of gratitude and praises are often the sole motivator that makes open-source authors keep going, so don't be shy and say it!

Do you want to see a pull request merged, a much-needed feature implemented, and a long-awaited release shipped? Instead of reminding them how much work is on their plate by asking "any news" or showing dissatisfaction (which only kills the motivation), say how their work is essential. Tell me how much you value them. Tweet kind words about them and their project. Sometimes that's the best you can do.

## Some exciting links

- Amazing article on h[how to effectively use the commit history as an incredible documentation source
](http://mislav.uniqpath.com/2014/02/hidden-documentation/)
- [Github's recommendations for PRs](https://github.com/blog/1943-how-to-write-the-perfect-pull-request), which has some useful tips on notifying teams and colleagues via the GitHub @-syntax.
- [The GOV.UK git styleguide](https://github.com/alphagov/styleguides/blob/main/git.md)
- [An example of a perfect commit message](https://github.com/gds-operations/vcloud-edge_gateway/pull/111)

---

Thanks to @mikepea and [their great PR etiquette resource](https://gist.github.com/mikepea/863f63d6e37281e329f8) which inspired loads of this section 🙏
