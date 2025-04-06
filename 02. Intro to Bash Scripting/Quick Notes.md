---
title: Quick Notes
updated: 2025-04-06 17:46:28Z
created: 2025-04-06 17:36:42Z
---

**In Bash we can always pass up to 9 arguments** (`$0`\-`$9`) to the script without assigning them to variables or setting the corresponding requirements for these. `9 arguments` because the first argument `$0` is reserved for the script.

**We can store a built in variable** within a customized one like this: `myvar=$(ls)`  
Then we do echo $myvar and it will execute ls command

### **How to Access Arrays**

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

&nbsp;