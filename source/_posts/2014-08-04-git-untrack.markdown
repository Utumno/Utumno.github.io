---
layout: post
title: "git untrack"
date: 2014-08-04 17:06:13 +0300
comments: true
categories: git
---

[A brief on ignoring files][1]. I wish it was as simple as that.

The biggest problem for me is when I want to ignore a file that was previously tracked - ignore it forever but have it _not_ deleted when someone pulls from the remote. This is not possible unfortunately - see the comments by torek starting here

And a [git assume-unchanged vs skip-worktree][2] post that details what happens to an `--assume-unchanged` file on pull etc. Also SO questions:

- [git assume unchanged vs skip worktree - ignoring a symbolic link][3] - see this answer for `core.sparseCheckout`
- [Git - Difference Between 'assume-unchanged' and 'skip-worktree'][4]
- [Who touched my git assume-unchanged bit?][5]
- [git-update-index(1) Manual Page][6]
- and a discussion on the mailing list with Hamano: http://thread.gmane.org/gmane.comp.version-control.git/146082
- a very interesting answer explaining differences between how git expands patterns passed to it - referred to as "pathspecs" - as opposed to how shell would resolve them (globs): stackoverflow.com/questions/2899875/git-add-not-working-with-png-files/2902126#2902126
- Finally, [how to have local versions of tracked config files in git][7]

Glob patterns to regexes:

- Java: http://stackoverflow.com/questions/1247772/is-there-an-equivalent-of-java-util-regex-for-glob-type-patterns
- Python: http://stackoverflow.com/a/1555769/281545

And btw [adding “.gitignore” to .gitignore works perfectly][8].

And git filter branch a file out: [`git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' HEAD`][9]


Bonus:
- squash last 3 commits: git reset --soft head~3 && git add -A && git commit -m "Squashed 3 last commits"
- [git rebase --onto][10] - should be using it more
- [.gitignore Syntax: bin vs. bin/* vs. bin/** vs bin/][11]


  [1]: https://help.github.com/articles/ignoring-files
  [2]: http://fallengamer.livejournal.com/93321.html
  [3]: http://stackoverflow.com/a/6139470/281545
  [4]: http://stackoverflow.com/questions/13630849/git-difference-between-assume-unchanged-and-skip-worktree
  [5]: http://stackoverflow.com/questions/7399153/who-touched-my-git-assume-unchanged-bit
  [6]: https://www.kernel.org/pub/software/scm/git/docs/git-update-index.html
  [7]: https://gist.github.com/canton7/1423106
  [8]: http://caiustheory.com/ignore-gitignore-in-git/
  [9]: http://stackoverflow.com/a/1274126/281545
  [10]: http://git-scm.com/book/en/Git-Branching-Rebasing
  [11]: http://stackoverflow.com/questions/8783093/gitignore-syntax-bin-vs-bin-vs-bin-vs-bin/20391855#20391855
