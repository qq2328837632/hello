# hello

记录一下如何将md文件上传到hello仓库

```
 git pull origin main --allow-unrelated-histories
 可以使用--allow-unrelated-histories选项来允许合并两个无关的历史。
 这将允许合并两个无关的历史，然后将远程仓库的更改拉取到本地仓库。

注意：在执行这个命令之前，请确保你已备份了重要的代码和仓库，以防止意外的数据丢失。
```

```
首先，确保你在进行推送之前已经将远程仓库的最新更改同步到本地仓库。可以使用以下命令将远程仓库的更改拉取到本地仓库：

git pull origin main
这将自动合并远程仓库的更改和你本地仓库的提交记录。

如果拉取操作成功完成并且没有产生冲突，再次尝试执行推送操作：

git push -u origin main
现在，应该能够成功将本地代码推送到远程仓库中。
后面的更新




如果你想要将多个文件一次性推送到远程仓库而不进行逐个文件的暂存，你可以使用以下命令来简化操作:

在你的本地项目目录中，将所有要添加的文件放置在 _posts 文件夹中。

打开命令行或终端，并进入项目目录。

直接提交所有文件的更改。运行以下命令：

git commit -am "Add multiple posts"
这会自动将所有已修改的文件添加到暂存区并提交更改。注意，这种方法只适用于已跟踪（tracked）的文件。

推送你的更改到远程仓库。运行以下命令：

git push origin main
这会将你的提交推送到名为 origin 的远程仓库的 main 分支。如果你的远程分支不是 main，请相应地修改命令。

通过使用 -a 选项，git commit 命令将自动添加所有已修改的文件到暂存区（不需要显式使用 git add），然后进行提交。
但请注意，这种方法只适用于已经被Git跟踪（已经被添加到版本控制）的文件。如果有新的文件需要添加到追踪列表中，仍然需要使用 git add 命令将它们添加到暂存区

即
$ git commit -am "Add multiple posts"
[main 283cb79] Add multiple posts
 1 file changed, 63 insertions(+), 1 deletion(-)


86191@LAPTOP-14LS7DVE MINGW64 /c/Blog/source (main)
$ git push origin main
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 1.73 KiB | 1.73 MiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:qq2328837632/hello.git
   8b4ab8b..283cb79  main -> main

```

```
如果你想要将本地的 Markdown 文件上传到 GitHub 上的仓库，首先你需要在本地使用 git init 命令初始化一个 Git 存储库。然后，将 Markdown 文件添加到该存储库并提交更改。最后，使用 git push 命令将更改推送到 GitHub 上的仓库。

以下是一些常见的步骤：

在本地的 Markdown 文件所在的目录中打开终端或命令提示符。
使用命令 git init 初始化一个新的 Git 存储库。
使用命令 git add <file> 添加要上传的 Markdown 文件。例如，git add example.md。
使用命令 git commit -m "Initial commit" 提交更改，并添加相应的提交消息。
创建一个与 GitHub 上的仓库关联的远程仓库，可以使用 git remote add origin <repository-url>。这里的 <repository-url> 是你在 GitHub 上的仓库的 URL 地址。
最后，使用 git push -u origin master 命令将本地的更改推送到 GitHub 上的仓库。
接下来，你可以按照上述步骤继续进行操作，将本地的 Markdown 文件添加到该存储库并推送到 GitHub 上的仓库。

记得使用 git add <file> 命令将要上传的 Markdown 文件添加到存储库中，并使用 git commit -m "提交消息" 命令提交更改。

然后，创建一个与 GitHub 上的仓库关联的远程仓库，可以使用 git remote add origin <repository-url> 命令。这里的 <repository-url> 是你在 GitHub 上的仓库的 URL 地址。

最后，执行 git push -u origin master 命令将本地的更改推送到 GitHub 上的仓库。
如果你的 .git 文件夹和要添加的文件夹处于同一目录下，你可以使用 git add 命令的相对路径来将整个文件夹添加到 Git 存储库中。

请按照以下步骤操作：

打开终端或命令提示符，并确保你当前位于正确的 Git 存储库目录中。
运行命令 git add <folder>，将 <folder> 替换为要添加的文件夹的相对路径。例如，如果你的文件夹名为 myfolder，它与 .git 文件夹在同一目录下，你可以使用命令 git add myfolder。
Git 将会将整个文件夹添加到存储库中。
请确保在执行该命令之前，你已经处于正确的存储库目录中，并且要添加的文件夹和 .git 文件夹在同一级目录下。
要查看是否成功将文件夹添加到 Git 存储库中，你可以使用 git status 命令来查看当前存储库的状态。

请按照以下步骤操作：

打开终端或命令提示符，并确保你当前位于正确的 Git 存储库目录中。
运行命令 git status。
Git 将显示当前存储库的状态，包括已添加的文件、未跟踪的文件和提交等信息。
如果文件夹成功添加到存储库中，你应该能在输出结果中看到相应的文件夹名称。
如果文件夹在输出结果中显示为绿色，则表示已成功添加到存储库中。如果文件夹显示为红色，则表示尚未添加或有其他更改。如果文件夹未显示在输出结果中，则表示可能路径有误或者文件夹为空。

通过这种方式，你可以快速检查是否成功将文件夹添加到 Git 存储库中。
```

