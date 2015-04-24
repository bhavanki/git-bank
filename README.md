git-bank
========

**A custom Git command for saving off diffs.**

*Yeah, I already have that, it's called git stash.*

Well, yes, that works fine, unless you want to hang on to the diffs for a long
time. Then you end up with a stack of diffs to juggle, and also because you
were probably lazy and didn't give a clear description of each one, you can't
tell which is which anymore. So, that's why you might want to use this.

## Installation

```
curl http://github.mtv.cloudera.com/bhavanki/git-bank/raw/master/git-bank > ~/bin/git-bank \
  && chmod +x ~/bin/git-bank
```

## Usage

Type `git bank help` for a summary.

The typical flow is to start with changes in your working tree that you want to
save off (or "bank") as a diff.

```
$ git bank save my-awesome-changes
Banked my-awesome-changes
$ git bank list
my-awesome-changes bank-f734188e44a8bfb1830d9f3afe1abda3f48551a5
$ git bank show my-awesome-changes
# ... contents of diff ...
```

The local changes are left in place in the working tree, so you can carry on
as usual or do some sort of `git reset` to do something else.

The diff is kept inside the Git repository as a regular old object. An internal
"ledger" maps from the friendly names you pick for the diffs to the tags
associated with the stored diffs. All these details don't matter too much,
except to point out that you get a separate bank for each Git repository.

Someday, when you want to apply a saved diff:

```
$ git bank apply my-awesome-changes
Applied my-awesome-changes
```

### About Untracked Files

New / untracked files aren't saved by git-bank, because `git diff` doesn't
include them. If you want to save them off, use `git add -N` on the files
first, but note that this breaks `git stash` while the empty blobs are there.

## Interesting Uses

* A tool expects a specific version of Scala, but you have a newer version
  installed and you keep having to update the tool before running it.
* You run tests from a different directory than usual, so you always have to
  update paths in the test configuration before you start.
* Any others you can think of? Let me know!


