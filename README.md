> This project's steps can be found on the [article](https://dev.to/christopherkade/up-your-git-game-and-clean-up-your-history-4j3j) or down bellow.

This repository will help you understand how rebase works by actually using it ! Don't worry if you make any mistakes, this is a clean slate, you can always re-clone it if you mess up.

## Example 1: fixing up a commit with rebase

**Scenario**: you have committed something that does not deserve a commit of its own, or you want to reduce the number of commits on your branch to one before making a pull request.

- From the `master` branch, create a new branch.

- Create a new file, its content doesn't really matter.

- Commit that new file to your branch.

```bash
git add index.js
git commit -m "add index.js"
```

- Update something in that file

- Commit it again with a message such as "update index.js"

- Run `git log`, as you can see, we now have 2 commits

We now want to `fixup` the `update` commit into the `add` commit, because this small change does not deserve a commit of its own.

To do so, we'll use the **interactive** mode of `git rebase`, which lets us apply the rebasing with a nice interface.

- Run the rebase command like so:

```bash
git rebase -i HEAD~2
```

`HEAD~2` means start from the last commit on the branch (the head) and go back 2 commits. If we wanted to manipulate more commits, we could change the value to the far right.  
You should now have an interface that looks something like this:

![rebase interactive](https://thepracticaldev.s3.amazonaws.com/i/h4po6e92b0icvrpki9sz.png)

Don't panic, this only shows you the two commits you are changing at the top, and the available commands bellow them.   
By default, the rebase interface uses Vim, to write in it, simply press the **i** key. You are now in **"INSERT"** mode. As we want to fixup the second commit in the first one, all we have to do is write `fixup` or `f` instead of `pick` in front of it. Our `update index.js` commit will now be squashed into the `add index.js` but only the `add index.js`'s message will be kept.

- Update the second line like so:

```bash
pick c0091ec add index.js
f a19336e update index.js
```

Now, we want to apply the rebase, press **escape** to leave the **INSERT** mode, press **:** (colon) and enter **wq** for "write" and "quit" and press **ENTER** to apply these changes. The colon simply allows you to write commands for Vim to execute. 

The following message should now appear in your console:

> Successfully rebased and updated refs/heads/{YOUR BRANCH NAME}.

Check your `git log`, you now have one beautiful and clean commit !

- Finally, force push to that branch to apply the rebase to the remote server

```bash
git push origin {BRANCH-NAME} -f
```

The `-f` is essential as a rebase modifies your git history and requires to be forced.

## Example 2: dropping a commit

These next 2 steps will be extremely similar to the first one because you now have the tools to do any kind of rebasing 🎉

**Scenario**: you want to completely remove a commit

We'll drop the `add FILENAME` commit we previously made:

- Run the rebase command

```bash
git rebase -i HEAD~1
```

- Add a `d` or `drop` in front of the commit you wish to drop.

![rebase drop](https://thepracticaldev.s3.amazonaws.com/i/nl7wqia23khpgf7by6jf.png)

- Run `:wq` in your Vim editor (and check with `git log` that the commit was dropped)

- Don't forget to force push it to your remote server 😀

## Example 3: rewording a commit

Pretty similar, with one change.

**Scenario**: you want to fix a typo or rewrite a commit's title or description

- Create a random commit

![git reword](https://thepracticaldev.s3.amazonaws.com/i/o88cmcyqj1596f52gyiy.png)

- Run the rebase command

```bash
git rebase -i HEAD~1
```

- Add a `r` or `reword` in front of the commit you wish to reword (no need to edit the title now).

- Run `:wq` in your Vim  editor. This will open a similar editor with the commit(s) you wish to reword.

![git reword 2](https://thepracticaldev.s3.amazonaws.com/i/ou14d5c0l0wisqix9wr2.png)

- Update the commit's title and description to your will, run `:wq` and that's it ! Check with `git log` that the rewording was applied

![git reword 3](https://thepracticaldev.s3.amazonaws.com/i/b1o6v4ichuc69ruig868.png)

- Don't forget to force push it to your remote server 😀

## Example 4: rebasing on `master`

> This example isn't reproduced in the Github project, but feel free to test it out.

**Scenario**: you have multiple PRs (Pull Requests) open at the same time

![rebase master screenshot](https://thepracticaldev.s3.amazonaws.com/i/ja8pn2wr7pl39yhaxf76.png)

You merge one PR, now, your second PR is not up to date with `master`, oh no !

![rebase master screenshot 2](https://thepracticaldev.s3.amazonaws.com/i/bl9r2aajzi2384qa4f9n.png)

This very frequent scenario will have us rebase our second PR on `master` so that it gets the new code merged from the first PR.

- From the branch you want to rebase (in our case, the second PR's branch), run the following:

```bash
git fetch
```

This downloads all the references our branch needs to apply the rebase. 

- Then, execute the rebase like so:

```bash
git rebase origin/master
```

- Finally, run a `git push origin {MY-BRANCH} -f` to apply the rebase to our remote server

![rebase master screenshot 3](https://thepracticaldev.s3.amazonaws.com/i/m220k16b638zl42xaqbo.png)

> Hurray !

