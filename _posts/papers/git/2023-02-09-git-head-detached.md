---
layout: post
title: [Error] HEAD detached from
category: Git
tag: git error
---

#### 문제

`HEAD detached from <branch name>` 이라고 하면서 commit한 내용에 대해서 push가 되지 않음

#### 해결

1. 임시 branch를 생성해서 문제가 발생한 branch를 옮긴다

```
git branch tmp
git checkout tmp
git branch -f <branch name> tmp
git checkout <branch name>
```

이 때, 다음과 같이 뜨면 성공!

```
Switched to branch '<branch name>'
Your branch is ahead of '<branch name>' by 1 commit.
  (use "git push" to publish your local commits)
```

위와 같이 commit한 내용이 아직 push가 안 된 상태로 원복된다.

2. 임시 branch 삭제하고 commit한 내용에 대한 push를 진행

```
git branch -d tmp
git push
```
