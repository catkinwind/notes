1. list users
  awk -F: '$0=$1 " uid="$3' /etc/passwd

2. execute rspec
  bundle exec rspec spec/models/lxc\_hypervisor\_spec.rb

3. fetch a remote branch

git fetch
git checkout [branch name]

4. rename a branch

  "git branch -m <new_name>" to rename current branch.
  "git branch -m <old_name> <new_name>" to rename a branch.

5. http://stackoverflow.com/questions/6006737/git-merge-errors

  it's worth understanding what those error messages mean - needs merge and error: you need to resolve your current index first indicate that a merge failed, and that there are conflicts in those files. If you've decided that whatever merge you were trying to do was a bad idea after all, you can put things back to normal with:

  git reset --merge

6. execute cucumber: cucumber features/shutdown\_server.feature -p dev


7. remove a remote branch
    git push origin :[branch_name]
    git push origin --delete [branch_name]


8. set transaction timeout for Websphere Liberty.
    <transaction propogatedOrBMTTranLifetimeTimeout="7200" totalTranLifetimeTimeout="7200" />

9. remove a log in github project.

  git reset --hard 0a5fbaf78aef4228
  git push -f origin master

10. how to add another ssh key to git.

    if you are using git bash
    eval "$(ssh-agent -s)"
    ssh-add [key]

    in other terminals,

    eval $(ssh-agent -s)

11. grep -rnw '/path/to/somewhere/' -e "pattern" could find in file


12. how to run ohai:
  1) find ohai binary .
  2) -l will specify the log level
  3) -d will specify the directory locally.

13. git log --stat will display which files are modified.

    git log --stat --graph will display the graph of branch

    git show HEAD~1 will show change of current branch for 1th commits.

14. bg will send the job to background. fg will bring it to front end.

15. how to display file name in vim: ctrl g

16. how to bulk rename files in some pattern: for f in d\*.c; do mv "$f" "d11${f#d0}"; done

17. attach a local git repo to remote one: git push --mirror git@repo.

18. rename file in some pattern: for f in d0\*.c; do mv "$f" "d1${f#d0}"; done

19. how to make a "find in files" script:

#!/bin/bash

find . -type f -iname "$1" | xargs grep "$2"

then we could use fif.sh '\*.rb' 'something to search' to search in files match the pattern.

20. zsh is slow in git repo directories.
    git config --add oh-my-zsh.hide-status 1
    git config --add oh-my-zsh.hide-dirty 1
    Detailed Answer: There are two central git functions (git_prompt_info() and parse_git_dirty()) in lib/git.zsh. Each Method has a git config switch to disable it (oh-my-zsh.hide-status and oh-my-zsh.hide-dirty)

    from http://stackoverflow.com/questions/12765344/oh-my-zsh-slow-but-only-for-certain-git-repo

21. Numbers software engineers should know.
L1 cache reference:                                   0.5 ns
Branch mispredict:                                    5 ns
L2 cache reference:                                   7 ns
Mutex lock/unlock:                                    100 ns
Compress 1K bytes with a cheap compression algorithm: 10,000 ns
Send 2K bytes over 1 Gbps network:                    20,000 ns
Read 1MB sequentially from memory:                    250,000 ns
Round trip within same datacenter:                    500,000 ns
Disk seek:                                            10,000,000 ns
Read 1MB sequentially from network:                   10,000,000 ns
Read 1MB sequentially from disk:                      30,000,000 ns
Send packet CA->Netherlands->CA:                      150,000,000 ns
Cheap compression factor:                             2
