# SFDX Source Driven Development Training: Source to Org
 
The goal of this tutorial is to get hands on experience with foundational aspects of the Salesforce DX workflow. It's intended to be written for anyone to be able to pick up the tools and follow along without any prior experience.
 
We'll focus on setting up your development environment, pulling metadata from a source repository, understanding the basic pieces of SFDX project structure, pushing metadata to a Salesforce Org, pulling changes from the Org back to your local machine, and committing and pushing this work from a local branch to a GitHub repository.
 
## Prerequisites
 
### Sign Up for a Dev Org
 
Please sign up for a free salesforce.com developer 'Dev' org, do not use your work sandbox orgs.
Dev Orgs are free, full-featured Salesforce environments that allow you to develop and test Salesforce work. This tutorial requires access to a Dev Org. If you don't have one, you can sign up here: **https://developer.salesforce.com/signup**
 
### Install the Pre-Requisites 
 
We'll need to install a few pre-requisite applications:
- ![Microsoft Visual Studio Code](https://code.visualstudio.com/) 
- ![Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli) 
- ![Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
 
Microsoft Visual Studio Code has a series of extensions built by Salesforce and tailored for Salesforce DX. They provide an interface for working with the Salesforce CLI.
 
To install this extension, open Visual Studio Code and press `Control + Shift + X` to open the Extensions sidebar.
- Search for and install the Salesforce Extension Pack (Expanded).
 
![VS Code Extension Pack](/screenshots/vs_code_extension_pack.PNG)
  
### Create an SFDX Project in VS Code
 
Proceed with this step only after installing Microsoft Visual Studio Code.
 
A Salesforce DX project consists of a specific file structure and a configuration file that identifies a directory as a Salesforce DX project. It is the first step in setting up your development environment.
 
###### Creating an SFDX Project can be done directly in VS Code
 
1. Navigate to `View --> Command Palette --> SFDX: Create Project`<p>&nbsp;</p>
 
![SFDX create project](/screenshots/sfdx_create_new_project.PNG)
 
2. Select the Standard project template.<p>&nbsp;</p>
 
![SFDX Standard Project](/screenshots/sfdx_project_standard.PNG)
 
3. Provide a name for your project.<p>&nbsp;</p>
 
    A Windows Explorer or Finder window will open and ask you where you would like to create your project. The recommendation is to create your project in a local folder. For example, **<span style="color:DarkGreen">c:/(msid)/repositories/</span>** for Windows or **<span style="color:DarkGreen">/Users/(msid)/repositories</span>** for Mac OS.
>A note on naming: For this training, we recommend a name that identifies your project as being tied to SFDX training. For projects related to specific environments, we recommend naming your project after the name of the Org its tied to.
 
##### Documentation
 
- [Project Setup | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_workspace_setup.htm)
- [Create a Salesforce DX Project | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.238.0.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_create_new.htm)
- [Salesforce DX Project Structure and Source Format | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_source_file_format.htm)
 
 
### Authorize an Org in VS Code
 
The next step is to connect a Salesforce Org to your SFDX Project. This will enable you to communicate between your project and the chosen Org. This process is called authorizing an Org.
 
There are multiple ways to do this:
 
1. Navigate to `View --> Command Palette --> SFDX:Authorize an Org`<p>&nbsp;</p>
 
![Command Palette Authorize](/screenshots/sfdx_authorize_org.PNG)
>There are four authorization options based on the type of Org.
       - **Project Default:** This is the URL listed inthe project's `sfdx-project.json` file. It defaults to `login.salesforce.com` when the project is created.
        - **Production:** Choosing this will open a browser tab to `login.salesforce.com`.
        - **Sandbox:** Choosing this will open a browser tab to `test.salesforce.com`.
        - **Custom:** Choosing this will prompt you to provide a custom URL (used more with Scratch Orgs).
 
- For the purposes of this training, choose Production (`login.salesforce.com`), because you will be logging into your Dev Org.<p>&nbsp;</p>
- Next, you'll be prompted to enter an Alias for the authorized Org. A best practice is to use the name of the Org as the alias. This will make it easy to quickly identify Orgs you want to connect to in the future. For this training, a recommended alias would be 'MyDevOrg'.<p>&nbsp;</p>  
 
2. In the VS Code Status bar at the bottom of the application window.<p>&nbsp;</p>
![SFDX Status Bar](/screenshots/no_default_org.PNG)
    >When you first create a project the status bar will contain a No Default Org Set indicator. Clicking on that will bring you to the option to authorize an Org and show all the Orgs you've previously authorized. This list of Orgs can be used to quickly change your default or target Org.
    - After authorizing a new Org or choosing an existing authorized Org, the name of the Org alias will be displayed in the status bar. Additionally, a window icon to the left of the Org name will open the Org in a new browser tab.
    ![Default Org Set](/screenshots/default_org_set_status_bar.PNG)
 
 
##### Documentation
 
- [Authorize an Org Using the Web Server Flow | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_web_flow.htm)
- [Change or Open Your Default Org | Salesforce Extensions for Visual Studio Code](https://developer.salesforce.com/tools/vscode/en/user-guide/default-org)
 
### Connect to a Repository
 
Now that VS Code and a Salesforce Org can communicate with one another, the next step is to connect VS Code to a source control repository.
 
This tutorial's source control is maintained in GitHub, and a GitHub repository will be the source of truth for your Salesforce Org. This means changes to an Org's metadata will be reflected in source control first, and then deployed to testing environments and production, rather than reconciling lower environments via a sandbox refresh after a deployment to production.
 
I have created a GitHub repository to use for this training and documentation: **https://github.com/jpc-79/sfdx_example**
 
To connect to this repository in your VS Code SFDX project follow these instructions:
 
1. Open the VS Code Terminal via `Terminal --> New Terminal` in the menu bar.<p>&nbsp;</p> ![Git New Terminal](/screenshots/new%20terminal.PNG)
 
2. From the terminal command line, type **<span style="color:MediumBlue">git init</span>** and press Enter.<p>&nbsp;</p>   ![Git Init](/screenshots/git_init.PNG)
 
 
    >This is a command to have the git software start tracking all changes in your project directory.
 
    ![Git Init](/screenshots/git_init.PNG)
 
3. From the GitHub repository **([link](https://github.com/jpc-79/sfdx_example))**, click the green button labeled **<span style="color:ForestGreen">Code</span>**, and copy the HTTPS URL.<p>&nbsp;</p>
<p align=center>![Copy Code 1](/screenshots/coe_github.PNG)
 
![Copy Code 2](/screenshots/github_greencode.PNG)
 
![Copy Code 3](/screenshots/github_greencode_url.PNG)</p>
 
4. Again from the Terminal, type:<p>&nbsp;</p>
 
    **<span style="color:MediumBlue">git remote add origin `https://github.com/jpc-79/sfdx_example.git`</span>**<p>&nbsp;</p> ![git add remote](/screenshots/terminal_git_add_remote.PNG)
 
    >The word <span style="color:ForestGreen">origin</span> here is just a short alias you can use to reference the repository easily. It can be anything, but <span style="color:ForestGreen">origin</span> is the word implicitly used by git, so we'll continue to use it here.
 
    ![Git Remote Add](/screenshots/terminal_git_add_remote.PNG)
 
5. Type **<span style="color:MediumBlue">git remote -v</span>** to confirm that you have successfully added the remote repository to your SFDX project.<p>&nbsp;</p>
    > This command outputs the url of the repository you're connected to and should match the COE repository linked above.
 
    ![Git Remote V](/screenshots/terminal_git_add_remote_v.PNG)
 
6. Next, from the Terminal type **<span style="color:MediumBlue">git fetch</span>**:<p>&nbsp;</p>  
    >This command downloads (fetches!) all the commits and files from a remote repository (the one hosted by GitHub) to a local repository (the one you're connected to in VS Code).
 
7. The last git command to run from the terminal is **<span style="color:MediumBlue">git checkout main -f</span>**<p>&nbsp;</p> ![git checkout](/screenshots/git%20checkout.PNG) 
    >This command switches to the main branch of the repository. The main branch is the highest-level branch in the repository, and the one where new work will be merged into.
   
    >The `-f` is short for force. This prevents git from asking you if you're sure you want to overwrite the contents you haven't saved on one branch with new ones it is bringing in from the new branch.
 
8. In the lower left of the VS Code status bar, you should see the word **<span style="color:ForestGreen">main</span>** to signify that you are connected to the main branch of this repository. ![git status bar](/screenshots/git%20branch%20status%20bar.PNG)
 
##### Documentation
 
- [Git Init | GitHub Docs](https://github.com/git-guides/git-init)
- [Managing Remote Repositories | GitHub Docs](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)
- [Git Remote | GitHub Docs](https://github.com/git-guides/git-remote)
- [Visual Studio Code User Interface | Visual Studio Code](https://code.visualstudio.com/docs/getstarted/userinterface)
 
### Create a Branch
 
A key advantage of source control is the ability to easily and quickly create unique copies of metadata in a repository. This is done via branches.
 
If we start with the main branch of a repository, we can create a new branch from that, which is an exact copy of main. From this new branch, we can add development that is unique to our own work (e.g., a user story).
 
When we are done with work on our own branch, we can request to merge it back into the main branch. Once individual branches are merged into the main branch, everyone has access to everyone else's new development.
 
Follow the steps below to create a new branch for this tutorial. This new branch will be created from the repository's main branch.
 
1. From the Terminal, (`Terminal --> New Terminal` in the menu bar), type
 
**<span style="color:MediumBlue">git checkout -b dx_demo_yourlastname</span>** (replace the last part with your own last name).
>A clearly defined branch naming convention is critical to organizing branches in a repository. In this example, the first part of the branch name identifies the purpose, and the last part identifies whose work it is.
 
![git checkout new branch](/screenshots/git%20checkout%20new%20branch.PNG)
![git checkout new branch](/screenshots/git%20checkout%20new%20branch_output.PNG)
>A common naming convention for feature branches could be US123456-lastname. Here the work is tied to a user story and the person doing the work.
 
2. When you create a new branch or switch to one, the VS Code Status Bar will update the lower left with the new branch you are working on.![git checkout new branch](/screenshots/git%20branch%20status%20bar.PNG)
 
Once the Status Bar updates with your new branch, your development environment is now connected to your Dev Org.
 
##### Documentation
 
- [Using Source Control in Your Codespace](https://docs.github.com/en/codespaces/developing-in-codespaces/using-source-control-in-your-codespace)
- [About Branches | GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches)
- [Managing Branches | GitHub Docs](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/making-changes-in-a-branch/managing-branches)
- [Git Cheat Sheet | GitHub](https://education.github.com/git-cheat-sheet-education.pdf)
 
### Create a Custom Object in Dev Org
 
Now that your development environment is connected to a unique branch, it's time to walk through how new development can be created, saved to a branch, and merged into a shared branch.
 
The first step is creating new metadata. Keep it simple for this tutorial:
1. Open your Dev Org (click the window icon next to your Dev Org alias in the VS Code Status Bar to open in a browser).<p>&nbsp;</p>
2. Navigate to Setup and create a new Custom Object. Name it whatever you want.
 
##### Documentation
- [Create a Custom Object in Lightning Experience](https://help.salesforce.com/s/articleView?id=sf.dev_objectcreate_task_lex.htm&type=5)
 
### Retrieve Custom Object from Dev Org to VS Code
 
The repository is the source of truth for an Org. If new metadata is added in development, it needs to be brought back into source control from wherever it was created. This section will walk through how to pull your new Custom Object down from your Dev Org and into VS Code.
 
Here are two ways to do this:
 
##### Org Browser
 
1. The Org Browser is available from the Explorer bar in VS Code (the Explorer bar defaults to the left side of the application window).<p>&nbsp;</p> ![org browser](/screenshots/org_browser_menu.PNG)
2. The Org Browser window shows all the possible types of metadata in the default Org (in this case, your Dev Org).<p>&nbsp;</p>
3. Scroll down to Custom Objects and expand the directory.<p>&nbsp;</p> ![org browser icons](/screenshots/org_browser_icons.PNG)
    >Notice there are two icons when you hover over the name of the metadata directory. One is for refreshing the list of components within the default Org, and the other is to retrieve the metadata of this type from the default Org to your VS Code.
4. Find the Custom Object you created in your Dev Org and click the cloud icon to retrieve the object's metadata into VS Code.<p>&nbsp;</p>
5. Next go back to the SFDX project file tree in the Explorer bar. Go to `force-app > main > default > objects` and confirm that your new Custom Object is listed in the directory.
 
##### Terminal
 
1. The other way to retrieve metadata from an Org is via the command line in VS Studio's Terminal.<p>&nbsp;</p>
2. From the menu bar, go to `Terminal > New Terminal`.<p>&nbsp;</p>
3. From the command line, type **<span style="color:MediumBlue">sfdx force:source:retrieve -m CustomObject:API_Name_of_Your_Custom_Object__c</span>**<p>&nbsp;</p> ![SFDX Retrieve](/screenshots/sfdx-force_source_retrieve.PNG)
    >So, if your Custom Object is named Campfire, you would type:
    **<span style="color:MediumBlue">sfdx force:source:retrieve -m CustomObject:Campfire__c</span>**
 
4. The Terminal will provide a status bar to show the retrieval progress. Once it has successfully been retrieved, go to the SFDX project file tree in the Explorer bar (`force-app > main > default > objects`) and confirm that your Custom Object is listed in the directory.
 
##### Documentation
 
- [Org Browser | Salesforce for VS Code](https://developer.salesforce.com/tools/vscode/en/user-guide/org-browser)
- [source Commands | Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_force_source.htm)
 
 
### Commit Your Custom Object to Your Branch with Git
 
Now that the Custom Object metadata has been pulled from the Dev Org into VS Code, it's important to save that change to your branch. In git terminology, this is called committing the work.
 
To commit the addition of your Custom Object, open the Terminal (`Terminal > New Terminal`) and on a new command line type: **<span style="color:MediumBlue">git status</span>** ![Git Status](/screenshots/1git_status.png)
 
What gets returned is the changes to the project directory the git application has noticed since you first ran the **<span style="color:MediumBlue">git init</span>** command.
 
In this case, you should only have your Custom Object listed. There is one thing that needs to happen before you can commit your change. Git requires the changes to be staged first. Think of it as taking what you want to commit and putting it aside from all the things you don't want to change.
 
To stage your changes for commit, type on a new command line:
 
**<span style="color:MediumBlue">git add force-app/main/default/objects/api_name_of_your_custom_object__c</span>** ![Git Add](/screenshots/2git_add.png)
 
Next, from the command line again, type: **<span style="color:MediumBlue">git status</span>** ![Git Status](/screenshots/3git_status_after_add.png)
 
What returns this time is the path to your Custom Object but with green text. Files or directories returned by git in green text are ready to be committed.
 
An important part of committing work to a branch is identifying the change you're adding to the repository. This is done in a `git commit` message. A brief and descriptive commit message helps someone else easily understand the reason for your commit.
 
To commit your change to your branch, type the following:
 
**<span style="color:MediumBlue">git commit -m "add (name of your custom object)"</span>** ![Git Commit](/screenshots/4git_commmit.png)
 
This works for this tutorial, but when you're working with your teams' Salesforce Org, the best practice would be to start each commit with the User Story number tied to the changes you're committing.
 
If you type **<span style="color:MediumBlue">git status</span>** again, it will show a clean working directory with 1 commit ready to be pushed to the repository. ![Git Status](/screenshots/5git-status_after_commit.png)
 
Before pushing this change from your local branch to the repository in GitHub, you'll make one more change.
 
##### Documentation
 
- [Git Status | Git Guides, GitHub](https://github.com/git-guides/git-status)
- [Git Add | Git Guides, GitHub](https://github.com/git-guides/git-add)
- [Git Commit | Git Guides, GitHub](https://github.com/git-guides/git-commit)
 
### Create Apex Class and Deploy to Dev Org
 
Developers will spend more time working on code that's created and maintained in VS Code compared to declarative changes from within an Org. Though this code may be written in VS Code, it needs to get into the Org in order to be tested.
 
This process is straightforward with SFDX and VS Code. To demonstrate, you will create an empty Apex Class, deploy it to your Dev Org, and commit the new class to your local branch.
 
1. From VS Code Menu Bar: `View --> Command Palette --> SFDX: Create Apex Class`<p>&nbsp;</p>
    >You'll be prompted to give your class a name. Give it any name you want.
    >- Select the default placement `force-app/main/default/classes`
    >- Confirm in the explorer tab that your new class has been created.
   
   ![Apex Class](/screenshots/VSCode_SFDXCreateApex.png)
   ![Default Dir](/screenshots/VSCode_SelectDefaultDirectory.png)
   ![Name Apex Class](/screenshots/VSCode_NameApex.png)
 
2. From `force-app/main/default/classes/` in the Explorer tab, right click the name of your new class.<p>&nbsp;</p> ![Find Apex Class in Explorer](/screenshots/VSCode_Apex_Class.png)
![Find Apex Class in Explorer](/screenshots/VSCode_DeploySourceToOrg.png)
    >In the right-click menu, choose `SFDX:Deploy Source to Org`
        >- This deploys the selected metadata to your default Org.
3. Open your Dev Org (click the windows icon next to your Dev Org alias in the VS Code status bar).<p>&nbsp;</p>
    - Navigate to `Setup > Apex Classes`<p>&nbsp;</p>
    - Confirm that your Apex Class deployed to your Dev Org.<p>&nbsp;</p>
4. Commit new Apex Class to local branch.<p>&nbsp;</p>
    - From the Terminal `(Terminal --> New Terminal)`, type and run **<span style="color:MediumBlue">git status</span>**<p>&nbsp;</p> ![Git Status Apex Class](/screenshots/Git_Status_Apex.png)
![Git Add All](/screenshots/Git_Add_All.png)
![Git Status After Add All](/screenshots/Git_Status_After_Add_Apex.png)
![Git Commit Apex](/screenshots/Git_Commit_Apex.png)
 
        >- If the new Apex class is in red text and the only metadata listed, type and run:
        **<span style="color:MediumBlue">git add --all</span>**
        >- If there are other metadata in red text, type and run:
        **<span style="color:MediumBlue">git add force-app/main/default/classes/api_name_of_class.cls</span>**
        >- Type and run **<span style="color:MediumBlue">git status</span>** again and confirm that your Apex Class is listed in green text.
        >- To commit this new class to your branch, type and run:
        **<span style="color:MediumBlue">git commit -m "add new apex class"</span>**
 
##### Documentation
 
- [Create an Apex Class | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_develop_create_apex.htm)
- [Develop Against Any Org | Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_develop_any_org.htm)
 
### Push Local Branch to GitHub Repository
 
New that both the new Custom Object and Apex Class have been committed to your local branch, the next step is to push the local branch up to the repository in GitHub from your VS Code.
 
This step is important because it begins the process of adding your new changes to the shared source of truth you started with when you added the remote to VS Code.
 
1. From the Terminal `(Terminal > New Terminal)`, type and run the following command:
**<span style="color:MediumBlue">git push -u origin name-of-your-local-branch</span>**<p>&nbsp;</p> ![Git Push Apex](/screenshots/Git_Push_Apex_Class.png)
 
2. The command line will provide feedback on the status of your push and tell you that your local branch is now set up to track with the version of the branch we moved into the repository.<p>&nbsp;</p>
 
3. Navigate to the DX Demo repository **[(link)](https://github.optum.com/SalesforceCoE/DX_Demo)**.<p>&nbsp;</p>
    >A yellow banner will appear displaying that your local branch had recent pushes to the repository.
4. Click the green `Compare & Pull Request` button.<p>&nbsp;</p>  ![Git Compare](/screenshots/Git_Compare_And_Pull.png)
![Git Create Pull Request](/screenshots/Git_Create_Pull_Request2.png)
 
    >A pull request is a process of source-driven development that initiates merging one branch into another.
   
    >Pull requests are a way of flagging the changes you want to add to a shared branch, like the main branch.
   
    - On the Open a Pull Request screen, add your name and a descriptive subject for the pull request. Then add a comment describing your changes.<p>&nbsp;</p>
   
    - Click the green `Create Pull Request` button.
 
In an actual source-driven development process, someone besides you will review your pull request to perform code reviews, quality analysis, and confirm that what is intended to be merged is what will be merged.
 
The reviewer might flag work for you to review or update, in which case you would go back to your Org or VS Code and follow the smae retrieval/deployment, commit/push process outlined above.
 
Once the reviewer is satisfied with the changes, they will approve the pull request and the development from your branch will be merged into the shared source of truth.
 
##### Documentation
 
- [Git Push | Git Guides, GitHub](https://github.com/git-guides/git-push)
- [Pushing Commits to a Remote Repository](https://docs.github.com/en/get-started/using-git/pushing-commits-to-a-remote-repository)
- [About Pull Requests | GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [Creating a Pull Request | GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- [Merging a Pull Request with a Merge Queue | GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)

This e-mail, including attachments, may include confidential and/or
proprietary information, and may be used only by the person or entity
to which it is addressed. If the reader of this e-mail is not the intended
recipient or intended recipient’s authorized agent, the reader is hereby
notified that any dissemination, distribution or copying of this e-mail is
prohibited. If you have received this e-mail in error, please notify the
sender by replying to this message and delete this e-mail immediately.
