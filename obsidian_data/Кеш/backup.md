```
#!/bin/bash

  

# Запрашиваем пути у пользователя

read -p "Enter backup directory: " BACKUP_DIR

read -p "Enter source directories (separated by space): " -a SOURCE_DIRS

  

# Проверка папок

for dir in "${SOURCE_DIRS[@]}"; do

if [ ! -d "$dir" ]; then

echo "❌ Ошибка: Папка $dir не существует!"

exit 1

fi

done

  

# Создаём папку для бэкапов

mkdir -p "$BACKUP_DIR"

  

# Архивируем (с выводом лога)

echo "Создаём архив..."

archive_name="$BACKUP_DIR/backup_$(date +%Y-%m-%d_%H-%M).tar.gz"

tar -czvf "$archive_name" "${SOURCE_DIRS[@]}"

  

# Проверка

if [ $? -eq 0 ]; then

echo "✅ Бэкап успешно создан: $archive_name"

echo "Размер: $(du -h "$archive_name" | cut -f1)"

else

echo "❌ Ошибка при создании бэкапа!"

exit 1

fi
```