# Overview

Executable Documentation is a novel approach to simplify the evaluation and adoption of solutions provided with a CLI tool, such as Azure services. It achieves this by providing one-click and interactive learning experiences for deploying recommended architectures on Azure. These experiences utilize Innovation Engine, an open-source project that amplifies standard markdown language such that it can be executed step-by-step in an educational manner and tested via automated CI/CD pipelines.   

Problems
- Content isn't reliable, accurate, or relevant for users. As products change, so should documentation.​
- Content doesn’t span across services and OSS as E2E Solutions.​
- Content doesn't match their expertise level and their products' complexity.​

## How to write an Exec Doc
To write an Exec Doc, follow these steps:

1. [Set up Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) locally in your IDE like VS Code 

2. Set up azure-docs-pr in your local machine 
    
    - Fork the [MicrosoftDocs/azure-docs-pr](https://github.com/MicrosoftDocs/azure-docs-pr) repo, which is where docs changes are made internally. Your fork URL would contain the following within: `<your_github_username>/azure-docs-pr` 

    - Clone a copy of your fork to your local machine 

    - Make any changes to an existing/new doc in an IDE such as VS Code 

    - Push all changes to your fork as necessary 

3. Ensure your doc is a markdown file 

4. Ensure your doc is written with the LF line break type (given at the bottom right corner of your IDE screen) 

    Example: 

5. Ensure doc and all dependencies that it references live under the same parent folder inside your fork 

6. Appropriately add doc metadata at the start of the doc 

    - Mandatory Fields 
        - title = the title of the exec doc
        - description = the description of the exec doc 
        - ms.topic = what kind of document it is e.g. article, blog, etc. 
        - ms.date = the date the doc was last updated by author 
        - author = author's GitHub username 
        - ms.author = author's username (e.g. Microsoft Alias)
        - ms.custom = comma-separated list of tags (innovation-engine, linux-related-content are two tags that need to be in this list) 
        
    - Optional Fields 
        - ms.permissions = an optional comma-separated list of permissions needed to execute the doc  
        - ms.service  
        - ms.collection  
        - ms.reviewer

    Example: 

    ```markdown 
    ---
    title: 'Quickstart: Deploy an Azure Kubernetes Service (AKS) cluster using Azure CLI' 

    description: Learn how to quickly deploy a Kubernetes cluster and deploy an application in Azure Kubernetes Service (AKS) using Azure CLI. 

    ms.topic: quickstart 

    ms.date: 04/09/2024 

    author: tamram 

    ms.author: tamram 

    ms.custom: H1Hack27Feb2017, mvc, devcenter, devx-track-azurecli, mode-api, innovation-engine, linux-related-content 
    ---
    ```

7. Ensure that the doc contains at least 1 code block   

8. Every code block in the doc should be one of the following types: 

    - Azurecli 
    - Bash 
    - Terraform 
    - Azure-cli-interactive 
    - Azurecli-interactive 
    - Console 
    - Output 
    - Json 
    - Yaml 

    Example: 

    ```bash 
    az group create --name $MY_RESOURCE_GROUP_NAME --location $REGION 
    ``` 

9. Author needs to add expected similarity block(s) below relevant code blocks i.e. there should be a block like the one shown below whenever the terminal/cloudshell returns an output for a code block 

    Example: 

    Results: 

    <!-- expected_similarity=0.3 --> 

    ```JSON 
    { 
        <code block> 
    } 
    ``` 
```

10. Every expected similarity block should have all the PII (Personal Identifiable Information) stricken out from it and replaced with x’s 

    Example: 

    Results: 

    <!-- expected_similarity=0.3 --> 

    ```JSON 
    { 
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myAKSResourceGroupxxxxxx", 
        "location": "eastus" 
    } 
    ``` 

11. If you are converting an existing Azure doc to an exec doc and if the existing doc contains a “Delete Resources” or equivalent section and contains a command to delete resources within, remove the command block(s) from that section   

- Tip: If command blocks are retained, Innovation Engine would execute the command(s) and delete the resources, which is something we do not want 

12. Ensure there is a new line before and after every content heading, subheading, description, and code block 

13. Ensure the headings are appropriate 

    - Have only one h1 heading in the doc 
    - You can have multiple h2s and h3s subheadings in the doc  
    - You should not have any h4 headings 

14. Test the doc using Innovation Engine (IE) inside Azure Cloudshell 

- Open Cloudshell at My Dashboard - Microsoft Azure 

[Optional]: Set your active subscription to the one you are using to test exec docs. Ideally, this sub should have permissions to run commands in your tested exec docs. Run the following command: 

    ```bash
    az account set --subscription “<subscription name or id>” 
    ``` 

- Install and set up the latest stable build of Innovation Engine. Run the following command (ensure it is all in one line): 

    ```bash
    curl –Lks https://raw.githubusercontent.com/Azure/InnovationEngine/v0.1.3/scripts/install_from_release.sh | /bin/bash -s -- v0.1.3 
    ``` 

- Test your WIP doc using Innovation Engine. Run the following command: 

```bash
ie execute <URL to the raw exec doc markdown that lives in GitHub, etc.> 
``` 

15. Create a PR in GitHub once a doc is ready to be uploaded, pointing to the upstream repo [MicrosoftDocs/azure-docs-pr](https://github.com/MicrosoftDocs/azure-docs-pr) 

16. Assign the original doc author (if not you) as a reviewer in the PR 

17. Add “#sign-off"  in the PR comments once the doc is successfully reviewed  

18. Confirm that the PR has merged into the public repo after (upto) ~24 hours: [MicrosoftDocs/azure-docs](https://github.com/MicrosoftDocs/azure-docs) 

*Tip: If you are converting an existing Azure doc, save the raw markdown text in a file 

Remember, the goal of an Exec Doc is to provide a hands-on learning experience, so focus on practical examples and actionable steps.


## What is Executable Documentation? 
Executable documentation takes standard markdown language and amplifies it by 
allowing the code commands within the document to be executed in full or step by step in an educational manner, and tested 
via automated CI/CD pipelines.

## Install Innovation Engine CLI
To install the Innovation Engine CLI, run the following commands. To install a specific version, set VERSION to the desired release number, such as "v0.1.3".
You can find all releases [here](https://github.com/Azure/InnovationEngine/releases).

```bash
VERSION="latest"
wget -q -O ie https://github.com/Azure/InnovationEngine/releases/download/$VERSION/ie

# Setup permissions & move to the local bin
chmod +x ie
mkdir -p ~/.local/bin
mv ie ~/.local/bin
```

## Build Innovation Engine from Source
Paste the following commands into the shell. This will 
clone the Innovation Engine repo, install the requirements, and build out the 
Innovation Engine executable.

```bash
git clone https://github.com/Azure/InnovationEngine;
cd InnovationEngine;
make build-ie;
```

Now you can run the Innovation Engine tutorial with the following 
command:

```bash
./bin/ie execute tutorial.md
```

# How to Use Innovation Engine
The general format to run an executable document is: 
`ie <MODE_OF_OPERATION> <MARKDOWN_FILE>`

## Modes of Operation
Today, executable documentation can be run in 3 modes of operation:

Interactive: Displays the descriptive text of the tutorial and pauses at code 
blocks and headings to allow user interaction 
`ie interactive tutorial.md`

Test: Runs the commands and then verifies that the output is sufficiently 
similar to the expected results (recorded in the markdown file) to be 
considered correct. `ie test tutorial.md`

Execute: Reads the document and executes all of the code blocks not pausing for 
input or testing output. Essentially executes a markdown file as a script. 
`ie execute tutorial.md`

## Use Innovation Engine with any URL

Documentation does not need to be stored locally in order to run IE with it. With v0.1.3 and greater, you can run `ie execute`, `ie interactive`, and `ie test` with any URL that points to a public markdown file, including raw GitHub URLs. See the below demo:

https://github.com/Azure/InnovationEngine/assets/55719566/ce37f53c-9876-42b9-a033-1e4acaeb9d50

## Use Executable documentation for Automated Testing
One of the core benefits of executable documentation is the ability to run 
automated testing on markdown file. This can be used to ensure freshness of 
content.

In order to do this one will need to combine innovation engine executable 
documentation syntax with GitHub actions. 

In order to test if a command or action ran correctly executable documentation 
needs something to compare the results against. This requirement is met with 
result blocks.

### Result Blocks
Result blocks are distinguished in Executable documentation by a custom 
expected_similarity comment tag followed by a code block. For example

<!!--expected_similarity=0.8-->
<!--expected_similarity=0.8-->
```text
Hello world
```
This example purposely breaks the comment syntax so that it shows up in 
markdown. Otherwise, the tag of expected_similarity is completely invisible.

The expected similarity value is a floating point number between 0 and 1 which 
specifies how closely the output needs to match the results block. 0 being no 
similarity, 1 being an exact match.

>**Note** It may take a little bit of trial and error to find the exact value for expected_similarity.

### Environment Variables

You can pass in variable declarations as an argument to the ie CLI command using the 'var' parameter. For example:
```bash
ie execute tutorial.md --var REGION=eastus
```

CLI argument variables override environment variables declared within the markdown document,
which override preexisting environment variables.

Local variables declared within the markdown document will override CLI argument variables.

Local variables (ex: `REGION=eastus`) will not persist across code blocks. It is recommended
to instead use environment variables (ex: `export REGION=eastus`).

### Setting Up GitHub Actions to use Innovation Engine

After documentation is set up to take advantage of automated testing a github 
action will need to be created to run testing on a recurring basis. The action 
will simply create a basic Linux container, install Innovation Engine 
Executable Documentation and run Executable documentation in the Test mode on 
whatever markdown files are specified.

It is important to note that if you require any specific access or cli tools 
not included in standard bash that will need to be installed in the container. 
The following example is how this may be done for a document which runs Azure 
commands.

```yml
name: 00-testing

on:
  push:
    branches:
    - main

  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Deploy
      env:
        AZURE_CREDENTIALS: ${{ secrets.AZURE_CREDENTIALS }}
        GITHUB_SHA: ${{ github.sha }}
      run: |
        cd $GITHUB_WORKSPACE/
        git clone https://github.com/Azure/InnovationEngine/tree/ParserAndExecutor
        cd innovationEngine
        pip3 install -r requirements.txt
        cp ../../articles/quick-create-cli.md README.md
        python3 main.py test README.md
```

## Contributing

This is an open source project. Don't keep your code improvements,
features and cool ideas to yourself. Please issue pull requests
against our [GitHub repo](https://github.com/Azure/innovationengine).

Be sure to use our Git pre-commit script to test your contributions
before committing, simply run the following command: `python3 main.py test test`

This project welcomes contributions and suggestions.  Most
contributions require you to agree to a Contributor License Agreement
(CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit
https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine
whether you need to provide a CLA and decorate the PR appropriately
(e.g., label, comment). Simply follow the instructions provided by the
bot. You will only need to do this once across all repos using our
CLA.

This project has adopted
the
[Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see
the
[Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with
any additional questions or comments.


## Trademarks

This project may contain trademarks or logos for projects, products, or 
services. Authorized use of Microsoft trademarks or logos is subject to and 
must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must 
not cause confusion or imply Microsoft sponsorship. Any use of third-party 
trademarks or logos are subject to those third-party's policies.

## Microsoft Open Source Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.