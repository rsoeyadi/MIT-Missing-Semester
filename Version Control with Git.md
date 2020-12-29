Lecture 6 at 1:26pm on December 29th, 2020

---

# Version Control (git)

Version Control Systems - help track the history of changes to some set of documents and facilitate collaboration

Useful even if you are just doing work by yourself

files = trees

files = blobs

**Blob** is an abbreviation for “binary large object”. When we **git** add a file such as example_file. txt , **git** creates a **blob** object containing the contents of the file. **Blobs**are therefore the **git** object type for storing files.

Every state (of each project) points back to what preceded it

---

## It's not linear

you can work on different things in parallel (I.e., work on a bug in a project but also try to implement a new feature in a separate snapshot)

then you can combine snapshots together to support both the bug fix and new feature, for example

if there is a problem from merging two parallel branches of development, that's called a **merge conflict**

these snapshots are called **commits**

```
o <-- o <-- o <-- o
            ^
             \
              --- o <-- o
```

there merge would look like this:

```
o <-- o <-- o <-- o <---- o
            ^            /
             \          v
              --- o <-- o
```

Commits in Git are immutable. This doesn’t mean that mistakes can’t be corrected, however; it’s just that “edits” to the commit history are actually creating entirely new commits, and references (see below) are updated to point to the new ones.

---

## The Data Model as pseudocode

```
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

An “object” is a blob, tree, or commit:

```
type object = blob | tree | commit
```

In Git data store, all objects are content-addressed by their [SHA-1 hash](https://en.wikipedia.org/wiki/SHA-1).

```
objects = map<string, object>

def store(object):
    id = sha1(object)
    objects[id] = object

def load(id):
    return objects[id]
```

Blobs, trees, and commits are unified in this way: they are all objects. When they reference other objects, they don’t actually *contain* them in their on-disk representation, but have a reference to them by their hash.

## References

All snapshots can be identified by their SHA-1 hash (40 hexadecimal characters)

Git’s solution to this problem is human-readable names for SHA-1 hashes, called “references”. References are pointers to commits. Unlike objects, which are immutable, references are mutable (can be updated to point to a new commit). For example, the `master` reference usually points to the latest commit in the main branch of development.

```
references = map<string, string>

def update_reference(name, id):
    references[name] = id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id in references:
        return load(references[name_or_id])
    else:
        return load(name_or_id)
```

With this, Git can use human-readable names like “master” to refer to a particular snapshot in the history, instead of a long hexadecimal string.

One detail is that we often want a notion of “where we currently are” in the history, so that when we take a new snapshot, we know what it is relative to (how we set the `parents` field of the commit). In Git, that “where we currently are” is a special reference called “HEAD”.

---

## Repositories

Finally, we can define what (roughly) is a Git *repository*: it is the data `objects`and `references`.

All `git` commands map to some manipulation of the commit DAG by adding objects and adding/updating references.

---

## Command Line

to initialize, `git init` in the directory

what is going on right now? `git status` 

## Staging Area

what do you want to include in the next snapshot?

`git add ____`

`git commit`

we have `add` and `commit` because you might not want everything to be included into the new snapshot

`git log --all --graph --decorate` to visualize the commit graph

---

By convention, the default main branch is `master`; it's a pointer to commits

`HEAD` is a special reference used to refer to where you are currently looking (**the last snapshot**)

`git checkout` lets you move around in your version history

- it'll change the state of your working directory to what you asked for
- it **changes the contents** which can be dangerous

Therefore,

- `checkout` is changing where `HEAD` pointer is is and changes the contents

---

`git diff` can show you what has changed since the last snapshot

---

## Branches

`git branch` will show the branches in the current repository

`git branch [NEW BRANCH]` will create a new branch

`git checkout [NEW BRANCH]` will go to that new branch

now if you make changes, that new branch will be updated to a new snapshot, while the other branch (like Master) will still be pointing to the older snapshot

`git checkout -b [NEW BRANCH]` will create a new branch and switch to it, **in one command**

`git merge` to merge branches together

---

## Merge Conflicts

sometimes the developer needs to resolve merge conflicts

then you can merge

---

## Remotes

Git is aware of all of the existing copies of the repository

if you're collaborating with others, it can be aware of all the copies connected to a remote repository, like on Github

you can fetch changes as well from Github

`git clone <url> <folder name>` to copy from a certain branch

`git fetch` will get the updated repository to your local machine

`git pull` is `fetch` and `merge`

---

## Extra Tools

`git config` - you can configure git

`git clone --shallow` - you will have the **latest snapshot only**, not the entire repository

`.gitignore` to specify files that you don't want git to track 