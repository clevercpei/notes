# VSCode Python 环境配置

## 1. terminal无法输入，python无法run

#### - 该问题是由于VSCode的terminal设置无法与系统匹配造成，尤其是在安装conda管理环境之后。

#### - 解决方案

- 依次点击 设置-Features-Terminal

- 选择Linux对应的Edit in settings.json，打开

- 将"terminal.integrated.inheritEnv"设置为true

- conda和shell路径根据实际情况设置

    `{`

    ​	`"editor.fontSize": 18,`

    ​	`"workbench.editorAssociations": [],`

    ​	`"terminal.integrated.inheritEnv": true,`

    ​	`"terminal.integrated.shell.linux": "/bin/fish",`

    ​	`"python.condaPath": "/opt/anaconda/bin/",`

    ​	`"terminal.external.linuxExec": "alacritty",`

    ​	`"terminal.integrated.automationShell.linux": "/bin/fish",`

    ​	`"python.envFile": "~/.conda/envs/"`

    `}`

- 保存重启VSCode

# 2.无法直接打开Jupyter notebook的.ipynb文件

#### - 该问题是Vscode编辑器和Jupyter notebook插件编辑设置不匹配造成，修改设置即可。

#### - 解决方案

- 点击Jupyter插件的插件设置，找到 Jupyter › Experiments: Opt Into设置项

- 点击Edit in settings.json

- 设置"jupyter.experiments.optInto": [] 项为 "CustomEditor"

- 在同一设置文件中设置"workbench.editorAssociations": [] 项为

  ​         {

  ​            "viewType": "jupyter.notebook.ipynb",

  ​            "filenamePattern": "*.ipynb"

  ​        }

- 完整设置如下

  `{
      "editor.fontSize": 18,
      "workbench.editorAssociations": [
          {
              "viewType": "jupyter.notebook.ipynb",
              "filenamePattern": "*.ipynb"
          }
      ],
      "terminal.integrated.inheritEnv": true,
      "terminal.integrated.shell.linux": "/bin/fish",
      "python.condaPath": "/opt/anaconda/bin/",
      "terminal.external.linuxExec": "alacritty",
      "terminal.integrated.automationShell.linux": "/bin/fish",
      "python.envFile": "~/.conda/envs/",
      "jupyter.alwaysTrustNotebooks": true,
      "jupyter.experiments.optInto": ["CustomEditor"]
  }`

- 保存重启VSCode