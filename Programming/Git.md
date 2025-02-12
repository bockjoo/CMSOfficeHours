# Git

Powerful version-control software.

Check out this [nice tutorial](https://www.sbf5.com/~cduan/technical/git/git-1.shtml).

## Let's 'Git' Started

```bash
mkdir <project_name>
cd <project_name>
git init

# Add some files... make some changes... Then:
git add <file1>

git commit -m "My first commit"  # Useful to make
git commit -a    # Adds all modified files, but not new ones.

git log    # Shows a log of your commits.
git log --graph --oneline --all    # Visually see the tree of commits.
git status
git diff <file>
git mv <file>    # Rename files.
git rm <file>    # Remove files.
```

General workflow:

1. Do some programming.
2. git status to see what files I changed.
3. git diff [file] to see exactly what I modified.
4. git commit -a -m [message] to commit.

branch==head (almost!)

`HEAD` (in all caps) refers to the most recent commit in the branch.

- FIXME: or does HEAD mean current commit?

A general head is a reference to a commit object. It's like a pointer.

When you do `git checkout <head-name>` you are pointing HEAD to the commit object <head-name>

- it is almost always best to git add and git commit before checking out! Otherwise git will complain.
- The important point here is, save your changes (commit them) before moving away from this commit/branch/head.
- Similarly for before merging... COMMIT YOUR CHANGES!

```bash
git branch    # shows existing heads, with a star at HEAD.
git branch -r    # Show remote branches.
git fetch <remote> <branch>  # Fetch a specific remote branch into your local.
# NOTE: if for some reason the above command doesn't retrieve the branch, do:
# git remote update origin 
git diff <head1>..<head2>    # Shows diff between commits ref'ed by <head1> and <head2>
git diff <head1>...<head2>    # Shows diff between <head2> and the common ancestor of <head1>,<head2>
git log <head1>..<head2>    # Show change log between <head2> and common ancestor

git config merge.renameLimit 999999
```

## A Good Workflow

Keep master branch in a stable, releaseable state.

- Make new branches to play with new features.
- Merge new feature branches into master when ready.
- After merging, you should delete the branch: `git branch -d <head>`
- Remember: "commits are cheap"

### Merge Branches

```bash
git checkout <stable_head>
git merge <feature_branch>
```

Collaboration:
git clone <remote>    # Copies a repo.

- Also copies all commit objects!

### Fetch Branches

Download objects from another repo, without *making* commits.

```bash
git fetch <remote_repo_ref>
# Retrieves all commits from remote repo, but DOES NOT move your heads.
# I.e., downloads all the recent changes, but will NOT put it in your current checked out code (working area).

# Fetching is more gentle than pulling, which updates your heads:
git pull <remote_repo_ref> <remote_head>
# Retrieves remote commits AND updates heads. 
# The remote heads usually start with origin/<head>
- `git pull` automatically does a `git fetch`

# Grab a specific file from a GitHub repo:
git fetch
git checkout <remote>/<branch> -- <path/to/local_file>   # Super useful!
```

### Stash Changes

You can stash (**store**) your recent changes without making a commit:

```bash
git stash        # Save modifications for later (e.g. now you can `git pull`).
git stash apply  # Bring those saved modifications back to life!

# Other `stash` commands:
git stash save "Message"     # Put in stash.
git stash list               # Show stash.
git stash show stash@{0}     # Show stash stats.
git stash show -p stash@{0}  # Show stash changes.
git stash pop stash@{0}      # Use custom stash item and drop it.
git stash apply stash@{0}    # Use custom stash item and do not drop it.
git stash apply --index      # Use custom stash item and index.
git stash branch new_branch  # Create branch from stash:.
git stash drop stash@{0}     # Delete custom stash item.
git stash clear              # Delete complete stash.
```

`git push` does the opposite of `git pull` and tries to add new commits to the remote repo. 
- Also updates remote head to point to same commit as local head.

"fast-forward merge" just means that the updates were linear. 
- i.e. the commit being updated just moves forward by X commits, with no branching involved.

You can add a new branch to the remote repo:
`git push --set-upstream origin new-branch`
`git branch --track`    # Not sure how to use this.
`git branch --set-upstream`    # Not sure how to use this.

Delete a branch on remote repo:
`git push [remote-repository-reference] :[head-name]`

Rebasing - an alternative to merging.
`git rebase <commit>`
1. Collect all commits between HEAD and common ancestor of <commit> and HEAD. 
2. Put HEAD at <commit>
3. Apply all collected commits to <commit>
- This makes it look kind of like a fast forward merge.
- Essentially just scoops up the commits along this branch and attaches them to <commit>

Suppose you want to make your master branch look just like remote master branch:
git checkout master
git reset --hard origin/master
- WARNING: this may also affect your other local branches...?

Ultimately, a Git repo is a tree of commits.

A tag is a stable release of your code. 
- You are tagging a specific commit object so you can always go back to it for stability.
- Think of it like a new master branch for a version of your code. 

Make a tag:
git tag -a <tag_name> -m "<some message about it>"

WARNING: Don't name your tag the same as one of your branches.

Delete a local tag:
git tag --delete <tagname>
Delete a remote tag:
git push <remote_alias> :ref/tags/<tag_name> 

## Github Markdown

```
*<words>*	or 	_<words>_		# make <words> italic		(called "emphasis")
**<words>** or 	__<words>__		# make <words> bold 		(called "strong emphasis")
**<words> and _<newwords>_**	# <words> and <newwords> 	(called "combined emphasis")
~~<words>~~ 					# make <words> strikethrough
{code}<words>{code} 			# make <words> monospace and code-like
!!<space>						# make entire message monospace by beginning message with '!!' and then a space!
@@<space>						# ignore all special formatting by beginning message with '@@' and then a space

Code
`<code>`			# inline <code>

```python
<code>
```				# block <code> with python syntax highlighting

Headers
# H1			# biggest text (used for headings)
## H2
### H3
#### H4
##### H5	# smallest text
###### H6	# smallest text, but greyed out

Lists	('⋅' is a whitespace)
1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list. 
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
⋅⋅1. Second item in the sub-list. Remember, GitHub Markdown has automatic numbering
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅	# two trailing spaces keeps you in same paragraph
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅

Unordered Lists
* Unordered list can use asterisks
- Or minuses
+ Or pluses

Tables
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| *zebra stripes* | `are neat`      |    $1 |

- Colons can be used to align columns.
- There must be at least 3 dashes separating each header cell.

Blockquotes			# look like quotes from a forum or email
> <quoted_text>

Hyperlink:
[<words>](<URL>)			# inserts a hyperlink at the string <words>

Image:
![<Image>](<URL>)			# inserts an image

You can also add: Images, Hyperlinks, inline HTML, and YouTube videos


Make a horizontal line (all methods are the same):
*** 	or 	___ 	or 	---	
```
