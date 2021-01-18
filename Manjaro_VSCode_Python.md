# VSCode Python 环境配置

## 1. terminal无法输入，python无法run

该问题是由于VSCode的terminal设置无法与系统匹配造成，尤其是在安装conda管理环境之后。

解决方案

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

- 重启VSCode