+++
author = "Alcadramin"
title = "TL;DR; Ignore a tracked file in Git without removing it"
description = "Ignore a tracked file in Git without removing it"
tags = "Git"
series = "TL;DR;"
categories = "TL;DR;"
specification = "TL;DR;"
keywords = ["git"]
+++

So the other day when I was organizing my [Dotfiles](https://github.com/Alcadramin/Dots), I wanted to ignore some files with `.gitignore` however since it was tracked, it was not ignoring the files. (I want to file exists in Remote but I don't want to push changes because it contains sensetive data).

Found a neat way to do it:

- This command will manually ignore the changes:

```bash
git update-index --assume-unchanged <file>
```

- If you want to start tracking changes again:

```bash
git update-index --no-assume-unchanged <file>
```

> [Source](https://superuser.com/a/1655712)
