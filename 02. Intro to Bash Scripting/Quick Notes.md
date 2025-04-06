---
title: Quick Notes
updated: 2025-04-06 22:58:26Z
created: 2025-04-06 17:36:42Z
---

&nbsp;

**In Bash, a newline character (a line break)** is generally treated as a command terminator, similar to a semicolon. This means that if each command is on a separate line, you don't necessarily need to use semicolons. Semicolons are primarily used when you want to execute multiple commands on the same line. For example: <span style="color: #000000;">command1; command2; command3</span>

**In Bash we can always pass up to 9 arguments** (`$0`\-`$9`) to the script without assigning them to variables or setting the corresponding requirements for these. `9 arguments` because the first argument `$0` is reserved for the script.

**We can store a built in variable** within a customized one like this: `myvar=$(ls)`  
Then we do echo $myvar and it will execute ls command

# **How to Access Arrays**

```
#!/bin/bash
fruits=("Apple" "Banana" "Cherry")
echo "First fruit: ${fruits[0]}"
echo "All fruits: ${fruits[@]}"

#To determine the length of an array, use ${#my_array[@]}​
echo "Number of fruits: ${#fruits[@]}" # Outputs: 3
    
#Be cautious with spaces in array elements. Enclose elements with spaces in quotes:​
my_array=("value with spaces" "another value")
```

&nbsp;

# **COMPARISON OPERATORS**

### To Be Safe:

#### ✅ `[[ ... ]]` (Double brackets):

- Handles strings and comparisons more gracefully
    
- Supports `&&`, `||`, `<`, `>`, and pattern matching
    
- Doesn't break if variables are empty or unset
    

#### ✅ Quoting `"$var"`:

- Prevents word splitting (e.g., from spaces in input)
    
- Avoids syntax errors when variables are unset
    
- Prevents wildcards (`*`) from expanding unexpectedly  
    <br/>
    

### 🔐 Safe Pattern (ALWAYS works):

```bash
if [[ "$var" == "hello" ]]; then
  echo "Hello there!"
fi
```

### 🧨 Risky Pattern (AVOID):

```bash
if [ $var == hello ]; then   # Breaks if $var is empty or has spaces
  echo "Maybe works, maybe not"
fi
```

&nbsp;

### 🔍 Key Notes:

- **Double brackets `[[ ... ]]` are more powerful**: support for multiple conditions (`&&`, `||`), string comparison with `< >`, and pattern matching.
    
- **Single brackets `[ ... ]`** are more limited: require quoting and can only handle one condition at a time — for multiple, you have to chain them:
    

```bash
if [ "$a" -gt 5 ] && [ "$a" -lt 10 ]; then
  echo "a is between 5 and 10"
fi
```

- **Arithmetic expressions `(( ... ))`** are best for math, and you can do:

```bash
if (( a > 5 && a < 10 )); then
  echo "a is between 5 and 10"
fi
```

&nbsp;

#### ✅ Correct ways to do arithmetic in Bash:

1.  **Double parentheses `(( ... ))`** – best for math!

```bash
a=5
b=3
if (( a > b )); then
  echo "a is greater than b"
fi
```

2.  **`let` command** – older way (still works):

```bash
let "sum = a + b"
echo $sum
```

3.  **`expr` command** – really old-school, not recommended:

```bash
sum=$(expr $a + $b)
echo $sum
```

&nbsp;

# Examples

```bash
#!/bin/bash

# Check if the specified file exists and if we have read permissions
if [[ -e "$1" && -r "$1" ]]
then
    echo -e "We can read the file that has been specified."
    exit 0

elif [[ ! -e "$1" ]]
then
    echo -e "The specified file does not exist."
    exit 2

elif [[ -e "$1" && ! -r "$1" ]]
then
    echo -e "We don't have read permission for this file."
    exit 1

else
    echo -e "Error occured."
    exit 5
fi
```

```BASH
#!/bin/bash

if [[ "$1" == "admin" ]]; then
  echo "Welcome, admin!"
elif [[ -z "$1" ]]; then
  echo "No username provided."
else
  echo "Access denied for $1"
fi

```

```BASH
#!/bin/bash

if [ "$1" -lt 18 ]; then
  echo "You are a minor."
elif [ "$1" -ge 18 ]; then
  echo "You are an adult."
fi

```

```BASH
#!/bin/bash

if [ -f "$1" ]; then
  echo "It's a file."
elif [ -d "$1" ]; then
  echo "It's a directory."
else
  echo "File or directory does not exist."
fi

```

```BASH
#!/bin/bash

if [[ -f "$1" && -r "$1" ]]; then
  echo "Readable file exists."
elif [[ ! -e "$1" ]]; then
  echo "File does not exist."
elif [[ -e "$1" && ! -r "$1" ]]; then
  echo "File exists but is not readable."
fi

```

&nbsp;

&nbsp;