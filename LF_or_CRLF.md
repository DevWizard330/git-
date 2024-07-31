今日在使用git时出现警告# warning: LF will be replaced by CRLF
好奇心驱使下，我在官方文档中找到了原因:

LF即LineFeed，换行符；CRLF即CarriageReturnLineFeed，回车符+换行符。

Windows下许多编辑器在换行时默认使用的是回车符+换行符(CRLF)，但在Mac或Linux中使用的只有换行符(LF)。因此，不同操作系统的开发者使用git时就会面临一些讨厌的格式问题。为了解决这个问题，git提供了自动换行符转换功能，输入git config --global core.autocrlf true命令以开启这个功能。这样，git会在将文件添加到暂存区时自动将CRLF转换为LF；在签出代码到文件系统时，git会将LF转换会CRLF。

以下内容来自[Git - Git Configuration (git-scm.com)](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
### Formatting and Whitespace

Formatting and whitespace issues are some of the more frustrating and subtle problems that many developers encounter when collaborating, especially cross-platform. It’s very easy for patches or other collaborated work to introduce subtle whitespace changes because editors silently introduce them, and if your files ever touch a Windows system, their line endings might be replaced. Git has a few configuration options to help with these issues.

#### `core.autocrlf`

If you’re programming on Windows and working with people who are not (or vice-versa), you’ll probably run into line-ending issues at some point. This is because Windows uses both a carriage-return character and a linefeed character for newlines in its files, whereas macOS and Linux systems use only the linefeed character. This is a subtle but incredibly annoying fact of cross-platform work; many editors on Windows silently replace existing LF-style line endings with CRLF, or insert both line-ending characters when the user hits the enter key.

Git can handle this by auto-converting CRLF line endings into LF when you add a file to the index, and vice versa when it checks out code onto your filesystem. You can turn on this functionality with the setting. If you’re on a Windows machine, set it to  — this converts LF endings into CRLF when you check out code:`core.autocrlf``true`

```console
$ git config --global core.autocrlf true
```

If you’re on a Linux or macOS system that uses LF line endings, then you don’t want Git to automatically convert them when you check out files; however, if a file with CRLF endings accidentally gets introduced, then you may want Git to fix it. You can tell Git to convert CRLF to LF on commit but not the other way around by setting to :`core.autocrlf``input`

```console
$ git config --global core.autocrlf input
```

This setup should leave you with CRLF endings in Windows checkouts, but LF endings on macOS and Linux systems and in the repository.

If you’re a Windows programmer doing a Windows-only project, then you can turn off this functionality, recording the carriage returns in the repository by setting the config value to :`false`

```console
$ git config --global core.autocrlf false
```
