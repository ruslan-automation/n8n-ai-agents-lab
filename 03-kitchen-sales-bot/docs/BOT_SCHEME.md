# 🏗 Архитектура Telegram-бота: "Кухни на заказ"

Эта схема описывает логику работы лидогенерирующего бота на платформе n8n.
**Используйте этот файл как основу для вашего PDF-гайда (Лид-магнита).**

---

## 🧩 Визуальная Схема (Mermaid)

```mermaid
graph TD
    %% Стилизация узлов
    classDef trigger fill:#ff9900,stroke:#333,stroke-width:2px,color:white;
    classDef logic fill:#00ccff,stroke:#333,stroke-width:2px,color:white;
    classDef telegram fill:#0088cc,stroke:#333,stroke-width:2px,color:white;
    classDef google fill:#0F9D58,stroke:#333,stroke-width:2px,color:white;
    classDef media fill:#9b59b6,stroke:#333,stroke-width:2px,color:white;

    %% Узлы
    Start((Telegram Trigger)):::trigger
    Merge[Merge Data]:::logic
    Router{Router / Switch}:::logic
    
    %% Ветка Приветствия
    Greeting[Greeting + Photo]:::media
    StartQuiz[Кнопка 'Начать']:::telegram
    
    %% Ветка Опроса (Квиз)
    Q1[Вопрос 1: Форма]:::telegram
    Q2[Вопрос 2: Метраж]:::telegram
    Q3[Вопрос 3: Бюджет]:::telegram
    Q4[Вопрос 4: Сроки]:::telegram
    
    %% Обработка и Сохранение
    Parse[Code: Parsing Data]:::logic
    Sheets[Google Sheets]:::google
    Admin[Admin Alert]:::telegram
    
    %% Прогрев (Финал)
    Confirm[Confirmation Text]:::telegram
    Wait1(Wait 4s):::logic
    Video[Video Note 'Кружок']:::media
    Wait2(Wait 8s):::logic
    Album[Photo Album 'Примеры']:::media

    %% Связи
    Start --> Merge --> Router
    
    %% Логика Роутера
    Router -- "/start" --> Greeting
    Greeting --> StartQuiz --> Q1
    
    Router -- "Клик: Форма" --> Q2
    Router -- "Клик: Метраж" --> Q3
    Router -- "Клик: Бюджет" --> Q4
    Router -- "Клик: Сроки" --> Parse
    
    %% Навигация "Назад"
    Q2 -- "Назад" --> Q1
    Q3 -- "Назад" --> Q2
    Q4 -- "Назад" --> Q3
    
    %% Финализация
    Parse --> Sheets
    Parse --> Admin
    Sheets --> Confirm
    Confirm --> Wait1
    Wait1 --> Video
    Video --> Wait2
    Wait2 --> Album

    %% Подписи
    subgraph "Frontend (Telegram)"
        Greeting
        Q1
        Q2
        Q3
        Q4
        Confirm
        Video
        Album
    end

    subgraph "Backend (n8n Logic)"
        Merge
        Router
        Parse
        Wait1
        Wait2
    end
    
    subgraph "Database & CRM"
        Sheets
        Admin
    end
```

---

## 📝 Пояснение логики (для видео)

1.  **Stateless Режим:**
    *   Мы не храним состояние каждого пользователя в базе данных.
    *   Вся история выбора "зашивается" в кнопку следующего шага (Chain Data).
    *   *Пример:* Когда пользователь жмет "Бюджет: 100к", кнопка содержит данные: `Форма=Угловая | Размер=3м | Бюджет=100к`.
2.  **Бесшовность (EditMessage):**
    *   Мы не шлем 10 сообщений подряд. Мы редактируем одно и то же сообщение, меняя текст и кнопки. Это создает ощущение **приложения**, а не чата.
3.  **Humanize (Очеловечивание):**
    *   Мы используем `Wait` (паузы) и `Video Note` (круглые видео), чтобы бот не выглядел бездушной машиной.
    *   Конверсия в ответ у такого бота **в 2.5 раза выше**, чем у обычной формы.
