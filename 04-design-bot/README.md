# 🎨 AI Design Assistant (Telegram Bot)

**Гибридный ИИ-ассистент для генерации и редактирования изображений.**

Этот Telegram-бот способен понимать запросы на генерацию дизайна и использовать продвинутые нейросети (через интерфейс n8n) для создания уникального графического контента. Идеально подходит для контент-менеджеров, SMM и дизайнеров интерьеров.

## 📊 Основные возможности
* Генерация изображений по запросам через локальные LLM с использованием API Groq и OpenRouter.
* Подключение внешней панели управления (Dashboard API).
* Работа с продвинутой моделью Mira.

---

## ⚙️ Структура проекта

Как и в лучших корпоративных решениях, код разделен на логические блоки:
* 📁 `workflows/` — файлы логики для n8n (`AI Design Assistant.json` и `Dashboard API.json`).
* 📁 `docs/` — инструкции, HTML-гайды по Mira и посты для Telegram.
* 📁 `assets/` — демонстрация работы системы.

---

## ⚡ Примеры работы (Скриншоты системы)

Взаимодействие бота в Telegram и финальный результат работы AI-моделей:
![Bot Setup](assets/1_botfather..jpg)
![Final Generation Test](assets/5_final_test.jpg)

Граф узлов n8n (как данные передаются внутри):
![AI Node Architecture](assets/Ai_node.jpg)
![Zarub Logic](assets/Zarub_1.jpg)

Архитектурная настройка локальных LLM (Groq & OpenRouter):
![Groq](assets/Gorq-1.jpg)
![OpenRouter](assets/OpenRouter-1.jpg)
![OpenRouter Model](assets/OpenRouter-3.jpg)

---

## 🛠 Технологический стек
* **Оркестрация**: n8n (AI Agent Node)
* **LLM / Vision**: Groq API, OpenRouter API
* **Дополнительно**: Mira
