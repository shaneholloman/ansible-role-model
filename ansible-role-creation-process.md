# Ansible Role Creation Process

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

This part may seem like magic, but the reason it just works is because we are using the docker images I have created for this purpose. Molecule will pull the image from Docker Hub and run the tests in the container on your machine. This is the same process that will be used in the CI/CD pipeline.

!!! note Note:

    This is why we need to set the environment variables for Docker Hub and Galaxy in the GitHub repository

```sh
molecule test
```

## Write the Ansible Role Readme file

This will based on the initialized template

## Ansible Role GitHub Deploy and CI

You can do this manually or simply use the [kickstart script](ansible-role-model/kickstart.sh)

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
