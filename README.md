# GID Symbols Grammar for Syntax Highlighting

Add syntax highlighting for https://github.com/ltOgt/GID to vscode.

Since this extension is not published, you have to clone it into `~/.vscode/extensions/`.


## Custom coloring

Add the following to your `settings.json` (e.g. at `~/.config/Code/User/`):

```json
{
    //...
    "editor.tokenColorCustomizations": {
        "textMateRules": [
            {
                "scope": "base.gidsym",
                "settings": {
                    "foreground": "#00FF00"
                }
            },
            {
                "scope": "notice.gidsym",
                "settings": {
                    "foreground": "#FF0000"
                }
            },
            {
                "scope": "reference.gidsym",
                "settings": {
                    "foreground": "#5454fc"
                }
            },
            {
                "scope": "line.gidsym",
                "settings": {
                    "foreground": "#FF0000"
                }
            }
        ]
    }
}
```