---
layout: post
title: Automatic Synchronization of 2 Git Repositories
categories: git
---

## Setup Git Repository on GitHub

[...]

## Mirror Git Repository on GitHub

Set up a (bare) mirror of the source repository:

```
$ git clone --mirror git@github.com:github_user/project.git
```

To fetch the changes:

```
$ cd project.git
$ git fetch -q --all -p
```

Make fetching automatic by installing a `cron` task:

```
$ sudo vi /etc/cron.d/sync_git_repos

*/5 * * * * app cd /path/to/project.git && git fetch -q --all -p
-or-
*/5 * * * * app cd /path/to/project.git && git fetch -q
```

`*/5` in the last line defines the minute at which the synchronization takes place, for example,
`*/2` would cause the sychronization to take place every two minutes. `*/5` causes the synchronization
to take place on minutes divisible by 5 (5, 10, 15, etc.)

## Sync Mirror to Git Repository on GitHub

The easiest way for that is to add a `post-receive` hook to local repository, it will automatically
push to GitHub each time you push to your repository.

If you don't have a post-receive hook yet, copy the sample over:

```
$ cp hooks/post-receive.sample hooks/post-receive
```

This file must be executable!

```
$ chmod a+x hooks/post-receive
```

Then add the following lines to `hooks/post-receive`

```
#!/bin/sh
#

git push --quiet origin &
```

## Reference

- http://www.pragmatic-source.com/en/opensource/tips/automatic-synchronization-2-git-repositories
- https://www.chiliproject.org/projects/chiliproject/wiki/HowTo_mirror_your_git_repository_on_Github
- https://support.asperasoft.com/hc/en-us/articles/216127358-How-to-run-a-cron-job-every-5-minutes
