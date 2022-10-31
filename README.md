<h1 align="center">365g<br><sup>Project 365 (G)</sup><br>2022/10/26 - Present</h1>
<p align="center">365 is a project where I record one to several notes of my life for every single day that exists.</p>

This is the public, global version of project 365 - originally a personal/private project.

## Automation

A modified script for convenience

```bash
entryg () {
  ENTRYLOC="/r/e/d/a/c/t/e/d/365g/$(date +'%Y/%m/%d').md"
  REPOLOC="/r/e/d/a/c/t/e/d/365g/"
  ALIAS="Name"

  echo "EntryPath: $ENTRYLOC\n"

  cd $REPOLOC

  echo "git pull:"
  git -C $REPOLOC pull

  GITLASTCOMMITCODE=$(git -C $REPOLOC show -s --format=%s)

  if test -f "$ENTRYLOC"; then
    echo "Entry alread exists, skipping creation...\n"
  else
    echo "No entry found, creating...\n"
    mkdir -p $(dirname $ENTRYLOC)
    touch $ENTRYLOC
    echo "# $(date +'%B %d, %Y')" >> $ENTRYLOC
  fi

  echo "\n## $(date +'%H:%M:%S') ($ALIAS)\n\n" >> $ENTRYLOC

  vi "+normal G$" $ENTRYLOC
  # vi "+normal G$" +startinsert $ENTRYLOC

  GITCURRENTCOMMITCODE="$(date +'%Y%m%d')-0"

  if [[ "$(date +'%Y%m%d')" = "${GITLASTCOMMITCODE:0:8}" ]]; then
    GITCURRENTCOMMITCODE="$(date +'%Y%m%d')-$((${GITLASTCOMMITCODE:9} + 1))"
  fi

  git -C $REPOLOC add *
  git -C $REPOLOC commit -m "$GITCURRENTCOMMITCODE"

  echo "Entry committed\n"

  {
    git -C $REPOLOC push

    echo "Entry pushed to remote:main\n"

  } || {
    echo "Push to remote failed"
  }

  git -C $REPOLOC log -n 1
}
```
