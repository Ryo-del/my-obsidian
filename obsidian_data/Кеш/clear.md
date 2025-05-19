```
#!/bin/bash

  

cachefile="/home/ryo/test"

read -p "Введите путь к мусору" cachefile

  
  

find "$cachefile" -type f \( -mtime +3 -o -size +100c \) -delete

echo "файлы удалены"
```