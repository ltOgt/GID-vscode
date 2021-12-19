# GID Symbols Grammar for Syntax Highlighting

Add syntax highlighting for https://github.com/ltOgt/GID to vscode.

Since this extension is not published, you have to clone it into `~/.vscode/extensions/`.


## Custom coloring

Add the following to your `settings.json` (e.g. at `~/.config/Code/User/` for Linux or `~/Library/Application Support/Code/User/settings.json` for mac):

```json
{
    //...
        "editor.tokenColorCustomizations": {
        "textMateRules": [
            {   
                "scope": "base.gidsym",
                "settings": {
                    "foreground": "#5FAF00"
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
            },  
            {   
                "scope": "comment.gidsym",
                "settings": {
                    "foreground": "#8787AF"
                }   
            },  
            {   
                "scope": "comment-block.gidsym",
                "settings": {
                    "foreground": "#8787AF"
                }   
            },  
            {   
                "scope": "content.comment-block.gidsym",
                "settings": {
                    "foreground": "#8787AF"
                }   
            },
            {
                "scope": "bold.style.gidsym",
                "settings": {
                    "foreground": "#FFFFFF",
                    "fontStyle": "bold"
                }
            },
            {
                "scope": "underline.style.gidsym",
                "settings": {
                    "foreground": "#FFFFFF",
                    "fontStyle": "underline"
                }
            },
            {
                "scope": "italic.style.gidsym",
                "settings": {
                    "foreground": "#FFFFFF",
                    "fontStyle": "italic"
                }
            }
        ]   
    }   

}
```

## Small note on the regex
For you and my future self to better understand the regular expressions, here is a small explanation of `base.gidsym`:

`(?<=^|[\\s|\\|])(>|\\.|\\(|\\)|<|\\^|°|\\?|:|:=|\\^=|~|/|§|¬|!=|i|0|=|x|->|=>|<-|[0-9]+\\)|--|\\+|-|//|\\$|>>|::|\\|-|E|e|00|\\*|#|\\{|\\}|d|v|@|\\*\\*|_|\\[ \\]|\\[x\\]|\\[\\*\\]|\\[\\?\\]|\\[p\\]|\\[~\\])(?=$|[\\s|\\|])`

- escaped characters
    - `\\s` spaces + tabs (vs the regular character `s`)
    - `\\|` pipe (vs `|` which has the special meaning `OR`)
    - `\\.` dot (vs `.` which has the special meaning `ANY`)
    - `\\(` brace (vs `(` which has the special meaning `START MATCH GROUP`) (same for `)`)
    - `\\[` bracket (vs `[` which has the special meaning `START SYMBOL GROUP`) (same for `]`)
    - `\\^` hat (vs `^` which has the special meaning `START OF LINE` (or `NOT` inside `[...]`))
    - `\\?` hat (vs `?` which has special meanings e.g. in look-arounds)
    - `\\+` plus (vs `+` which has the special meaning `AT LEAST ONE`)
    - `\\*` star (vs `*` which has the special meaning `AT LEAST ZERO`)
    - `\\$` dollar (vs `$` which has the special meaning `END OF LINE`)
    - `\\{` curly (vs `{` which can be used to set counts and ranges, e.g. `[a]{1,10}` meaning between 1 and 10 `a`s) (same for `}`)
- positive look-ahead
    - `(?<=^|[\\s|\\|])` makes sure that the next match group `(>|...)` is preceded (`(?<=...)`) by space (`\\s`), a pipe (`\\|`), or the beginning of the line (`^`) without those symbols being a part of the match
    - this means " S problem" and " S|? problem" will match the symbols, but "Some problem?" will not.
    - also note how `<` and `=` must not be escaped, they become special characters only in this context
    - `^` can't be inside the `[...]` since then it would be the `NOT` symbol (see [here](https://stackoverflow.com/a/9155707/7215915))
- positive look-behind
    - `(?=$|[\\s|\\|])` same as positive look-ahead but behind the previous match group

for text mate regex in general see https://macromates.com/manual/en/regular_expressions
