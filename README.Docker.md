# 🐳 Dockerfile для ViteForge (Vite + React + TypeScript + Tailwind CSS + shadcn/ui)

Этот Dockerfile предназначен для контейнеризации фронтенд-проекта, построенного на базе Vite, React, TypeScript,
Tailwind CSS и shadcn/ui.

---

## 🚀 Как это работает

Контейнер состоит из двух стадий:

1. **Стадия сборки (build)** — используется лёгкий образ `node:20-alpine` для установки зависимостей и сборки проекта с
   помощью Vite.
2. **Стадия запуска (production)** — используется `nginx:alpine` для обслуживания собранной статической версии
   приложения.

---

## 📂 Структура Dockerfile

```Dockerfile
# Стадия 1: билдим Node-приложение
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

# Стадия 2: используем nginx для отдачи статики
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
