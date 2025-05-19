```
#! /bin/bash

  

read -p "Enter dirictory wich you want using: " directory

  

find "$directory" -type f -name "*.txt" -exec bash -c '

for file; do

new="${file%.txt}_$(date +%Y-%m-%d_%H-%M).txt"

mv "$file" "$new"

done

' bash {} +
```