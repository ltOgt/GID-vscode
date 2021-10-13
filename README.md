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

## Small note on the regex
For you and my future self to better understand the regular expressions, here is a small explanation of `base.gidsym`:

`(?<=[\\s|\\|])(>|\\.|\\(|\\)|<|\\^|°|\\?|:|:=|\\^=|~|/|§|¬|!=|i|0|=|x|->|=>|<-|[0-9]+\\)|--|\\+|-|//|\\$|>>|::|\\|-|E|e|00|\\*|#|\\{|\\}|d|v|@|\\*\\*|_|\\[ \\]|\\[x\\]|\\[\\*\\]|\\[\\?\\]|\\[p\\]|\\[~\\])(?=[\\s|\\|])`

- escaped characters
    - `\\s` spaces + tabs (vs the regular character `s`)
    - `\\|` pipe (vs `|` which has the special meaning `OR`)
    - `\\.` dot (vs `.` which has the special meaning `ANY`)
    - `\\(` brace (vs `(` which has the special meaning `START MATCH GROUP`) (same for `)`)
    - `\\[` bracket (vs `[` which has the special meaning `START SYMBOL GROUP`) (same for `]`)
    - `\\^` hat (vs `^` which has the special meaning `START` (or `NOT` inside `[...]`))
    - `\\?` hat (vs `?` which has special meanings e.g. in look-arounds)
    - `\\+` plus (vs `+` which has the special meaning `AT LEAST ONE`)
    - `\\*` star (vs `*` which has the special meaning `AT LEAST ZERO`)
    - `\\$` dollar (vs `$` which has the special meaning `END`)
    - `\\{` curly (vs `{` which can be used to set counts and ranges, e.g. `[a]{1-10}` meaning between 1 and 10 `a`s) (same for `}`)
- positive look-ahead
    - `(?<=[\\s|\\|])` makes sure that the next match group `(>|...)` is preceded by space (`\\s`) or a pipe (`\\|`) without those symbols being a part of the match
    - this means " S problem" and " S|? problem" will match the symbols, but "Some problem?" will not.
    - also note how `<` and `=` must not be escaped, they become special characters only in this context
- positive look-behind
    - `(?=[\\s|\\|])` same as positive look-ahead but behind the previous match group