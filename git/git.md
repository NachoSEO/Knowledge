# Git

## Development workflow process
1. Update to the last version of the code removing the non existent branches: `git pull -p`
2. Remove non merged branches: `git branch -d $(git branch --merged master | grep -v master)`
3. Check if there’s any package to be added: `yarn` or `npm install`
4. Change the code in local
5. Create a branch: *`git checkout -b feature/{id}-{description}`*
6. Check changes in staging
7. Analyze which files are you going to upload: `git status`
8. (Optional) If everything it’s okay, time to run the tests: `yarn run test`
9. If every test it’s passed we can proceed to:
    1. `git add .`
    2. Check what you changed in every file: *`git diff --cached`
    3. `git commit -S -m “id-description”` `-S` only if you can [sign commits](../encryption/encryption.md#Sign-commits)
    4. *`git push origin HEAD`*
10. If we detect any problems we can put away all our changes of our branch with: `git stash`
11. We can do this command to change something from the commit: `git commit --amend`
12. To see what things we have stashed we can use: `git stash list`
13. To return the changes we need to use: `git stash pop`
14. Push the changes: `git push`
15. When the changes are pushed, the terminal will prompt the Pull Review URL. *Command + click* to enter the PR
16. Put the description in the PR and click the close the branch when merged
17. When the PR is approved time to merge!