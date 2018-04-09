# angry-barbie
$ git checkout -b gh-pages
$ rm .git/index
$ printf "hello world.\n" > index.html
$ git add index.html
$ git reset --hard $(echo "branch gh-pages init." | git commit-tree $(git write-tree))
$ git push -u origin gh-pages
