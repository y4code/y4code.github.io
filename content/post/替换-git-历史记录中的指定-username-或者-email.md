+++
date = 2019-12-20T16:00:00Z
title = "替换 Git 历史记录中的指定 username 或者 email"
image =  "images/featured-post/post-4.jpg"
+++

    git filter-branch -f --commit-filter '
            if [ "$GIT_AUTHOR_NAME" != "给你康的名字" ];
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