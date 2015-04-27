# Just enough Git to be (less) dangerous

Git. It's something you may have heard about, and perhaps even used -- but do you know how it works?
If you're using the Github GUI, probably not. So here, you are going to learn just enough Git to be (less) dangerous.

This is going to be kept somewhat brief and should clear up the [Git documentation](http://git-scm.com/docs) just enough to make it a bit more comprehensible. It's extremely thorough, but also overwhelming if you haven't already read half of it.


## WTF is even going on here
First, it's helpful to understand what Git is. Git is a DVCS (Distributed Version Control System) which means it remembers the history of your files and you can do it without being online. Git was invented by the same madman that gave us Linux: Linus Torvalds.

The great secret to understanding Git, that will probably make your eyes go wide and an "ahh" noise come out of your mouth, is this:

**Git is just a bunch of tools for emailing patches around**

No, seriously. There are like five commands for this express purpose. There's even a subheading in the docs: am, apply, format-patch, send-email, request-pull. If you check out the Linux kernel mailing list, this is still how they do things, with Git handling all of the boring work. Even the [git manpage](https://www.kernel.org/pub/software/scm/git/docs/) describes it as a "stupid content tracker".

Git is a collection of one-off tools for solving a particular problem, which is probably not the problem you're trying to solve.

Let's examine the Git model with this in mind.

## Commits

A commit is a patch. That's it. It lists some changes to some files, in a "unified diff" format. It also has some headers, which look *suspicously a lot like email headers*. There's an author, timestamp, and some other stuff.

Here’s where the magic happens. Remember, a patch expresses differences between two sets of files. (Let’s call them “trees” — as in directory trees.) So if you email me a patch, I can’t do a lot with it unless we agree on what I’m applying the patch to. It might be helpful to specify, say, “apply this to the Linux kernel”. It might be even more helpful to specify “apply this to Linux kernel release 3.0.5”.

A Git commit encodes this by having a “parent” header, saying which commit this one should be applied on top of.

Ah, but wait, a commit is just a patch. How do you apply a patch to another patch? You can only apply a patch to a full set of files (tree). But after you do, you get a new full set of files (tree). So “commit” is also used to mean “the state of the repository after applying this patch”.

That still leaves you with a bit of a recursive problem. If you have commit C, and it says its parent is B… well, you don’t know what the state of the repository at B looks like until you apply that, so you need to look at its parent, right?

Right. Git history is really a long chain of instructions for how to recreate a codebase from scratch, one step at a time. Imagine it as a pile of patches, like an inbox on your desk. Some trivial history might look like this:

* Commit C: My parent is B. Add “three” to the end of the file “numbers.txt”.
* Commit B: My parent is A. Add “two” to the end of the file “numbers.txt”.
* Commit A: Create the file “numbers.txt” containing “one”.

Here, commit A is a little bit special, in that it has no parent. That means that its patch can only create new files — there are no existing files to change! Otherwise it’s just a commit like any other.

So you start out with nothing. Then you apply patch A, which gives you one. Then you can apply patch B, which gives you one two. And finally you can apply patch C, which gives you one two three, the state of the codebase as of commit C. (Git doesn’t literally do this every time, of course; there is plenty of smart caching and whatnot. But the model acts pretty much like this.)

Git’s documentation tends to draw history from left to right, so the above would be displayed as:

    A---B---C

Same idea, just written differently. It makes a little more sense if you imagine arrows: A → B → C.

In reality, commits are identified not by letters but by hashes, which look like 8edc525cd8dc9e81b5fbeb297c44dd513ba5518e but are generally shortened to 8edc52. You might think they’re called “hashes” because they are long hex strings that happen to look like SHA-1 hashes. Well yes but also they are literally SHA-1 hashes of the patch, including the headers. (And since the parent is one of the headers, the hash includes the hash of the parent, which included the hash of its parent, etc. It’s a long chain of hashing all the way back to the beginning. Just like Bitcoin!)

A nice property of this hashing is that an individual commit cannot be changed. You can’t go back and just quietly stuff a line into patch A, because that would change its hash, and B wouldn’t point at the changed A. If you wanted to update B‘s parent, that would change its hash, and on it goes. Once you have a commit hash, you can be fairly confident that its existing history is immutable.

## Trees

I snuck this term into the above section because it’s an actual first-class thing in Git: a directory tree, containing some assortment of files. Every commit has an associated tree, which reflects the state of the repository as of that commit. Trees are identified by hashes, too.

You very rarely have to know or care about trees — after many years of using Git I’ve thought about trees, like, twice. They’re just an implementation detail. I mention them only because the documentation mentions them, and it’s nice to be able to understand what the hell the documentation is talking about. They come up in two (practical) places in the docs.

1. Some commands are documented as taking a “tree-ish” argument, e.g. using git checkout to check out individual files. This just means “something that Git can get a tree out of”. Since every commit has a tree, you can just use a commit.
2. There are a lot of references to the “working tree”. This is just the tree that you’re working in, i.e. the actual copy of the codebase that’s sitting on your hard drive.

And that’s all you need to know about trees!

## Branches

If you’re used to Mercurial, forget about Mercurial branches. I don’t know how they work, and Mercurial users have told me they’re such a pain in the ass that no one really uses them any more anyway.

Instead, consider the plight of our wireless driver dev from before. They want to change the kernel code to add their driver, but to create a patch when they’re done, they also need a pristine copy of the code. A patch lists differences, so you need two things to make a patch.

Well, no problem. When they first download the code, they can stick it in a directory called master (because it’s the master copy). Then when they go to work on their driver, they can copy the entire thing into a second directory called terrible-broadcom-driver. To generate the patch, they just diff the two directories.

That’s Git branches in a nutshell.

Note that with this approach, no one else knows or cares about your branch names. After all, you’re not sending anyone else your entire directory; you’re just sending them some patches. The patches don’t contain the branch name; they only know their parents.

More technically, a branch is just a name that points to some commit. (Literally, nothing else. A branch foo is a 41-byte text file containing a commit hash.) However, a branch has the special property that if you make a new commit while that branch is checked out, the branch name will be bumped to point at the new commit. Again, this works just like the manual example: if you do some work or apply a patch in your terrible-broadcom-driver directory, obviously the new contents of the directory will reflect the new change.

This is why Git is said to have “cheap local branching”. It’s cheap because a branch is nothing more than a name; it’s local because you aren’t forced to keep your branch names synchronized with anyone else’s.

Branches add a new wrinkle to our model: now history doesn’t have to be linear. Two different patches can have the same parent! Of course, you don’t really need branches for that — if two people check out the Linux kernel and both make a change, they’ll both produce patches with the same parent. Speaking of which…

## Remotes

“Remote” means “somewhere else”, so naturally a Git remote is just a Git repository that exists somewhere else. Usually this is a central server, but it doesn’t have to be; you can work without a central server at all, where several devs just list each other as remotes.

When you first clone a repository, the place you cloned from is set up as a remote called “origin”, because it’s the origin of your code. Whoa.

You’ll also get all of origin’s branches. Well. Sort of. Branch names are local, remember. If your origin has a branch called foo, Git will create a branch for you called origin/foo (called a “remote-tracking” branch). And since you didn’t create it, it doesn’t show up in git branch by default.

You usually don’t want to check out remote branches directly, anyway.

## Merging

Say you’re our kernel developer again. You grab the kernel, which is currently at state B. You write your driver, and send off a patch. But wait! In the meantime, other people have already had their patches applied!

So now you have: A → B → C

But Linus has: A → B → D → E → F

Or, to draw it the way the Git docs would, where time flows from left to right (ignore the period -- it's there only for formatting):

    .     C            terrible-broadcom-driver
         /
    A---B---D---E---F  origin/master

Well hey, no problem. The Linux kernel is a huge project, so chances are, none of those new patches touched any of the same files as you.

If we were emailing patches around, we might just say screw it and apply C on top of F, even though it says it belongs on top of B. But in the Git model, a commit is identified by its hash, which includes its parent. Changing the parent would require creating a new, different commit, with a different hash.

Instead, Git can just merge these two different views of history together, by creating a new commit with two parents: C and F.

    .      C-----------.    terrible-broadcom-driver
         /             \
    A---B---D---E---F---G  origin/master

If none of the changes from either side trample on each other, it’s a “trivial” merge. Since nothing new actually changed, the patch in G is empty; it only exists to glue C and F together.

If both sides changed the same parts of the same files in different ways, you have a merge conflict, and you’ll have to specify which side wins. Any change you make then becomes part of the patch in G.

## Tags

Tags are names for commits, rather like branches. However, tags are intended to be permanent: they’re mostly using for marking version releases. You can check out a tag, but a tag can’t be your “current branch”, and a tag will never automatically bump when you make a new commit.

Also, tags are (mostly) global, not namespaced like branches. They don’t change, after all, so it’s generally assumed that everyone should agree on what they point to.


# Okay that's great but how do I do anything
What a good question. There are already [plenty of cheat sheets](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) in the world but here is mine, assuming you want minimal interaction with Git.

## Get some code

git clone https://github.com/some_jerk/bad_code will dump that jerk’s bad code into a new bad_code directory.

When you want to update it, you can git pull origin master, which will fetch all the changes and attempt to merge them into your current branch. If you haven’t changed anything, this will just bump your checkout to what’s current.

If it’s an old checkout and you don’t remember whether you’ve done anything, you might want git pull --ff-only origin master, which will only do anything if the pull would be a “fast-forward”. That just means that your side hasn’t made any commits and no merge is necessary. In other words, if you have A and your origin has A → B → C, that’s a fast forward, because Git only has to pile more commits directly on top of the ones you already have.

## Look at stuff

git log will show you a log. The format is kind of verbose and not great for skimming history.

git log --oneline --graph --decorate is a lot nicer at a glance. You might also want to just install tig, which is basically the same thing but you can press Enter on a commit to see a diff right there.

git log --follow <path> shows you a log of only changes that touched a particular file (or directory). The --follow means to follow the file’s history across renames, and only works for a single file.

git show <commit> shows you the patch introduced by a commit. git show <commit>:<path> shows you the state of a file as of a particular commit.

## Actually use this damn thing to make this damn patch for this damn project

git status tells you the current state of your codebase: the branch you’re on, the changes you’ve made, etc.

git branch <name> creates a new branch based on the commit you have checked out, but doesn’t switch to it. You generally want something like git checkout -b <name> origin/master instead, which creates the new branch based on origin/master and also checks it out.

git checkout <branch> sets your current branch and checks out that state of the codebase. You can also check out a remote branch, a tag, or an individual commit, but these will unset your current branch, and you’ll get warnings about having a “detached HEAD”. That literally means HEAD doesn’t point to a branch, and if you make new commits, they won’t have anything pointing to them and will be easy to lose.

git add tells Git about new files you’ve created that you’d like to have in the next commit.

git rm tells Git you intend to delete the file, and also deletes it physically. (Git will refuse if the file has been changed, so this is always reversible. Also you could just rm the file and git commit -a will pick up on it.)

git mv tells Git you’re renaming a file. (Note that Git doesn’t actually store renames; it guesses on-the-fly whether a file has been renamed.)

git commit -a will pop open a text editor to ask for a commit message, then make a commit from all the changes you’ve made to all the files Git knows about.

Something in the Git model I didn’t mention yet: there’s a thing called the “index” or “staging area” or sometimes “cache”. (I don’t know why it needs so many names.) This is what you’ve said you’re going to commit. When you use git add and friends, any changes to a file (or the whole thing, if it’s a new file) are staged and show up in yellow in git status. Unstaged changes are listed separately, in green. If you use a plain git commit without -a, only staged changes become part of the commit. This is pretty useful at times, because it lets you do a bunch of exploratory work and then parcel it into several commits for future archaeologists. (If you’re feeling fancy, look into git add -p.) But you can just use git commit -a every time if you want. Hell, you don’t even need git add; you can just pass a list of files to git commit.

## Okay now how do I get it anywhere
By pushing, which just means bumping one or more branches on a particular remote. Git will only let you do a fast-forward when pushing — you can’t even automatically merge with a push. If you try to push and get a complaint about “non fast-forward” push, you just have to pull first and then try again. (But chances are you’re using GitHub and pull requests, where you push to a private branch, and GitHub does trivial merges for you.)

git push <remote> <branch> will push your branch to a branch of the same name to a remote. If you’re using a GitHub fork, then you probably have a single remote called “origin” which is your fork, and you are probably just working on the master branch. So you can do git push origin master and everything will be okay.

You can also do git push bare, which will usually do something useful. The default behavior for this has changed a couple times in recent releases, so I’m out of the habit of using it, but the current behavior is pretty safe and is basically: push the current branch to a remote branch of the same name.

## Oh my god what have I done

You copied and pasted some asshole’s git invocation from Stack Overflow, and now everything is broken. Don’t panic! And definitely don’t paste some other asshole’s claimed solution.

If your working copy or index are just completely screwed up, you can use git reset --hard to undo all your uncommitted changes. But do not use it lightly, since it’s naturally a destructive operation.

If you were doing some interactive multi-step thing like git rebase or git cherry-pick and it’s gone horribly wrong, git status will tell you, and e.g. git rebase --abort will bail and put you back where you started.

If you think you’ve lost commits somehow, you might find them in git reflog.

Absolute worst case, you can always get your work out as patches with git show and start over with a fresh clone.

## Also, some syntax stuff

Anywhere you need to name a commit, you can use the name of a branch, because a branch always unambiguously names a commit. A tag also works.

HEAD is a special kinda-sorta branch name that just refers to whatever you have checked out right now.

There’s a whole bunch of syntax for specifying commits or ranges of commits. You can peruse man gitrevisions at your leisure. The most useful ones are:
* foo^ is the (first) parent of foo. Most commonly used as HEAD^. Note that ^ is special in a lot of shells and might need escaping.
* foo..bar is a range, and means everything after foo, up to and including bar.

There’s more of this in man gitrevisions, but honestly I’ve never used 80% of it.

A lot of commands can take both commit names and paths, which is a little ambiguous, especially since branch names can contain slashes. All such commands should respect the convention of using a -- argument to mean “everything after here is a filename”.

## Some assumptions you might make that you should not make

Git is not a friendly project management tool. Git is a stupid content tracker.

Rather, Git is a weird filesystem, and it has a bunch of tools like rm and ls. The lower-level you go, the fewer assumptions Git will make about what you’re trying to do, and the less it will try to stop you from doing something bizarre. If you take only one thing away from this post, let it be this: Git was designed for people who already 100% understood it — the people who wrote it. It’s getting better about this, but this is the reason for a lot of its rough edges.

For the sake of nudging you out of the weeds, here are some assumptions you might already be making that you should not be making:

* A commit doesn’t have to have one parent. It can have two (if it’s a merge). Or three, or more (if it’s an “octopus” merge). Or zero (if it’s an initial commit).

* You can have a remote that has zero commits in common with your repository. There’s nothing strictly defining two repositories as containing the “same” codebase, or enforcing that they never interact. It’s just usually not that useful. (One possible use case: I’ve merged two projects into the same repository, without losing any of the history, by adding one as a remote of the other and just merging their histories together.)

* Similarly, you can have two branches in the same repository that have zero commits in common. (Which means you can have more than one initial commit!) This is how GitHub stores “GitHub pages”: they go on a separate gh-pages branch inside your repository, with a completely separate history.

* Commits don’t know what branch they were created on. A branch points to a single commit; a commit doesn’t point to a branch, ever. You can guess well enough for most practical cases, though.

* Git tracks files, not directories. You can’t store an empty directory in Git. The usual workaround is to stick a zero-byte file named .keep or something in the directory, and commit that file.

* The documentation does not necessarily list switches, or command forms, or really much of anything in order of usefulness. For example, the most fundamental command is arguably git commit, and the third switch documented is -C which is some curious merge thing that I don’t believe I have ever used. -m, which allows you to write a commit message, comes in at sixteenth place.

# Footguns to beware

Git is basically a weird filesystem, and the Git commands are basically weird filesystem commands. Just like ls and rm are equivalently opaque if you don’t already know what they do, Git commands don’t have any obvious tells for when they’re dangerous or not.

So here are some things that are dangerous, and how to use them safely, or at least how to use them not quite as dangerously.

### git rm

Well, obviously. In this case Git is nice enough to refuse to remove a file that has uncommitted changes, so you *probably* won't do much damage with this.

### git checkout
git checkout switches branches, but on a more fundamental level, what it does is check out files. You can use it as git checkout [commit] -- <files...> to check out some files, as of a particular commit. The commit defaults to your current branch, so the way to undo a change you’ve made to a file (but not yet committed) is git checkout -- <path>.

But this is naturally a destructive operation, with no confirmation. So be very sure you’re checking out the files you think you are.

You might want to try passing the -p switch, which will interactively prompt you about discarding each particular chunk of each file. (Several other commands take -p, including git add, which makes it possible to make several changes to a single file and only commit some of them. Pretty slick.)

### git reset
“Reset” is a weird command. It basically adjusts the state of your current branch, the index, and your working tree.

The dangerous part is git reset --hard <files...>, which will discard your work with no prompt, just like git checkout. There’s no “dry run” option, either. Be very careful with it, and triple-check that you don’t have anything you want to keep first.

### git rebase
I don’t care what anyone says. Do not use anything that says “rebase” unless you understand exactly what you are doing.

“Rebase” is for editing history. But you can’t edit history, because of the whole hashing thing. Rather, git rebase creates a new history.

Say you have A → B → C, where C is your own commit, and B is the latest commit in origin/master. You go to push, and, oh no! There’s already a new commit D on the server. So you have this:

    .      .---C  master
         /
    A---B---D    origin/master

You could merge here… or you could do a rebase. Fundamentally, “rebasing” means recreating a commit with a different parent. Git will take the patch that’s in C, apply it on top of D, fix any line numbers and ask you to resolve any conflicts (just like doing a merge), and then create a new commit from the result. This can’t still be commit C, because the parent is part of the commit hash, and the parent changed. Instead you get a commit C’. (The new hash doesn’t necessarily resemble the old one in any way; the apostrophe, pronounced “prime”, is a convention borrowed from math.)

So now you have:

    .      .---C
         /
    A---B---D         origin/master
             \
              .---C'  master

Your commit used to be based on B, but you’ve rewritten it to be based on D. Hence, rebase. I guess.

Anyway, now you can do a regular fast-forward push to origin.

Notice that C still exists, but it no longer has a name. Git keeps dangling commits like this around for 30 days (visible in git reflog) just in case you make a mistake, and deletes them during garbage collection.

Rebasing can be very disruptive, and should not be done lightly. Definitely never rebase commits that you’ve already published in any way — if anyone else has work based on your original commit C, it becomes a huge pain in the ass to update their work to be based on C’ instead. And if they don’t, you might end up with both C and C’ in your history, or they might conflict with each other, or who knows what. I’ve seen misuse of git rebase turn a linear branch with four commits into a tangled mess of some fifteen commits all merged constantly with copies of each other.

And there are further complications, like: C might be more than one commit, C might include merge commits, you might edit C while you’re at it, etc. It can get pretty gnarly pretty fast, and it’s not necessarily obvious how to resolve a wacky problem if you’re not totally sure what Git is doing.


### --force
Usually seen as an argument to git push after a rebase. Be super duper careful, since this will blindly overwrite whatever is on the remote. If you’re reading this article you probably have no good reason to force-push. And if you think you do, you probably still don’t, because Git 2.0 has --force-with-lease which at least defends against race conditions.

###  Committing passwords, large files, etc.
Git history is a chain going all the way back to the beginning of time. If you commit passwords or very large files, they are in history forever. Even if you delete something, it lives on forever in your repository. Git repositories, as a general rule, do not ever get smaller. And everyone, as a general rule, grabs your entire history when they clone.

The only way to get rid of something for good is to effectively rebase your entire history starting from the bad commit. (You’d rebase that history onto the same point, but you’d edit or delete the bad commit. That would change the bad commit’s hash, so every subsequent commit would change, too.)

You really don’t want to have to do that. Not only is it a huge pain, but it requires coordination with everyone else using your repository — after all, they still have copies of the original history, which they’ll need to get rid of as well. Or you’ll end up with the old history merged back into the new history!


# That's all I got
This is not a Git introduction for people who expect to be using Git heavily in the near future; this is a pile of notes for diving in head-first and maybe getting some useful work done without reading piles of documentation first.

Regardless, I hope some of this is useful, or at least makes other Git resources more intelligible!

If you are just dying to learn more about Git, the Internet is overflowing with other people trying to explain it. I defer to [GitHub’s list of articles](https://help.github.com/articles/good-resources-for-learning-git-and-github/).
