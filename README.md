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
