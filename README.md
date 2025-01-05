# ULE_DevSecOps_my_workflows
ULE_DevSecOps_my_workflows - Lab 2: "CI/CD with Act"

## Table of Contents
1. [Introduction](#introduction)
2. [Working with Act](#working-with-act)
3. [Running Workflows with Act](#running-workflows-with-act)
4. [Flags of Act](#flags-of-act)
5. [Use Cases](#use-cases)
6. [Conclusion](#conclusion)
7. [License](#license)

## 1. Introduction
The aim of this repo is to demonstrate the setup and usage of the Act tool, which allows us to run GitHub Actions locally.

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
  [Test Example/my_test_job] 🚀 Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)" 
  [Test Example/my_test_job] ✅ Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁 Job succeeded
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
  [Test Example/my_test_job] 🚀 Start image=catthehacker/ubuntu:act-latest
  [Test Example/my_test_job] ⭐ Run Main echo "Hello from Workflow :)" 
  [Test Example/my_test_job] ✅ Success - Main echo "Hello from Workflow :)"
  [Test Example/my_test_job] Cleaning up container for job my_test_job
  [Test Example/my_test_job] 🏁 Job succeeded
  ```



- `-e, --eventpath`: Use a custom JSON file for the event payload.
  ```powershell
  act -e event.json
  ```

- `-s, --secret`: Add secrets to the workflow.
  ```powershell
  act -s MY_SECRET=secret_value
  ```

- `-C, --directory`: Specify the working directory (default: .).
  ```powershell
  act -C path/to/your/directory
  ```

- `--dryrun`: Validate workflow without running containers.
  ```powershell
  act --dryrun
  ```

- `-v, --verbose`: Enable verbose logging for detailed output.
  ```powershell
  act -v
  ```

- `--graph, -g`: Visualize workflows as a graph.
  ```powershell
  act -g
  ```

- `--json`: Output logs in JSON format.
  ```powershell
  act --json
  ```

- `-q, --quiet`: Suppress logging from steps.
  ```powershell
  act -q

  ```
  

