
## Основные способы обработки

### 1. Использование `-exec`
```bash
# Базовый синтаксис
find /путь -name "*.txt" -exec команда {} \;

# Пример: поиск и вывод содержимого
find ~/документы -name "*.log" -exec cat {} \;
```

### 2. Эффективная обработка с `+`
```bash
find /путь -name "*.csv" -exec grep "шаблон" {} +
```

### 3. Использование `xargs` (безопасная версия)
```bash
find /путь -name "*.md" -print0 | xargs -0 -I{} команда "{}"
```

### 4. Цикл `while` для сложной обработки
```bash
find /путь -type f -print0 | while IFS= read -r -d '' файл; do
    echo "Обработка: $файл"
    # Действия с файлом
    mv "$файл" "${файл}.backup"
done
```

## Практические примеры

### Переименование файлов
```bash
find . -name "*.jpeg" -exec bash -c 'mv "$1" "${1%.jpeg}.jpg"' _ {} \;
```

### Удаление старых файлов
```bash
# Удалить файлы старше 30 дней
find /tmp -name "*.temp" -mtime +30 -delete
```

### Поиск и архивация
```bash
find /var/log -name "*.log" -size +10M -exec gzip {} \;
```

### Статистика по CSV
```bash
find /data -name "*.csv" -print0 | while IFS= read -r -d '' file; do
    echo "Анализ: $file"
    awk -F, '{sum+=$3} END {print "Сумма:", sum}' "$file"
done
```

## Важные примечания

1. Всегда проверяйте команды с `-print` перед выполнением
2. Для файлов с пробелами используйте `-print0` + `-0` или `-exec`
3. Для сложных операций используйте bash -c:

```bash
find . -name "*.txt" -exec bash -c '
  for file; do
    echo "Обработка: $file"
    cp "$file" "${file}.bak"
  done
' _ {} +
```

> **Совет**: В Obsidian используйте режим "Исходный код" (Ctrl+E) для правильного отображения блоков кода.