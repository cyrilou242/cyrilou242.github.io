---
layout: page
title: SNPTS
date: 2020-04-24T12:00:00.000+05:30
categories:
- misc
- git
lang: en
ref: git

---
## SNPTS

Things I use but often forget.

### Cleanup before a merge request

#### Squash commits (interactive)

    git rebase -i HEAD~3 --autostash

Effect: rerun the 3 last commits.  
Do: follow the interactive indication.  
Prefer `fixup` to `stash` to put one minor commit into another.  
Prefer `--force-with-lease` to `--force` when pushing.

#### Rewrite commits

    git reset (--soft) HEAD~3

Effect: remove the last 3 commits, but keep the changes  
use _--soft to_ keep the changes in the staging area  
Do: create new commits

#### Change last commit message

    git commit --amend -m "New Message"

### Cheery pick some changes

    git cherry-pick <SHA-1>

Use `--no-commit`  or `-n` to pick the change without committing.

### Quick stash

`--autostash` works with most of the commands.

Does not work with checkout, so:

    git stash && git fetch && git checkout my_branch && git stash pop

#### Download a single file

    git fetch --all
    git checkout origin/myBranch -- my/file.cpp

### Bash

#### Files with details

    ls -hal

#### Change permissions

[More permissions here.](https://linuxhint.com/linux_file_permissions/ "More permissions")

    chmod -R MODE DIRECTORY
    
    chmod -R 755 /var/www/html

MODE in octal notation is handy but cannot do incremental change.

    chmod +x my script.sh

[See the official doc for more variable expansion.](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)

#### getValue or default

    ${MY_VAR:-"default_value"}

#### getValue or default and set

If MY_VAR is not set, it is set to the right side.

    ${MY_VAR:="default_value"}

#### Expand if not empty else nothing

Expand the right if left is set and non-empty. Otherwise expands to nothing

    ${MY_VAR:+"$MY_VAR"}

#### Slice

    string=01234567890abcdefgh
    echo ${string:7}
    >>> 7890abcdefgh
    echo ${string:7:2}
    >>> 78
    echo ${string:7:-2}
    >>> 7890abcdef
    echo ${string: -7}
    >>> bcdefgh
    $ echo ${string: -7:2}
    bc
    $ echo ${string: -7:-2}
    bcdef

#### Get the length of a variable

    ${#VAR}

### Maven

#### Find unused dependencies

    ./mvnw dependency:analyze

If the dependency is not used at compile time, the plugin will state a dependency is not used. To mark a dependency as used: 

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <configuration>
            <usedDependencies>
                <dependency>commons-collections:commons-collections</dependency>
            </usedDependencies>
        </configuration>
    </plugin>

\--