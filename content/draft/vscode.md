+++
title = "VS Code 功夫"
date = 2010-01-01T13:08:47+08:00
readingTime = true
categories = ["工具"]
tags = ["功夫系列", "vs code"]
toc = true
draft = true
+++

很多配置，此文有点长 ☕️

<!--more-->

---

之前用 Sublime，挺喜欢，不过现在转至 VS Code，虽然没有 Sublime 快，但足已，而且插件更丰富，颜值超靓，更快更新，毕竟是一个人和一个世界软件顶尖公司开发团队的差别。

## 外观

> `主题`，`字体`，`颜色` 是构成颜值的关键三要素。

### 字体

```json
"editor.fontFamily": "'Jetbrains Mono', 'Hack Nerd Font', 'Fira Code'",
    "editor.fontLigatures": true,
"editor.fontSize": 16,
```

### 主题

👔 [Material Theme](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme)

### 颜色

```json
"materialTheme.accent": "Bright Teal",
"materialTheme.accentPrevious": "Bright Teal",
"materialTheme.fixIconsRunning": false,
```

### 图标

🔣 [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

```json
    "workbench.iconTheme": "eq-material-theme-icons-ocean",
```

## Preference

```json
{
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "[markdown]": {
        "files.trimTrailingWhitespace": false,
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[python]": {
        "editor.insertSpaces": true,
        "editor.tabSize": 4
    },
    "[yaml]": {
        "editor.insertSpaces": true
    },
    "beautify.language": {
        "html": ["html", "php", "erb"],
        "css": [],
        "js": []
    },
    "breadcrumbs.enabled": true,
    "cSpell.userWords": ["ansible"],
    "editor.acceptSuggestionOnEnter": "smart",
    "editor.cursorBlinking": "smooth",
    "editor.cursorStyle": "block",
    "editor.cursorSurroundingLines": 15,
    "editor.cursorSmoothCaretAnimation": true,
    "editor.fontFamily": "'Fira Code iScript', 'Hack Nerd Font', Consolas, 'Courier Prime'",
    "editor.fontLigatures": true,
    "editor.fontSize": 16,
    "editor.formatOnPaste": true,
    "editor.formatOnSave": true,
    "editor.lineHeight": 24,
    "editor.minimap.enabled": false,
    "editor.mouseWheelZoom": true,
    "editor.renderIndentGuides": false,
    "editor.renderWhitespace": "boundary",
    "editor.suggestSelection": "first",
    "editor.tabSize": 4,
    "editor.wordWrap": "on",
    "editor.wordWrapColumn": 120,
    "eslint.autoFixOnSave": false,
    "emmet.includeLanguages": {
        "ejs": "html",
        "erb": "erb",
        "html": "html",
        "javascript": "javascriptreact",
        "vue": "html"
    },
    "emmet.syntaxProfiles": {
        "javascript": "jsx",
        "xml": {
            "attr_quotes": "single"
        }
    },
    "eslint.validate": ["javascript"],
    "explorer.confirmDelete": false,
    "explorer.sortOrder": "type",
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 5000,
    "files.eol": "\n",
    "files.exclude": {
        "**/.classpath": true,
        "**/.project": true,
        "**/.settings": true,
        "**/.factorypath": true
    },
    "files.trimFinalNewlines": true,
    "files.trimTrailingWhitespace": true,
    "git.enableSmartCommit": true,
    "gitlens.advanced.messages": {
        "suppressCommitHasNoPreviousCommitWarning": false,
        "suppressCommitNotFoundWarning": false,
        "suppressFileNotUnderSourceControlWarning": false,
        "suppressGitVersionWarning": false,
        "suppressLineUncommittedWarning": false,
        "suppressNoRepositoryWarning": false,
        "suppressResultsExplorerNotice": false,
        "suppressShowKeyBindingsNotice": true
    },
    "gitlens.historyExplorer.enabled": true,
    "gitlens.keymap": "none",
    "gitlens.views.fileHistory.enabled": true,
    "gitlens.views.lineHistory.enabled": true,
    "html.format.wrapLineLength": 0,
    "javascript.validate.enable": false,
    "jshint.options": {
        "esversion": 8
    },
    "kite.showWelcomeNotificationOnStartup": false,
    "materialTheme.accent": "Bright Teal",
    "materialTheme.accentPrevious": "Bright Teal",
    "materialTheme.fixIconsRunning": false,
    "peacock.favoriteColors": [
        {
            "name": "Angular Red",
            "value": "#b52e31"
        },
        {
            "name": "Auth0 Orange",
            "value": "#eb5424"
        },
        {
            "name": "Azure Blue",
            "value": "#007fff"
        },
        {
            "name": "C# Purple",
            "value": "#68217A"
        },
        {
            "name": "Gatsby Purple",
            "value": "#639"
        },
        {
            "name": "Go Cyan",
            "value": "#5dc9e2"
        },
        {
            "name": "Java Blue-Gray",
            "value": "#557c9b"
        },
        {
            "name": "JavaScript orange",
            "value": "#f9e64f"
        },
        {
            "name": "Mandalorian Blue",
            "value": "#1857a4"
        },
        {
            "name": "Node Green",
            "value": "#215732"
        },
        {
            "name": "React Blue",
            "value": "#00b3e6"
        },
        {
            "name": "Something Different",
            "value": "#832561"
        },
        {
            "name": "Vue Green",
            "value": "#42b883"
        }
    ],
    "prettier.disableLanguages": [
        "html",
        "javascript",
        "javascriptreact",
        "json"
    ],
    "prettier.tabWidth": 4,
    "standard.autoFixOnSave": true,
    "standard.validate": ["javascript", "javascriptreact"],
    "stylelint.enable": true,
    "sync.anonymousGist": false,
    "sync.askGistName": false,
    "sync.autoDownload": false,
    "sync.autoUpload": false,
    "sync.forceDownload": false,
    "sync.forceUpload": true,
    "sync.gist": "c382d614120b1c4b60cbe24437024244",
    "sync.host": "",
    "sync.lastDownload": "",
    "sync.lastUpload": "2018-07-15T07:51:34.933Z",
    "sync.pathPrefix": "",
    "sync.quietSync": false,
    "sync.removeExtensions": true,
    "sync.showSummary": true,
    "sync.syncExtensions": true,
    "sync.version": 251,
    "telemetry.enableTelemetry": false,
    "terraform.telemetry.enabled": false,
    "telemetry.enableCrashReporter": false,
    "terminal.external.osxExec": "iTerm.app",
    "terminal.integrated.copyOnSelection": true,
    "terminal.integrated.fontFamily": "'Hack Nerd Font'",
    "terminal.integrated.fontSize": 14,
    "terminal.integrated.shell.osx": "zsh",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "window.menuBarVisibility": "default",
    "window.titleBarStyle": "native",
    "window.zoomLevel": 0,
    "workbench.colorCustomizations": {
        "activityBarBadge.background": "#64FFDA",
        "breadcrumb.activeSelectionForeground": "#64FFDA",
        "editorSuggestWidget.highlightForeground": "#64FFDA",
        "editorWidget.border": "#64FFDA",
        "editorWidget.resizeBorder": "#64FFDA",
        "list.activeSelectionForeground": "#64FFDA",
        "list.highlightForeground": "#64FFDA",
        "list.inactiveSelectionForeground": "#64FFDA",
        "menu.selectionForeground": "#64FFDA",
        "menubar.selectionForeground": "#64FFDA",
        "notificationLink.foreground": "#64FFDA",
        "panelTitle.activeBorder": "#64FFDA",
        "pickerGroup.foreground": "#64FFDA",
        "progressBar.background": "#64FFDA",
        "scrollbarSlider.activeBackground": "#64FFDA50",
        "settings.headerForeground": "#64FFDA",
        "settings.modifiedItemForeground": "#64FFDA",
        "settings.modifiedItemIndicator": "#64FFDA",
        "tab.activeBorder": "#64FFDA",
        "textLink.foreground": "#64FFDA"
    },
    "workbench.colorTheme": "Material Theme Palenight High Contrast",
    "workbench.editor.highlightModifiedTabs": true,
    "workbench.fontAliasing": "antialiased",
    "workbench.iconTheme": "material-icon-theme",
    "todo-tree.highlights.enabled": false,
    "todo-tree.tree.showScanModeButton": false,
    "workbench.statusBar.visible": false,
    "workbench.activityBar.visible": true
}
```

## 插件

插件超多的，所以看着办，这里介绍几个超级好用/必装的。

-   Settings Sync
    它是一款使用 GitHub 中的 Gists 作为配置文件存储来实现同步的。它可以让我们在无数个设备上同步 VS Code 的配置。

    -   配置 Settings Sync：
    -   上传本地配置：
    -   下载配置：

## 常用快捷键

![快捷键](/images/vscode/keys.png)

CTRL + , = 打开用户设置

## 操作技巧

### 多光标

-   在多个没有明显特征的地方添加相同的内容，此时如果配合鼠标操作，只需要按住 ⌥ 键，在需要添加光标的位置单击即可：
    ![多光标](/images/vscode/cursor1.gif)

-   有时统一需要在多行的行末添加一些内容，或者需从行末开始删除一些内容。这时只需选中要修改的内容，按 ⇧⌥i 就可以在行末添加光标进行操作：
    ![多光标](/images/vscode/cursor2.gif)

-   需要把文档中一些相同的内容统一选中进行某些修改，这时可以先选中一个内容，然后按下 ⌘d 就会自动选中下一个相同的内容：
    ![多光标](/images/vscode/cursor3.gif)

## 开发集成

### Web

🗓TBD

### Python

🗓TBD

### Go

🗓TBD

### Java

> -   [Language Support for Java, by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java)
> -   [Debugger for Java, by Microsoft](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)
> -   [Java Dependency Viewer, by Microsoft](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-dependency)
> -   [Maven for Java, by Microsoft](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)
> -   [Java Test Runner, by Microsoft](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test)

Spring 支持

> -   [Spring Boot Tools](https://marketplace.visualstudio.com/items?itemName=Pivotal.vscode-spring-boot)
> -   [Spring Boot Extension Pack](https://marketplace.visualstudio.com/items?itemName=Pivotal.vscode-boot-dev-pack)
> -   [Spring Boot Snippets](https://marketplace.visualstudio.com/items?itemName=developersoapbox.vscode-springboot-snippets)
