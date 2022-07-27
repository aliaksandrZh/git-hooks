# Git hooks

path: `.git/hooks` <br>

## pre-commit

### Prevent committing in master and release branch

filename: `pre-commit`

script:
```bash
#!/bin/sh

branch="$(git rev-parse --abbrev-ref HEAD)"

if [ "$branch" = "master" ] || [[ $branch = *"release/"* ]]
then
  echo "You can't commit directly to $branch branch!"
  exit 1
else
  echo "Committed to $branch"
fi
```


## pre-rebase

## Prevent rebasing into forbidden branches

filename: `pre-rebase`
```bash
#!/bin/sh

FORBIDDEN_BRANCHES="dev master"
FORBIDDEN_PATTERNS="release/"

CURRENT_BRANCH="$(git branch --show-current)"

stop_rebasing() {
  echo "You can't rebase into $1 branch!"
  exit 1
}

for forbidden_branch in $FORBIDDEN_BRANCHES
do
  if [ "$CURRENT_BRANCH" = "$forbidden_branch" ]
  then
    stop_rebasing "$CURRENT_BRANCH"
  fi
done


for forbidden_pattern in $FORBIDDEN_PATTERNS
do
  if [[ $CURRENT_BRANCH = *"$forbidden_pattern"* ]]
  then
    stop_rebasing "$CURRENT_BRANCH"
  fi
done

echo "Rebased to $CURRENT_BRANCH"
exit 0
```