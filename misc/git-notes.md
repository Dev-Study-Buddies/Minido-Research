# Github Guidelines

## Common git commands

*Note: All commands must be done inside a directory that is not a child/parent directory of another existing repository*

*Note: Omit the <...> from the commands and replace them with the appropiate value*

```bash
-------------------------------------------------------
# Pulling existing git project from github...
git clone <git repo link>

-------------------------------------------------------
# Working on a new thing

## Create and switch to new branch
git checkout -b <branchname>

## After creating new feature, bugfix, etc:
git add <add-your-files> # or add all: git add .

## commit your code with a message
git commit -m "<enter your message here>"

## Pushing your branch for the first time
## after doing this once, you can just do git push 
git push -u origin <branchname> 

-------------------------------------------------------
# Comparing local and remote branches

## Pull changes from github
git fetch origin

## List all branches
git branch -a

## See changes between local branch and remote branch
git diff <localbranch> <remotebranch>

-------------------------------------------------------
# Syncing local main branch with gihub; WARNING: Hard reset, will erase unsaved work
git fetch origin
git reset --hard origin/main
git clean -f -d
```

## Git workflow

```md
1. Member gets assigned a github issue & trello card
2. Member creates branch with naming scheme: `<issue-number>-descriptor` 
   2a. Ex:
          GitHub issue ---> "Create and test user endpoints #12"
          Branch name --> 12-add-user-endpoints
3. Member works on feature and commits their changes and pushes to their branch
4. Member creates pull request to merge into main branch when ready
5. Project members do PR review
6. Once approved, PR is merged and branch is closed
``` 

## Git commit message practices (Sources used: [freecodecamp](https://www.freecodecamp.org/news/writing-good-commit-messages-a-practical-guide/))

* Command to configure default editor for git commit messages: 
    `git config --global core.editor <your-text-editor-choice>`

* Commit message format (if doing `git commit` only):
```
Subject (short description). About 50 characters. Leave blank line after this

Here is the extended description (body) of the commit message. 
Should be about 72 characters. 
Contains detailed explanatory description of the change/ commit
```

* Commit message format (if doing `git commit -m`)
`git commit -m "Subject here" -m "Further description here..."`

* Some guidelines on good commit message practices:

```
1. Specify the type of commit (ex: feat, fix, style, refactor, etc.)
2. Separate the subject from the body with blank line
3. Capitalize the subject line and each paragrah
4. Use imparative mood in the subject line
   4a. Imperative mood = written as if giving a command or instruction
   4b. Ex: "Update database schemas" "Refactor User class for readability"

```

