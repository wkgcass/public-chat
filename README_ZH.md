# demo-of-chat-in-issue

fork该仓库，再做一些简单的配置，你就可以在Github issue中使用ChatGPT了！

## 前置要求

1. 你需要有一个Github账号
2. 你需要有一个能够调用API的OpenAI账号

> OpenAI账号刚注册后会有$18的限时免费额度，但如果花完了或者过期了就需要绑美国信用卡才能继续使用了。  
> 本文不涉及OpenAI账号的申请和账单的绑定。

## 第一步: Fork

### 1.1 简单的操作

如果你不会使用git命令，那么可以用这种相对比较简单的办法：  
在本仓库中，点击右上角的`Fork`，在新页面中点击`Create fork`。然后请等待fork完成，一般一分钟内就行。

完成后跳到第二步即可。

### 1.2 使用git

直接fork的仓库不能改为private，所以自行创建仓库再`clone+push`其实会更适合。

首先在Github上创建一个空的private仓库，比如`xxx/chat-in-issue`，注意不要创建.gitignore也不要创建README，保持仓库是空的即可。  
接着在本地机器随便找个地方，输入`git clone https://github.com/wkgcass/demo-of-chat-in-issue`。  
然后输入`git remote add fork ssh:`后面把你的仓库的ssh地址粘贴进去（如果你知道如何配置https gh token的话，相应做一下配置，用https协议也可以）。  
最后输入`git push fork main`。

后续如果想要同步两个仓库的内容，可以执行`git pull origin && git push fork main`

## 第二步：配置仓库

在你fork后的仓库中，点击顶栏的`Settings`。

### 2.1 打开Issue功能

在左侧边栏点击进入`General`。往下翻，找到`Features`里面的`Issues`，勾选`Issues`左边的选框。

此时，在页面顶栏应当能看到出现了`Issues`。

### 2.2 配置白名单

在左侧边栏点击进入`Secrets and variables`，然后点击抽屉中的`Actions`。在右边的`Actions secrets and variables`中点击`Variables`选项卡。点击`New repository variable`。  
在`Name`一栏内输入`CHAT_IN_ISSUE_USER_WHITELIST`，在`Value`一栏内输入符合你Github账号名的正则表达式，格式如下：

首先点击页面最右上角你的Github头像，弹出的菜单中，第一行就是"Signed in as XXX"，末尾那个就是你的Github账号名。比如我的就是wkgcass。  
在你的账号名前面添加`^`，末尾添加`$`，如果你的名字中带有`-`或者`.`，那么需要在这些字符前添加一个反斜杠`\`。  
最后得到的结果，就是你需要在`Value`一栏内输入的内容。

例如我的账号名是`wkgcass`，那么我的`Value`中就需要输入`^wkgcass$`。  
再举个例子，如果我的账号名是`hello-world`，那么我的`Value`中就要输入`^hello\-world$`。

### 2.3 配置OpenAI API Key

还是在`Secrets and variables`的`Actions`中，只不过这次选择`Secrets`选项卡。点击`New repository secret`。  
在`Name`一栏内输入`OPENAI_KEY`，在`Value`一栏内输入你的OpenAI API Key，一般以`sk-`为开头。

### 2.4 配置workflow token权限

在左侧边栏找到`Code and automation`里面的`Actions`，点一下弹出抽屉，点里面的`General`。  
翻到最下面，把`Workflow permissions`的选项修改为`Read and write permissions`，然后点击底下的`Save`。

### 2.5 启用workflow

点击顶栏中的`Actions`。如果是fork的仓库，此时会要求你主动启用workflow。点击`I understand my workflows, go ahead and enable them`即可。

## 第三步：大功告成！测试一下

进入`Issues`页面，点右边的`New Issue`，随便写一个标题，然后在内容中输入`/chat:`  
后面写上你想跟ChatGPT说的内容。  
点击右下角的`Submit new issue`。

这时你可以进入`Actions`页面查看运行过程。一般几十秒后就能执行完成。

回到刚才的issue中，查看ChatGPT给你响应的消息。

## 第四步：关闭通知

用issue时，每次有新的comment都会有邮件通知，比较烦人。这里可以直接把整个仓库的推送关掉。  
点仓库页面右上角那个`Watch`的小按钮，下拉菜单中选择`Ignore`即可。

---

## 第五步：高级配置

如果对话内容比较长，有一些配置可能会影响对话上下文。

可以参考[文档](https://github.com/marketplace/actions/chat-in-issue)修改高级配置。  
修改方式如下：

首先点击顶栏中的`Code`，点击页面中央文件列表中的`.github/workflows`目录。  
进入后，点击`chat-in-issue.yaml`，点击内容框右上角的小铅笔图标来编辑内容。

所有需要修改的内容其实都在最底下那个`with`的里面。按照文档调整配置项，注意缩进，还有注意其值需要填写字符串，即使配置项是数字也必须括在英文引号内。

例如：

```yaml
      - uses: wkgcass/chat-in-issue@v1
        with:
          openai-key: ${{ secrets.OPENAI_KEY }}
          user-whitelist: ${{ vars.CHAT_IN_ISSUE_USER_WHITELIST }}
          prompt-limit: "8000"
          prompt-from-beginning-max: "3000"
```

修改完毕后，点击右上角的`Commit changes...`，在弹出的对话框中点击`Commit changes`即可。

## 第六步：高级交互

请阅读[文档](https://github.com/marketplace/actions/chat-in-issue)中关于`prefix`配置项的内容，那里有一些特殊的prompt用法。
