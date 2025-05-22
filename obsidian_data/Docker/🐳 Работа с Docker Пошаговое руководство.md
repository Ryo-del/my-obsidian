

⸻

## 1. 📦 Установка Docker

Перед началом работы установите Docker:

- [Скачать Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Проверка установки:
```bash
docker --version
```

⸻

2. 📁 Создание Dockerfile

Если вы пишете собственное приложение, создайте файл Dockerfile:
```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

Если образ уже есть на Docker Hub, (https://hub.docker.com/) этот шаг можно пропустить.

⸻

3. 🏗️ Сборка Docker-образа
```bash
docker build -t my-node-app .
```
 
 • -t — имя образа.
 • . — текущая директория с Dockerfile.

⸻

4. 🚀 Запуск контейнера
```bash
docker run -d -p 3000:3000 --name node-container my-node-app
```

 • -d — в фоновом режиме.
 • -p — проброс портов.
 • --name — имя контейнера.

⸻

5. 🔎 Проверка состояния контейнера
```bash
docker ps
docker logs node-container
```

⸻

6. 🛠️ Работа с томами

Для сохранения данных вне контейнера:
```bash
docker run -v /my/local/folder:/app/data my-node-app
```
 • Локальная папка будет отображаться внутри контейнера по пути /app/data.

⸻

7. 🧼 Остановка и удаление
```bash
docker stop node-container
docker rm node-container
docker rmi my-node-app
docker system prune
```
 • Удаление ненужных данных, контейнеров, образов.

⸻

8. 🐙 Docker Compose (при необходимости)

Пример docker-compose.yml:
```yml
version: "3.9"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```
Запуск:
```bash
docker-compose up --build
```


⸻

🔗 Полезные ссылки
 • Docker Docs (https://docs.docker.com/)
 • Docker Hub (https://hub.docker.com/)
 • Docker Compose Docs (https://docs.docker.com/compose/)

⸻

🧠 Связанные понятия (для графа Obsidian)
 • [[Dockerfile]]
 • [[Образ (Docker image)]]
 • [[Контейнер Docker]]
 • [[Docker Volumes]]
 • [[Docker Compose]]
 • [[Сеть в Docker]]

---

Хочешь, могу сделать отдельные `.md`-файлы для каждой секции (например, как atomic notes).