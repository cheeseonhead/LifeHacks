## Unhide hidden files
```
```

## Delete branches that don't exist on origin anymore
```
git fetch --prune; git branch -vv | grep "\[origin.*: gone\]" | sed 's/.*\[origin\/\(.*\): gone\].*/\1/g' | xargs git branch -D
```

## Prefix commits with JIRA ticket id
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
