---
title: "我在VS Code裡面用了哪些插件"
date: 2024-03-21T13:54:00+08:00
lastmod: 2024-03-25T09:03:00+08:00
draft: false
author: "Divazone King"
authorLink: ""
tags: ["vscode"]
categories: ["vscode"]
---

簡單紀錄一下我在VS Code裡面裝了哪些插件

## Cloud維運
* [Ansible](https://marketplace.visualstudio.com/items?itemName=redhat.ansible)
* [Terraform](https://marketplace.visualstudio.com/items?itemName=4ops.terraform)
* [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
* [Kubernetes](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools)
* [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) - 可使用VS Code遠端連線開發
* [DotENV](https://marketplace.visualstudio.com/items?itemName=mikestead.dotenv) - .env syntax highlighting


## 文字編輯相關
* [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) - YAML 檔的 syntax highlight
* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) - 檢測拼字錯誤
* [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) - TOML 檔的 syntax highlight
* [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow) - 縮排美化


## Golang
* [Go](https://marketplace.visualstudio.com/items?itemName=golang.Go)

## Python
* [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
* [Python Debugger](https://marketplace.visualstudio.com/items?itemName=ms-python.debugpy)
* [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) - 微軟的Python Language Server
* [Flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8) - Python Linter，符合PEP 8規範
* [Black Formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) - Python Formatter，符合PEP 8規範
* [isort](https://marketplace.visualstudio.com/items?itemName=ms-python.isort) - import排序，符合PEP 8規範
* [autoDocstring - Python Docstring Generator](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring) - 自動生產docstring，符合PEP 257規範


## git相關
* [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) - 可視化的git log
* [GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) - 強大的git extension in VS Code
* [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) - 方便查看git分支圖形

## Hugo工作流程(Markdown編輯)
* [Hugo Language and Syntax Support](https://marketplace.visualstudio.com/items?itemName=budparr.language-hugo-vscode)
* [Hugo Shortcode Syntax Highlighting](https://marketplace.visualstudio.com/items?itemName=kaellarkin.hugo-shortcode-syntax)
* [Hugo Snippets](https://marketplace.visualstudio.com/items?itemName=fivethree.vscode-hugo-snippets)
* [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
* [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid)
* [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) - 做 Marp Markdown 簡報的套件
* [Mermaid Markdown Syntax Highlighting](https://marketplace.visualstudio.com/items?itemName=bpruitt-goddard.mermaid-markdown-syntax-highlighting)
* [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) - 選擇圖檔路徑時節省時間

## 佈景主題
* [Chinese (Traditional) Language Pack for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hant)
* [One Dark Pro](https://marketplace.visualstudio.com/items?itemName=zhuangtongfa.Material-theme) - VS Code主題
* [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) - VS Code icon主題

## settings.json設定
```json
{
    "workbench.iconTheme": "material-icon-theme",
    "workbench.colorTheme": "One Dark Pro Darker",

    "[python]": {
        "editor.defaultFormatter": "ms-python.black-formatter",
		"editor.codeActionsOnSave": {
            "source.organizeImports": "explicit"
        }
    },
	"flake8.args": [
        "--max-line-length=100",
        "--ignore=E131,E302"
    ],
    "black-formatter.args": [
        "--line-length=100",
        "--skip-string-normalization"
    ],
	"isort.args": [
        "--src=${workspaceFolder}",
        "--line-length=100"
	],
	"isort.check": true,
    "autoDocstring.docstringFormat": "google-notypes",
    "git.ignoreRebaseWarning": true,
    "files.eol": "\n"
}
```








