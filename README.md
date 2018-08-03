## Swift functions to get object address
```swift
func addressOf(_ o: UnsafeRawPointer) -> String {
    let addr = unsafeBitCast(o, to: Int.self)
    return String(format: "%p", addr)
}

func addressOf<T: AnyObject>(_ o: T) -> String {
    let addr = unsafeBitCast(o, to: Int.self)
    return String(format: "%p", addr)
}
```

## Unhide hidden files
```
```

## Delete branches that don't exist on origin anymore
```
git fetch --prune; git branch -vv | grep "\[origin.*: gone\]" | sed 's/.*\[origin\/\(.*\): gone\].*/\1/g' | xargs git branch -D
```

## Delete all branches except current one
```
git branch | grep -v $(git rev-parse --abbrev-ref HEAD) | xargs git branch -D
```

## Prefix commits with JIRA ticket id
Must have branch name with `XX-####` in it. E.G. `tsp/TX-1234/adding-tests`, `tsp/TX-1234`
```
# Put this in the commit-msg hook
# If commit message is a fixup message, ignore it.
[[ -n "$(cat $1 | grep 'fixup!')" ]] && FIXUP="YES"

TICKET=$(git symbolic-ref HEAD | rev | rev | grep -o -E "[A-Z]+-[0-9]+")
if [[ -n "${TICKET}" && -z "${FIXUP}" ]]; then
    sed -i.bak -e "1s/^/[${TICKET}] /" $1
else
    BRANCH=$(git branch | grep \* | cut -c3-)
    sed -i.bak -e "1s/^/[${BRANCH}] /" $1
fi
```

## Stop tracking a remote branch without deleting
You don't have to delete your local branch.

Simply delete your remote tracking branch:

```git branch -d -r origin/<remote branch name>```
(This will not delete the branch on the remote repo!)

See "Having a hard time understanding git-fetch"

there's no such concept of local tracking branches, only remote tracking branches.
So origin/master is a remote tracking branch for master in the origin repo

As mentioned in Dobes Vandermeer's answer, you also need to reset the configuration associated to the local branch:

```git config --unset branch.<branch>.remote```

```git config --unset branch.<branch>.merge```

That will make any push/pull completely unaware of origin/<remote branch name>.
