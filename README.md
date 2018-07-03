## Unhide hidden files
```
```

## Delete branches that don't exist on origin anymore
```
git fetch --prune; git branch -vv | grep "\[origin.*: gone\]" | sed 's/.*\[origin\/\(.*\): gone\].*/\1/g' | xargs git branch -D
```
