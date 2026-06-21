---
tags:
  - ripgrep
  - excalidraw
  - tricks
draft: "false"
---
"which files, anywhere under here, mention this?"
```sh
grep -rl <regex>
```
-r recursive 
-l list filenames only that regex pattern 

```
rg <regex> 
```

![[Pasted image 20260621181300.png]]

ripgrep is faster and nicer (cause of rust, parallel processing and smart line buffering)
also skips node_modules .git by default 

## useful one liners 
```sh
# Files (not node_modules) containing TODO, case-insensitive
grep -ril --exclude-dir=node_modules "todo" .

# Lines matching, with 2 lines of context, recursive
grep -rn -C2 "function fetchUser" .

# Only .py and .ts files
grep -rl --include="*.py" --include="*.ts" "import requests" .

# Count matches per file
grep -rc "console.log" src/

# Whole word only (avoids matching "category" when searching "cat")
grep -rw "cat" .

# Files that do NOT contain a string
grep -rL "license" .
```