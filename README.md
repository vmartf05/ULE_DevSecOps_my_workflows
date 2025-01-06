# ULE_DevSecOps_my_workflows - Lab 2: "CI/CD with Act"

## Table of Contents
1. [Introduction](#introduction)
2. [Working with Act](#working-with-act)
3. [Running Workflows with Act](#running-workflows-with-act)
4. [Flags of Act](#flags-of-act)
5. [Conclusion](#conclusion)
6. [License](#license)

## 1. Introduction

The objective of this repository, which consists solely of this README, is to provide an overview of how to use Act to run GitHub workflows locally.

In the context of DevSecOps, Act is a powerful tool that allows developers to quickly iterate on their workflow configurations by providing rapid feedback without needing to commit and push changes to GitHub.

By emulating the GitHub environment, including environment variables and the filesystem, Act enables the local execution of workflows, ensuring that continuous integration and continuous delivery and deployment (CI/CD) processes are seamless and efficient. This approach eliminates the need for redundant tools like `make` by leveraging the GitHub Actions defined in `.github/workflows/` for task management and automation.

## 2. Working with Act

The Act tool helps streamline the development process by providing fast feedback and enabling you to debug workflows locally using Docker.

### 2.1. Installing Act

#### 2.1.1. Install Go

- Navigate to the GitHub repo in which ...: [Act Repository](https://github.com/nektos/act)

  - Follow the instructions in the "Manually building from source" section of the README:
  
    - **Download Go** from the official website: [Go Downloads](https://golang.org/dl/)

    - On Windows, open the installer and follow the wizard to install Go.
  
    - Open a Windows Command Prompt (cmd) window, navigate to the Go installation directory, and verify the Go version installed:
      ```powershell
      cd "C:\Program Files\Go"
      ````
      ```powershell
      go version
      ```
      Expected output: `go version go1.23.4 windows/amd64`

#### 2.1.2. Clone the Act Repository
- Open a Windows PowerShell, navigate to the directory where you want to clone the Act repo, and clone it to have a local copy of the Act repo on your computer:
  ```powershell
  cd "C:\Users\Usuario\Desktop"
  ```
  ```powershell
  git clone https://github.com/nektos/act.git
  ```
  Expected output:
  ```
  Cloning into 'act'...
  remote: Enumerating objects: 12205, done.
  remote: Counting objects: 100% (1889/1889), done.
  remote: Compressing objects: 100% (115/115), done.
  remote: Total 12205 (delta 1834), reused 1777 (delta 1773), pack-reused 10316 (from 3)
  Receiving objects: 100% (12205/12205), 10.01 MiB | 8.89 MiB/s, done.
  Resolving deltas: 100% (7260/7260), done.```
  ```

#### 2.1.3. Run Unit Tests
- Navigate to the cloned Act repo and run the Go tests:
  ```powershell
  cd "C:\Program Files\Go"
  ````
  ```powershell
  go test ./...
  ```
Since the majority of the tests passed, we can proceed with the next steps.

### 2.1.4. Build and Install the Act binary
- If necessary, stop the previous process:
  ```powershell
  Ctrl+C
  ````
- Build the Act binary with VCS stamping disabled:
  ```powershell
  go build -buildvcs=false -o act.exe
  ```
- Verify the presence of `act.exe` in the cloned repo:
  ```powershell
  Get-ChildItem
  ```
  Expected output:
  ```plaintext
   Mode                 LastWriteTime         Length Name
  ----                 -------------         ------ ----
  d-----        05/01/2025     16:46                .github
  d-----        05/01/2025     16:46                .vscode
  d-----        05/01/2025     16:46                cmd
  d-----        05/01/2025     16:46                pkg
  -a----        05/01/2025     16:46              0 .actrc
  -a----        05/01/2025     16:46            263 .codespellrc
  -a----        05/01/2025     16:46            352 .editorconfig
  -a----        05/01/2025     16:46            413 .gitignore
  -a----        05/01/2025     16:46            115 .gitleaksignore
  -a----        05/01/2025     16:46           1290 .golangci.yml
  -a----        05/01/2025     16:46           1044 .goreleaser.yml
  -a----        05/01/2025     16:46            277 .markdownlint.yml
  -a----        05/01/2025     16:46            484 .mega-linter.yml
  -a----        05/01/2025     16:46           2697 .mergify.yml
  -a----        05/01/2025     16:46             29 .prettierignore
  -a----        05/01/2025     16:46            132 .prettierrc.yml
  -a----        05/01/2025     16:46           1411 act-cli.nuspec
  -a----        05/01/2025     17:13       29652480 act.exe
  -a----        05/01/2025     16:46            286 codecov.yml
  -a----        05/01/2025     16:46             33 CODEOWNERS
  -a----        05/01/2025     16:46           5610 CONTRIBUTING.md
  -a----        05/01/2025     16:46           4970 go.mod
  -a----        05/01/2025     16:46          30928 go.sum
  -a----        05/01/2025     16:46           5994 IMAGES.md
  -a----        05/01/2025     16:46          11397 install.sh
  -a----        05/01/2025     16:46           1077 LICENSE
  -a----        05/01/2025     16:46            571 main.go
  -a----        05/01/2025     16:46           2485 Makefile
  -a----        05/01/2025     16:46           3222 README.md
  -a----        05/01/2025     16:46            230 VERIFICATION
  -a----        05/01/2025     16:46              6 VERSION
  ````
- Confirm the installation of Act
  ```powershell
  .\act.exe --version
  ```
  Expected output: ```act version 0.2.71```

Ensure that Docker is installed and running on your system, as Act uses Docker to replicate the GitHub Actions environment locally.

## 3. Running Workflows with Act

### 3.1. Create the Workflow File
- Navigate to the existing .github/workflows directory:
  ```powershell
  cd .github/workflows
  ```
- Open Notepad++, create a new workflow file named `example-workflow.yml` and save it inside the `.github/workflows` directory:
  ```yaml
  name: Test Example
  on: [push]
  jobs:
    my_test_job:
      runs-on: ubuntu-latest
      steps:
        - run: echo "Hello from Workflow :)"
  ```

### 3.2. Run the Workflow File

- Navigate back to the `act` directory:
  ```powershell
  cd ..\..
  ```
- Before running a specific job using Act, it's useful to list all available jobs in your workflows. This ensures you know the exact job names and can avoid any typos or mistakes. **List available jobs** in your workflows:
  ```powershell
  act -l
  ```
  Expected output: This will display a list of jobs defined in your workflow files. Once you know the exact job name (`my_test_job`), you can proceed to run it.

- **Run the specific job using Act**:
  ```powershell
  act -j my_test_job
  ```
  When asked, select the Medium size image by using the arrow keys to highlight "Medium" and then pressing Enter.
  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

## 4. Flags of Act

The Act tool comes with several flags that can be used to customize its behavior. These flags allow us to specify different options, such as which Docker image to use for the runner, adding secrets, and more.

### 4.1. Understanding Runners and Docker Images

GitHub Actions uses runners to execute workflows.

A runner is a server that has the GitHub Actions runner application installed. It waits for available jobs, runs them, and then reports the results back to GitHub.

Act replicates this environment locally by using Docker containers as runners. This allows us to test and debug workflows without pushing to GitHub. We can specify which Docker image to use for the runner platform using the `-P` flag.

### 4.2. Commonly Used Flags

Here are some commonly used flags with Act:

- `-P, --platform`: Specify Docker image for a platform.
  ```powershell
  act -j my_test_job -P ubuntu-latest=catthehacker/ubuntu:act-latest
  ```   
  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

- `-e, --eventpath`: Use a custom JSON file for the event payload.

  This allows us to simulate different events, such as pushes or pull requests, without making actual changes to the repository. 

  Here’s an example of an `event.json` file that you can create in Notepad++ and save in the `act` directory of your cloned repo:
  ```json
  {
    "ref": "refs/heads/main",
    "repository": {
      "full_name": "octocat/Hello-World"
    }
  }
  ```

  This command will run the `my_test_job` job using the event data from the `event.json` file:

  ```powershell
  act -j my_test_job -e event.json
  ```

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

- `-s, --secret`: Add secrets to the workflow.

  Adding secrets to our workflow is essential for securely passing sensitive data. This ensures that our information remains protected as it securely passes sensitive data to our jobs.

  We can use the `-s` flag to add secrets to the workflow, and it's helpful to combine it with the `-j` flag.
 
  ```powershell
  act -j my_test_job -s MY_SECRET=secret_value
  ```

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

- `-C, --directory`: Specify the working directory (default: `.`).

  This allows us to run Act from a different directory while specifying the path to our repository.

  The following command will look for the `.github/workflows` directory and workflow files inside the specified `act` directory, then run the `my_test_job` job using the workflows and files located in that directory:

  ```powershell
  act -j my_test_job -C "C:\path_to_your_act_directory"
  ```

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

- `--dryrun`: Validate workflow without running containers.

  This is useful for testing whether the workflow configuration is correct without executing the actual tasks.

  To perform a dry run and specify the job, use the `-j` flag followed by the `--dryrun` flag:

  ```powershell
  act -j my_test_job --dryrun
  ```

  Expected output:
  ```plaintext
  *DRYRUN* [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  *DRYRUN* [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  *DRYRUN* [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  *DRYRUN* [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  *DRYRUN* [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  *DRYRUN* [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  *DRYRUN* [Test Example/my_test_job] Cleaning up container for job my_test_job
  *DRYRUN* [Test Example/my_test_job] 🏁  Job succeeded
  ```

- `-v, --verbose`: Enable verbose logging for detailed output.

  This flag helps in debugging by providing more information about the workflow execution.

  To enable verbose logging and specify the job, use the `-j` flag followed by the `-v` flag:

  ```powershell
  act -j my_test_job -v
  ```

- `--graph, -g`: Visualize workflows as a graph.

  This flag helps to understand the structure and flow of our workflows by providing a visual representation.

  To visualize the workflow and specify the job, use the `-j` flag followed by the `--graph` flag:
  
  ```powershell
  act -j my_test_job --graph
  ```

  Expected output:
  ```plaintext
     ╭─────────────╮
     │ my_test_job │
     ╰─────────────╯
  ```

- `--json`: Output logs in JSON format.

  This flag helps in processing logs programmatically or integrating them with other tools.

  To output logs in JSON format and specify the job, use the `-j` flag followed by the `--json` flag:
  
  ```powershell
  act -j my_test_job --json
  ```

  Expected output:

  - **Initialization**:
  ```json
  {
    "level": "info",
    "msg": "Using docker host 'npipe:////./pipe/docker_engine', and daemon socket 'npipe:////./pipe/docker_engine'",
    "time": "2025-01-05T23:05:33+01:00"
  },
  ```
  
  - **Job Start**:
  ```json
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "🚀  Start image=catthehacker/ubuntu:act-latest",
    "time": "2025-01-05T23:05:33+01:00"
  },
  ```
  
  - **Docker Pull and Run**:
  ```json
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true",
    "time": "2025-01-05T23:05:33+01:00"
  },
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=[\"tail\" \"-f\" \"/dev/null\"] cmd=[] network=\"host\"",
    "time": "2025-01-05T23:05:34+01:00"
  },
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=[\"tail\" \"-f\" \"/dev/null\"] cmd=[] network=\"host\"",
    "time": "2025-01-05T23:05:35+01:00"
  }, 
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=",
    "time": "2025-01-05T23:05:35+01:00"
  },
  ```
  
  - **Step Execution**:
  ```json  
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "⭐ Run Main echo \"Hello from Workflow :)\"",
    "stage": "Main",
    "step": "echo \"Hello from Workflow :)\"",
    "stepID": ["0"],
    "time": "2025-01-05T23:05:35+01:00"
  },
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=",
    "stage": "Main",
    "step": "echo \"Hello from Workflow :)\"",
    "stepID": ["0"],
    "time": "2025-01-05T23:05:35+01:00"
  },
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "Hello from Workflow :)\r\n",
    "raw_output": true,
    "stage": "Main",
    "step": "echo \"Hello from Workflow :)\"",
    "stepID": ["0"],
    "time": "2025-01-05T23:05:35+01:00"
  },
  ```
  
  - **Success**:
  ```json
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "  ✅  Success - Main echo \"Hello from Workflow :)\"",
    "stage": "Main",
    "step": "echo \"Hello from Workflow :)\"",
    "stepID": ["0"],
    "stepResult": "success",
    "time": "2025-01-05T23:05:35+01:00"
  },
  ```
  
  - **Cleanup**:
  ```json
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "level": "info",
    "matrix": {},
    "msg": "Cleaning up container for job my_test_job",
    "time": "2025-01-05T23:05:35+01:00"
  },
  ```
  
  - **Job Result**:
  ```json
  {
    "dryrun": false,
    "job": "Test Example/my_test_job",
    "jobID": "my_test_job",
    "jobResult": "success",
    "level": "info",
    "matrix": {},
    "msg": "🏁  Job succeeded",
    "time": "2025-01-05T23:05:35+01:00"
  }


- `-q, --quiet`: Suppress logging from steps.

  This flag is useful if we prefer a cleaner and more concise output because it enable quiet mode to reduce the verbosity of the command output.

  To enable quiet mode and specify the job, use the `-j` flag followed by the `-q` flag:
  
  ```powershell
  act -j my_test_job -q
  ```
  
  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

### 4.3. Advanced Configuration

#### Bind Working Directory

Binding the working directory ensures that files are not copied into containers. Instead, it creates a bind mount, which directly links the files on your host machine with the container.

This is particularly useful for reducing the overhead of copying files and ensuring that any changes made to the files are immediately reflected in the container.

- **`-b, --bind`**: Bind the working directory to avoid copying files into containers.

  ```powershell
  act -j my_test_job -b
  ```
  
  This command will bind the working directory and run the `my_test_job job`, ensuring that any changes made to the files are immediately reflected in the container.
  
  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
 ```

#### Manage Secrets and Variables

Managing secrets and variables is crucial for securely passing sensitive data and configuration settings to your workflows.

By default, Act allows us to specify secrets and variables directly within the command. However, for better security and maintainability, we can read them from a file.

- **`--secret-file`**: Manage secrets by reading them from a file.

  ```powershell
  act -j my_test_job --secret-file .secrets
  ```
  
  This command reads secrets from the `.secrets` file and runs the `my_test_job job`.
  
  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

- **`--var-file`**: Manage variables by reading them from a file.

  ```powershell
  act -j my_test_job --var-file .vars
  ```

  This command reads variables from the `.vars` file and runs the `my_test_job job`.

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

### 4.4. Optimizations

#### Reuse Containers

Reusing containers maintains state between runs, leading to faster execution times. This is especially useful for iterative development and testing.

- **`-r, --reuse`**: Reuse containers by maintaining state between runs for faster execution.

  ```powershell
  act -j my_test_job -r
  ```

  This command will reuse the containers and run the `my_test_job job`, reducing the time required for subsequent runs.

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

#### Skip Image Pull

Skipping image pull uses locally cached images, avoiding the need to download them. This can significantly speed up the workflow execution if the images are already available locally.

- **`--action-offline-mode`**: Skip image pull and use locally cached images to avoid downloading.

  ```powershell
  act -j my_test_job --action-offline-mode
  ```

  This command will use locally cached images and run the my_test_job job, speeding up the execution by avoiding image downloads.

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=false
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

#### Watch for Changes

Watching for changes automatically runs workflows when files change.

This is useful for continuous development and testing, as it triggers the workflow whenever a change is detected.

- **`-w, --watch`**: Watch for changes and automatically run workflows when files change.

  ```powershell
  act -j my_test_job -w
  ```

  This command will watch for changes and automatically run the `my_test_job` job whenever a file change is detected.

  Expected output:
  ```plaintext
  [Test Example/my_test_job] 🚀  Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job]   🐳  docker pull image=catthehacker/ubuntu:act-latest platform= username= forcePull=true
  [Test Example/my_test_job]   🐳  docker create image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker run image=catthehacker/ubuntu:act-latest platform= entrypoint=["tail" "-f" "/dev/null"] cmd=[] network="host"
  [Test Example/my_test_job]   🐳  docker exec cmd=[node --no-warnings -e console.log(process.execPath)] user= workdir=
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)"
  [Test Example/my_test_job]   🐳  docker exec cmd=[bash -e /var/run/act/workflow/0] user= workdir=
  | Hello from Workflow :)
  [Test Example/my_test_job]   ✅  Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁  Job succeeded
  ```

  When you use the `-w` flag, Act enters watch mode and keeps running in the background, waiting for any changes in the files. It will automatically rerun the workflow whenever a file change is detected.

To exit watch mode and return to the command prompt, you can use `Ctrl+C`. This will stop Act from watching for changes and allow you to continue writing commands.

## Conclusion

Using Act to run GitHub workflows locally offers a robust solution for developers in the DevSecOps framework seeking rapid feedback and efficient task management.

By utilizing Act, the necessity of committing and pushing changes to GitHub for testing is eliminated, as the local environment mirrors GitHub's environment.

Additionally, Act can replace tools like `make` by executing tasks defined within GitHub Actions, thereby streamlining the development process and achieving the intended objective of effective workflow management and automation.
  
## License

## License

This project, ULE_DevOps_my_flask_app - Lab 2: "CI/CD with Act", is part of the course DevSecOps in Autonomous Systems, within the Master's program in Cybersecurity Research at the University of León (ULE). This project is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
