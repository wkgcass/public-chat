# Demo of chat-in-issue

Fork this repository and do some simple configuration, and you can use ChatGPT in Github issue!

## Prerequisites

1. You need to have a Github account
2. You need to have an OpenAI account that can call APIs

> After registering an OpenAI account, there is a $18 time-limited free quota, but if it is spent or expired, you need to bind a US credit card to continue using it.
> This article does not involve the application of OpenAI account and the binding of bills.

## Step I: Fork

### 1.1 Simple operation

If you don't know how to use git commands, you can use this relatively simple method:
In this repository, click on `Fork` in the upper right corner, and then click `Create fork` in the new page. Please wait for the fork to complete, generally within one minute.

After completion, proceed to step two.

### 1.2 Use git

Directly forked repositories cannot be changed to private, so it is actually more suitable to create a repository by yourself and `clone+push`.

First, create an empty private repository on Github, such as `xxx/chat-in-issue`, be careful not to create .gitignore or README, keep the repository empty.  
Then find a place on your local machine, enter `git clone https://github.com/wkgcass/demo-of-chat-in-issue`.  
Then enter `git remote add fork ssh:` and paste the ssh address of your repository after it (if you know how to configure https gh token, do the corresponding configuration, using the https protocol is also possible).  
Finally, enter `git push fork main`.

If you want to synchronize the content of the two repositories later, you can execute `git pull origin && git push fork main`.

## Step II: Configure the repository

In the repository you forked, click `Settings` on the top bar.

### 2.1 Enable Issue function

Click into `General` in the left sidebar. Scroll down to the `Issues` in the `Features`, check the checkbox on the left of `Issues`.

Now, you should be able to see `Issues` appearing on the top of the page.

### 2.2 Configure whitelist

Click into `Secrets and variables` in the left sidebar, then click `Actions` in the drawer. Click the `Variables` tab in the `Actions secrets and variables`. Click `New repository variable`.  
In the `Name` field, enter `CHAT_IN_ISSUE_USER_WHITELIST`, and in the `Value` field, enter a regular expression that matches your Github account name. The format is as follows:

First click on your Github avatar in the upper right corner of the page, and the first line in the pop-up menu is "Signed in as XXX", the last one is your Github account name. For example, mine is wkgcass.  
Add `^` in front of your account name and `$` at the end. If your name contains `-` or `.` characters, you need to add a backslash `\` before these characters.  
The result is what you need to enter in the `Value` field.

For example, if my account name is `wkgcass`, then I need to enter `^wkgcass$` in the `Value`.  
For another example, if my account name is `hello-world`, then I should enter `^hello\-world$` in the `Value`.

### 2.3 Configure OpenAI API Key

In `Secrets and variables`, still in `Actions`, select the `Secrets` tab. Click `New repository secret`.  
In the `Name` field, enter `OPENAI_KEY`, and in the `Value` field, enter your OpenAI API Key, usually starting with `sk-`.

### 2.4 Configure workflow token permissions

In the left sidebar, click into `Code and automation`, then `Actions`, a drawer pops up, click into `General`.  
Scroll to the bottom, change the option of `Workflow permissions` to `Read and write permissions`, then click `Save` at the bottom.

### 2.5 Enable workflow

Click `Actions` on the top bar. If it is a forked repository, you will be asked to enable the workflow actively. Click `I understand my workflows, go ahead and enable them`.

## Step III: Success! Try it out

Enter the `Issues` page, click `New Issue` on the right, write a title, then enter `/chat:` in the content.  
Write what you want to say to ChatGPT after that.  
Click `Submit new issue` in the lower right corner.

Now you can enter the `Actions` page to view the running process. It usually takes tens of seconds to complete.

Go back to the issue you just created, and check the response message from ChatGPT.

## Step IV: Turn off notifications

When using issue, there will be email notifications every time there is a new comment, which is annoying. Here, you can turn off push for the entire repository.  
Click the `Watch` button in the upper right corner of the repository page, and select `Ignore` in the dropdown menu.

---

## Step V: Advanced configuration

If the dialogue content is relatively long, some configurations may affect the dialogue context.

Refer to the [documentation](https://github.com/marketplace/actions/chat-in-issue) to modify advanced configuration.  
The modification method is as follows:

First, click on `Code` on the top bar, then click the `.github/workflows` directory in the center of the page.  
Then, click `chat-in-issue.yaml`, and click the small pencil icon on the right of the content box to edit the content.

All the contents that need to be modified are actually in the `with` at the bottom. Adjust the configuration items according to the documentation, pay attention to indentation, and note that the value needs to be filled in a string, even if the configuration item is a number, it must be enclosed in double quotation marks.

For example:

```yaml
      - uses: wkgcass/chat-in-issue@v1
        with:
          openai-key: ${{ secrets.OPENAI_KEY }}
          user-whitelist: ${{ vars.CHAT_IN_ISSUE_USER_WHITELIST }}
          prompt-limit: "8000"
          prompt-from-beginning-max: "3000"
```

After modification, click the `Commit changes...` on the upper right corner, then click `Commit changes` in the pop-up dialog. 

## Step VI: Advanced interaction

Please read the contents of the `prefix` configuration item in the [documentation](https://github.com/marketplace/actions/chat-in-issue), which has some special prompt usage.
