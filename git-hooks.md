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