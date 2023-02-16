+++
date = 2019-12-20T16:00:00.000Z
image = "images/featured-post/post-4.jpg"
title = "替换 Git 历史记录中的指定 username 或者 email"

+++
git filter-branch -f --commit-filter '
if \[ "$GIT_AUTHOR_NAME" != "给你康的名字" \];
then
GIT_AUTHOR_NAME="给你康的名字";
GIT_AUTHOR_EMAIL="给你康的邮箱";
git commit-tree "$@";
else
git commit-tree "$@";
fi' HEAD

其中第二行还可以改成

    if [ "$GIT_AUTHOR_NAME" = "不想给你康的名字" ];

完事后强制推送

    git push --force --tags origin 'refs/heads/*'

#### 3/25/2020 更新

发现一个可以用 GUI 来自由修改 Commit 提交时间和提交邮箱的[https://bokub.github.io/git-history-editor/](https://bokub.github.io/git-history-editor/ "https://bokub.github.io/git-history-editor/")

公司邮箱提交到私人GitHub上Commit上，就可以用这个工具进行修改

**需要注意的是，这个项目生成的命令在最后似乎多了一个fi，需要将其删掉**