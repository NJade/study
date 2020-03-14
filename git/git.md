# 기본 사용
* 머지 후 작업
```sh
# 마스터 브랜치
git pull origin

# 작업 브랜치
git rebase origin/master
```
* 다른 브랜치 작업 중 머지된 것 반영
```sh
git fetch origin
git stash
git rebase origin/master
git stash pop
```