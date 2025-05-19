```
#!/bin/bash

  
  

fag=$(df -h / | awk 'NR==2 {print $5}' | tr -d '%')

if [ "$fag" -gt 20 ]; then

echo "Warning: Disk space is low!"

echo $fag

else

echo "System OK"

echo $fag

fi
```