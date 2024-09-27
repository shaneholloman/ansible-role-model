# ansible-role-model

> CodeMap Source: Local directory: `/home/shadmin/_github/ansible-role-model`

This markdown document provides a comprehensive overview of the directory structure and file contents. It aims to give viewers (human or AI) a complete view of the codebase in a single file for easy analysis.

## Document Table of Contents

The table of contents below is for navigational convenience and reflects this document's structure, not the actual file structure of the repository.

<!-- TOC -->

- [ansible-role-model](#ansible-role-model)
  - [Document Table of Contents](#document-table-of-contents)
  - [Repo File Tree](#repo-file-tree)
  - [Repo File Contents](#repo-file-contents)
    - [README.md](#readmemd)
    - [.gitignore](#gitignore)
    - [.ansible-lint](#ansible-lint)
    - [.gitattributes](#gitattributes)
    - [.github/FUNDING.yml](#githubfundingyml)
    - [.github/workflows/ci.yml](#githubworkflowsciyml)
    - [.github/workflows/release.yml](#githubworkflowsreleaseyml)
    - [.github/workflows/remove.yml](#githubworkflowsremoveyml)
    - [.github/workflows/stale.yml](#githubworkflowsstaleyml)
    - [.yamllint](#yamllint)
    - [ansible-role-creation-process.md](#ansible-role-creation-processmd)
    - [ansible-role-model.code-workspace](#ansible-role-modelcode-workspace)
    - [defaults/main.yml](#defaultsmainyml)
    - [handlers/main.yml](#handlersmainyml)
    - [kickstart.sh](#kickstartsh)
    - [LICENSE](#license)
    - [meta/main.yml](#metamainyml)
    - [molecule/default/cleanup.yml](#moleculedefaultcleanupyml)
    - [molecule/default/converge.yml](#moleculedefaultconvergeyml)
    - [molecule/default/molecule.yml](#moleculedefaultmoleculeyml)
    - [molecule/default/prepare.yml](#moleculedefaultprepareyml)
    - [molecule/default/side\_effect.yml](#moleculedefaultside_effectyml)
    - [molecule/default/verify.yml](#moleculedefaultverifyyml)
    - [notes/notes.md](#notesnotesmd)
    - [requirements.yml](#requirementsyml)
    - [tasks/main.yml](#tasksmainyml)
    - [tests/inventory](#testsinventory)
    - [tests/test.yml](#teststestyml)
    - [vars/main.yml](#varsmainyml)

<!-- /TOC -->

## Repo File Tree

This file tree represents the actual structure of the repository. It's crucial for understanding the organization of the codebase.

```tree
.
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── release.yml
│   │   ├── remove.yml
│   │   └── stale.yml
│   └── FUNDING.yml
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── molecule/
│   └── default/
│       ├── cleanup.yml
│       ├── converge.yml
│       ├── molecule.yml
│       ├── prepare.yml
│       ├── side_effect.yml
│       └── verify.yml
├── notes/
│   └── notes.md
├── tasks/
│   └── main.yml
├── tests/
│   ├── inventory
│   └── test.yml
├── vars/
│   └── main.yml
├── .ansible-lint
├── .gitattributes
├── .gitignore
├── .yamllint
├── LICENSE
├── README.md
├── ansible-role-creation-process.md
├── ansible-role-model.code-workspace
├── kickstart.sh
└── requirements.yml

11 directories, 29 files
```

## Repo File Contents

The following sections present the content of each file in the repository. Large and binary files are acknowledged but their contents are not displayed.

### ansible-role-creation-process.md

````markdown
# Ansible Role Creation Process

This doc will explain how to build an Ansible role from scratch then pushed to GitHub and subsequently deploy to Ansible Galaxy for playbook consumption. Playbook flow is not covered here.

[[toc]]

## Ansible Role Initialization

### Step 1: Initialize Ansible Galaxy Role Skeleton

The first step in creating an Ansible role is to initialize it using the `ansible-galaxy init` command. This command creates a basic directory structure for the role.

When naming your role, Ansible Galaxy uses a namespace (like `shaneholloman` or `cello`), followed by the name of the role (like `bootstrap` or `ntpsec`). The final result would look something like `shaneholloman.bootstrap`.

```sh
ansible-galaxy init shaneholloman.bootstrap
```

However, this naming convention is not suitable for hosting the Ansible role code on GitHub, as GitHub does not allow `.` in repository names. Also, when we have many repositories, a standard naming convention becomes essential for organization. Therefore, we initialize the repository with the name we expect to use on Ansible Galaxy, but we rename it for our working code base. Instead of naming it `shaneholloman.bootstrap`, we rename it to `ansible-role-bootstrap`.

```sh
mv shaneholloman.bootstrap ansible-role-bootstrap
```

### Step 2: Initialize Molecule Scenario Skeleton

After renaming the directory, navigate into it and initialize a Molecule scenario. Molecule is a testing framework for Ansible that helps us write and manage test cases for our roles. We use the `molecule init scenario` command to create a skeleton for our test cases. The `-d docker` option specifies that we want to use Docker as our driver for running the tests.

```sh
cd ansible-role-bootstrap
molecule init scenario -d docker
```

### Step 3: Add Default Files to Scenario and Role

Next, you need to add some default files to the scenario and role. You can find examples of these default files in the `1.ansible-role-model` directory in This workspace.

### Step 4: Remove Unnecessary Default Files from Scenario and Role

Some default files may not be necessary for your specific role or scenario. You can find a list of these unnecessary default files in the `1.ansible-role-model` directory in this workspace. They are "listed" by their absence in the `1.ansible-role-model` directory.

### Step 5: Write Correct Metadata for Role

Finally, you need to write the correct metadata for your role. You can find an example of how to write this metadata in the `1.ansible-role-model/meta/main.yml` file in this workspace.

## Ansible Role Development Workflow

- Write Role Tasks
- Write Role Handlers
- Write Role Variables
- Write Role Templates
- Write Role Files
- Write Role Defaults
- Write Role Meta

## Ansible Role Testing

This part may seem like magic, but the reason it just works is because we are using the docker images I have already created and posted to DockerHub for this purpose. Molecule will pull the image from DockerHub and run the tests in the container on your machine. This is the same process that will be used in the CI/CD pipeline on GitHub via the Action workflows.

> :exclamation: **Note:** This is why we need to set the environment variables for Docker Hub and Galaxy in the GitHub repository.

```sh
molecule test
```

## Write the Ansible Role Readme file

This will be based on the initialized template

## Setup CI (GitHub Actions Workflows)

The dir `./.github` is where you'll find the GitHub Actions workflow magic. Usually you'll not have to make many changes in here.

```sh
.github/workflows/ci.yml
.github/workflows/release.yml
.github/workflows/remove.yml
```

Replace the `shaneholloman.model` with the rolename you are working on. This is the name of the role to be on Ansible Galaxy.

## Ansible Role GitHub Deploy and CI Trigger

You can do this manually or simply use the [kickstart script](./kickstart.sh)

```sh
bash kickstart.sh
```

Or consider these steps manually if you want to understand the process:

Here's a step-by-step breakdown of what the script does, including the corresponding code:

1. **Define the GitHub account or organization name**: This is the name of your GitHub account or organization where the repository will be created.

    ```sh
    githubAccount="shaneholloman"  # Change this to your personal account name if needed
    ```

2. **Initialize a new git repository**: This step creates a new git repository in the current directory. The `-b main` option specifies that the main branch should be created.

    ```sh
    git init -b main
    ```

3. **Get the name of the current repository from the top-level directory**: This step retrieves the name of the current directory, which is assumed to be the name of the repository.

    ```sh
    repoName=$(basename "$(git rev-parse --show-toplevel)")
    ```

4. **Create a new repository on GitHub using the gh CLI**: This step creates a new repository on GitHub with the same name as the current directory. The repository is public and the creation is confirmed without prompting the user.

    ```sh
    gh repo create "$repoName" --public -y
    ```

5. **Add the remote repository**: This step adds the newly created GitHub repository as a remote repository for the local git repository.

    ```sh
    git remote add origin https://github.com/$githubAccount/"$repoName".git
    ```

6. **Add all files in the current directory to the git repository**: This step stages all files in the current directory for commit in the git repository.

    ```sh
    git add .
    ```

7. **Commit the changes**: This step commits the staged changes with the message "Initial commit".

    ```sh
    git commit -m "Initial commit"
    ```

8. **Push the changes to GitHub**: This step pushes the committed changes to the main branch of the remote repository on GitHub.

    ```sh
    git push -u origin main
    ```

9. **Define an associative array where the key is the name of the secret and the value is the secret value**: This step defines an associative array of secrets, where the key is the name of the secret and the value is the secret value.

    ```sh
    declare -A secrets
    secrets

    =(


    ["DOCKERHUB_TOKEN"]="$DOCKERHUB_TOKEN"
    ["DOCKERHUB_USERNAME"]="$DOCKERHUB_USERNAME"
    ["GALAXY_API_KEY"]="$GALAXY_API_KEY"
    # Add more secrets here as needed
    )
    ```

10. **Check if environment variables exist**: This step checks if the environment variables for the secrets exist. If any are missing, it prints a message to the console and exits the script.

    You can add these environment variables to your `.bashrc` file by running the following commands:

    ```sh
    nano .bashrc
    ```

    add the following lines to the end of the file

    ```sh
    export DOCKERHUB_TOKEN="dckr_xxxxxxxxxxxxxxxxxxxx"
    export DOCKERHUB_USERNAME="shaneholloman"
    export GALAXY_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxx"
    ```

    ```sh
    missingVars=()
    for key in "${!secrets[@]}"
    do
    if [ -z "${secrets[$key]}" ]; then
        missingVars+=("$key")
    fi
    done

    if [ ${#missingVars[@]} -ne 0 ]; then
    echo "The following environment variables are missing:"
    for var in "${missingVars[@]}"
    do
        echo "$var"
    done
    echo "Please add them to your .bashrc file and run 'source ~/.bashrc'"
    exit 1
    fi
    ```

11. **Loop through each secret to set it for the current repository**: This step loops through each secret and sets it for the current repository on GitHub using the `gh secret set` command.

    ```sh
    for key in "${!secrets[@]}"
    do
    value=${secrets[$key]}
    command="echo -n $value | gh secret set $key --repo=$githubAccount/$repoName"
    eval "$command"
    done
    ```

12. **Tag and push after setting the secrets**: This step creates an empty commit, tags it with the version "0.0.1", and pushes the tag to the remote repository on GitHub.

    ```sh
    git commit --allow-empty -m "tagging first version"
    git tag -a 0.0.1 -m "pre release version - ubuntu only"
    git push origin 0.0.1
    ```

    The script manages this part like so:

    ```sh
    # Tag and push after setting the secrets
    commitMessage="tagging first version"
    tagVersion="0.0.1"
    tagMessage="pre release version - ubuntu only"

    git commit --allow-empty -m "$commitMessage"
    git tag -a $tagVersion -m "$tagMessage"

    # Ask the user if the current git tag and message are correct
    echo "The current git tag is $tagVersion with the message '$tagMessage'. Is this correct? (yes/no)"
    read answer

    if [ "$answer" != "${answer#[Yy]}" ] ;then
    git push origin $tagVersion
    else
    echo "Please edit the git tag and message in this script."
    fi
    ```

## Here's a review of what we did

We just went through the process of creating an Ansible role from scratch, pushing it to GitHub, and deploying it to Ansible Galaxy for playbook consumption.

We started by initializing the Ansible Galaxy role skeleton and renaming it for GitHub compatibility. We then initialized a Molecule scenario for testing and added necessary files while removing unnecessary ones. We wrote the correct metadata for the role and outlined the Ansible role development workflow, which includes writing role tasks, handlers, variables, templates, files, defaults, and metadata.

We also covered how to test the role using Molecule and Docker, and how to write the Ansible role readme file. We set up continuous integration using GitHub Actions workflows and deployed the role to GitHub, triggering the CI process. This included defining the GitHub account, initializing a new git repository, creating a new repository on GitHub, adding the remote repository, committing and pushing changes, defining and checking environment variables, setting secrets for the repository, and tagging and pushing after setting the secrets.
````

### requirements.yml

```yml
---
# No requirements
roles: []
collections: []

# Has requirements
# -----------------------------------
# roles:
#   - name: shaneholloman.model
#     version: 1.0.0

# collections:
#   - name: community.general
#     version: 3.0.0
```

### .yamllint

```txt
---
extends: default

rules:
  line-length:
    max: 220
    level: warning

ignore: |
  .github/workflows/stale.yml
```

### .gitignore

```ini
.molecule
*.log
*.swp
.tox
.cache
.DS_Store
*.retry
*/__pycache__
*.pyc
```

### kickstart.sh

```bash
#!/bin/bash

# Define the GitHub account or organization name
githubAccount="shaneholloman"  # Change this to your personal account name if needed

# Initialize a new git repository
git init -b main

# Get the name of the current repository from the top-level directory
repoName=$(basename "$(git rev-parse --show-toplevel)")

# Create a new repository on GitHub using the gh CLI
gh repo create "$repoName" --public -y

# Add the remote repository
git remote add origin https://github.com/$githubAccount/"$repoName".git

# Add all files in the current directory to the git repository
git add .

# Commit the changes
git commit -m "Initial commit"

# Push the changes to GitHub
git push -u origin main

# Define an associative array where the key is the name of the secret and the value is the secret value
declare -A secrets
secrets=(
  ["DOCKERHUB_TOKEN"]="$DOCKERHUB_TOKEN"
  ["DOCKERHUB_USERNAME"]="$DOCKERHUB_USERNAME"
  ["GALAXY_API_KEY"]="$GALAXY_API_KEY"
  # Add more secrets here as needed
)

# Check if environment variables exist
missingVars=()
for key in "${!secrets[@]}"
do
  if [ -z "${secrets[$key]}" ]; then
    missingVars+=("$key")
  fi
done

if [ ${#missingVars[@]} -ne 0 ]; then
  ## Print 2 blank lines here to make the output easier to read
  echo ""
  echo ""
  echo "The following environment variables are missing:"
  for var in "${missingVars[@]}"
  do
    echo "$var"
  done
  echo "Please add them to your .bashrc file and run 'source ~/.bashrc'"
  exit 1
fi

# Loop through each secret to set it for the current repository
for key in "${!secrets[@]}"
do
  value=${secrets[$key]}
  command="echo -n $value | gh secret set $key --repo=$githubAccount/$repoName"
  eval "$command"
done

# Tag and push after setting the secrets
commitMessage="tagging first version"
tagVersion="2.0.0"
tagMessage="An Ansible role model template only"

git commit --allow-empty -m "$commitMessage"
git tag -a $tagVersion -m "$tagMessage"

# Ask the user if the current git tag and message are correct
## Print 2 blank lines here to make the output easier to read
echo ""
echo ""
echo "The current git tag is $tagVersion with the message '$tagMessage'. Is this correct? (yes/no)"
read -r answer

if [ "$answer" != "${answer#[Yy]}" ] ;then
  git push origin $tagVersion
else
  echo "Please edit the git tag and message in this script."
fi
```

### ansible-role-model.code-workspace

```txt
{
	"folders": [
		{
			"path": "."
		}
	],
	"settings": {
		"ansible.python.interpreterPath": "/bin/python3",
		"ansible.schemas": {
			"ansible://yaml": "*.yml" // associate all .yml files with the Ansible schema
		}
	}
}
```

### .gitattributes

```txt
* text=auto eol=lf
```

### README.md

````markdown
# Ansible Role `model`

This is an ansible role model template only. A brief description of a functional role goes here.

## Requirements

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

## Role Variables

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

## Dependencies

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yml
- hosts: servers
  roles:
      - { role: shaneholloman.model, x: 42 }
```

## License

Unlicense

## Author Information

Shane Holloman
````

### .ansible-lint

```txt
skip_list:
  - 'yaml'
  - 'role-name'
```

### LICENSE

```txt
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <https://unlicense.org>
```

### molecule/default/verify.yml

```yml
---
- name: Verify
  hosts: all
  gather_facts: false
  tasks: []
```

### molecule/default/prepare.yml

```yml
---
- name: Prepare
  hosts: all
  gather_facts: false
  tasks: []
```

### molecule/default/converge.yml

```yml
---
- name: Converge
  hosts: all
  become: true

  pre_tasks:
    - name: Update apt cache.
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 600
      when: ansible_os_family == 'Debian'
      changed_when: false

  roles:
    - role: shaneholloman.model
```

### molecule/default/cleanup.yml

```yml
---
- name: Cleanup
  hosts: all
  gather_facts: false
  tasks: []
```

### molecule/default/molecule.yml

```yml
---
role_name_check: 1
dependency:
  name: galaxy
  options:
    role-file: requirements.yml
    requirements-file: requirements.yml
driver:
  name: docker
platforms:
  - name: instance
    image: "shaneholloman/docker-${MOLECULE_DISTRO:-ubuntu2204}-ansible:latest"
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    privileged: true
    pre_build_image: true
provisioner:
  name: ansible
  playbooks:
    converge: ${MOLECULE_PLAYBOOK:-converge.yml}
```

### molecule/default/side_effect.yml

```yml
---
- name: Side Effect
  hosts: all
  gather_facts: false
  tasks: []
```

### tasks/main.yml

```yml
---
# tasks file for shaneholloman.model
```

### notes/notes.md

````markdown
# Notes

Good working version:

```sh
git reset --hard cee243a
git push origin HEAD --force
```
````

### handlers/main.yml

```yml
---
# handlers file for shaneholloman.model
```

### .github/FUNDING.yml

```yml
---
github: shaneholloman
```

### .github/workflows/release.yml

```yml
---
# Release Repo to Ansible Galaxy
name: Release
'on':
  workflow_dispatch:
  push:
    tags:
      - '*'

defaults:
  run:
    working-directory: 'shaneholloman.model'

jobs:

  release:
    name: Release
    runs-on: ubuntu-latest
    steps:
      - name: Check out the codebase.
        uses: actions/checkout@v4
        with:
          path: 'shaneholloman.model'

      - name: Set up Python 3.
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Ansible.
        run: pip3 install ansible-core

      - name: Trigger a new import on Galaxy.
        run: >-
          ansible-galaxy role import --api-key ${{ secrets.GALAXY_API_KEY }}
          $(echo ${{ github.repository }} | cut -d/ -f1)
          $(echo ${{ github.repository }} | cut -d/ -f2)
```

### .github/workflows/stale.yml

```yml
---
name: Close inactive issues
'on':
  schedule:
    - cron: "55 23 * * 0"  # semi-random time

jobs:
  close-issues:
    runs-on: ubuntu-latest
    permissions:
      issues: write
      pull-requests: write
    steps:
      - uses: actions/stale@v8
        with:
          days-before-stale: 120
          days-before-close: 60
          exempt-issue-labels: bug,pinned,security,planned
          exempt-pr-labels: bug,pinned,security,planned
          stale-issue-label: "stale"
          stale-pr-label: "stale"
          stale-issue-message: |
            This issue has been marked 'stale' for lack of recent activity.<br>
            If there's no further activity:<br>
            Issue closes in 30 days.<br>
            Thank you for your contribution!
          close-issue-message: |
            This issue has been closed for inactivity.<br>
            If you feel this is in error:<br>
            Please reopen issue or file new issue with relevant details.
          stale-pr-message: |
            This pr has been marked 'stale' for lack of recent activity.<br>
            If there is no further activity:<br>
            Issue closes in 30 days.<br>
            Thank you for your contribution!
          close-pr-message: |
            This PR has been closed for inactivity.<br>
            If you feel this is in error:<br>
            Please reopen issue or file new issue with relevant details.
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

### .github/workflows/ci.yml

```yml
---
name: CI
'on':
  workflow_dispatch:
  pull_request:
  push:
    branches:
      - main
  schedule:
    - cron: "30 4 * * 1"

defaults:
  run:
    working-directory: 'shaneholloman.model'

jobs:

  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Check out the codebase.
        uses: actions/checkout@v4
        with:
          path: 'shaneholloman.model'

      - name: Set up Python 3.
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install test dependencies.
        run: pip3 install yamllint

      - name: Lint code.
        run: |
          yamllint .

  molecule:
    name: Molecule
    runs-on: ubuntu-latest
    strategy:
      matrix:
        distro:
          - rockylinux9
          - ubuntu2204
          - debian12

    steps:
      - name: Check out the codebase.
        uses: actions/checkout@v4
        with:
          path: 'shaneholloman.model'

      - name: Set up Python 3.
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install test dependencies.
        run: pip3 install ansible molecule molecule-plugins[docker] docker

      - name: Run Molecule tests.
        run: molecule test
        env:
          PY_COLORS: '1'
          ANSIBLE_FORCE_COLOR: '1'
          MOLECULE_DISTRO: ${{ matrix.distro }}
```

### .github/workflows/remove.yml

```yml
---
# Remove Repo from Ansible Galaxy
name: Remove
'on':
  workflow_dispatch:

defaults:
  run:
    working-directory: 'shaneholloman.model'

jobs:

  release:
    name: Remove from Galaxy
    runs-on: ubuntu-latest
    steps:
      - name: Check out the codebase.
        uses: actions/checkout@v4
        with:
          path: 'shaneholloman.model'

      - name: Set up Python 3.
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Ansible.
        run: pip3 install ansible-core

      - name: Trigger a removal from Galaxy.
        run: >-
          ansible-galaxy role delete --api-key ${{ secrets.GALAXY_API_KEY }}
          $(echo ${{ github.repository }} | cut -d/ -f1)
          $(echo ${{ github.repository }} | cut -d/ -f2)
```

### tests/test.yml

```yml
---
- hosts: localhost
  remote_user: root
  roles:
    - shaneholloman.model
```

### tests/inventory

```txt
localhost
```

### vars/main.yml

```yml
---
# vars file for shaneholloman.model
```

### meta/main.yml

```yml
---
galaxy_info:
  role_name: model
  author: shaneholloman
  description: This is only an Ansible Role development template
  company: "shaneholloman"

  # If the issue tracker for your role is not on github, uncomment the
  # next line and provide a value
  # issue_tracker_url: http://example.com/issue/tracker

  license: "license (Unlicense)"

  min_ansible_version: "2.12"

  # If this a Container Enabled role, provide the minimum Ansible Container version.
  # min_ansible_container_version:

  platforms:
    - name: EL
      versions:
        - all
    - name: Debian
      versions:
        - all
    - name: Ubuntu
      versions:
        - all

  galaxy_tags:
    - system

dependencies: []
```

### defaults/main.yml

```yml
---
# defaults file for shaneholloman.model
```

> This concludes the repository's file contents. Please review thoroughly for a comprehensive understanding of the codebase.
