# 💬 Messenger Frontend

Современный мессенджер с поддержкой видеозвонков, демонстрации экрана и обмена файлами.

## 🚀 Технологии

- **React 18** с TypeScript
- **Vite** для быстрой разработки
- **Tailwind CSS** для стилизации
- **Redux Toolkit** для управления состоянием
- **Socket.IO** для real-time коммуникации
- **WebRTC** для видео/аудио звонков
- **React Hook Form** для работы с формами
- **Framer Motion** для анимаций

## 📋 Возможности

- ✅ Регистрация и авторизация
- ✅ Real-time чат с сообщениями
- ✅ Видео и аудио звонки
- ✅ Демонстрация экрана
- ✅ Групповые конференции
- ✅ Система друзей
- ✅ Загрузка файлов и изображений
- ✅ Эмодзи и реакции
- ✅ Уведомления
- ✅ Темная/светлая тема
- ✅ Адаптивный дизайн

## 🛠 Установка и запуск

### Предварительные требования

- Node.js >= 18.0.0
- npm >= 9.0.0

### 1. Клонирование репозитория

```bash
git clone https://github.com/your-username/messenger-frontend.git
cd messenger-frontend
```

### 2. Установка зависимостей

```bash
npm install
```

### 3. Настройка переменных окружения

Скопируйте `.env.example` в `.env` и настройте переменные:

```bash
cp .env.example .env
```

Основные переменные:
```env
VITE_API_URL=http://localhost:5000/api
VITE_SOCKET_URL=http://localhost:5000
VITE_STUN_SERVER=stun:stun.l.google.com:19302
```

### 4. Запуск в режиме разработки

```bash
npm run dev
```

Приложение будет доступно по адресу http://localhost:3000

## 🏗 Структура проекта

```
src/
├── components/          # React компоненты
│   ├── common/         # Общие компоненты (Button, Modal, etc.)
│   ├── auth/           # Компоненты авторизации
│   ├── chat/           # Компоненты чата
│   ├── friends/        # Компоненты системы друзей
│   ├── calls/          # Компоненты звонков
│   └── profile/        # Компоненты профиля
├── pages/              # Страницы приложения
├── hooks/              # Кастомные React хуки
├── services/           # API сервисы
├── store/              # Redux store и slices
├── utils/              # Утилиты
├── types/              # TypeScript типы
└── styles/             # CSS файлы
```

## 🔧 Доступные скрипты

```bash
# Запуск в режиме разработки
npm run dev

# Сборка для продакшена
npm run build

# Предварительный просмотр продакшен сборки
npm run preview

# Проверка типов TypeScript
npm run type-check

# Линтинг кода
npm run lint

# Форматирование кода
npm run format

# Анализ размера бундла
npm run analyze
```

## 📱 API Integration

Приложение интегрируется с backend API через следующие эндпоинты:

- `POST /api/auth/register` - Регистрация
- `POST /api/auth/login` - Авторизация  
- `GET /api/chats` - Получение чатов
- `POST /api/messages` - Отправка сообщений
- `POST /api/friends/add` - Добавление друзей
- `POST /api/calls/initiate` - Инициация звонка

## 🔌 WebSocket События

Приложение использует Socket.IO для real-time коммуникации:

- `message:send` - Отправка сообщения
- `message:receive` - Получение сообщения  
- `call:initiate` - Инициация звонка
- `call:accept` - Принятие звонка
- `user:online` - Статус пользователя онлайн

## 🎨 Стилизация

Проект использует Tailwind CSS с кастомной конфигурацией:

- Кастомные цвета для мессенджера
- Анимации для уведомлений и переходов
- Адаптивный дизайн для всех устройств
- Поддержка темной и светлой темы

## 🧪 WebRTC конфигурация

Для видеозвонков используются STUN/TURN серверы:

```javascript
const rtcConfig = {
  iceServers: [
    { urls: 'stun:stun.l.google.com:19302' },
    { 
      urls: 'turn:your-turn-server.com:3478',
      username: 'username',
      credential: 'password'
    }
  ]
};
```

## 🔐 Безопасность

- JWT токены для авторизации
- CSRF защита
- Валидация данных на клиенте
- Безопасная загрузка файлов
- Content Security Policy

## 📊 Производительность

- Код-сплиттинг по маршрутам
- Ленивая загрузка компонентов
- Оптимизация изображений
- Service Worker для кеширования
- Bundle analysis

## 🌐 Совместимость браузеров

- Chrome >= 60
- Firefox >= 55  
- Safari >= 11
- Edge >= 79
- Поддержка WebRTC API
- Media Stream API

## 📋 Требования к разработке

### Code Style
- ESLint для проверки кода
- Prettier для форматирования
- Husky для git hooks
- Conventional commits

### TypeScript
- Строгая типизация
- Path mapping для импортов
- Типы для всех API
- Интерфейсы для компонентов

## 🚀 Деплой

### Продакшен сборка

```bash
npm run build
```

### Docker

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "run", "preview"]
```

### Nginx конфигурация

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/messenger;
    index index.html;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    location /api {
        proxy_pass http://backend:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## 🐛 Отладка

### Dev Tools
- React Developer Tools
- Redux DevTools
- Vite HMR
- Source maps

### Логирование
```javascript
// Только в development режиме
if (import.meta.env.DEV) {
  console.log('Debug info');
}
```

## 🤝 Участие в разработке

1. Fork проект
2. Создайте feature branch
3. Сделайте изменения
4. Добавьте тесты
5. Запустите линтер
6. Создайте Pull Request

### Правила коммитов

```
feat: добавить новую возможность
fix: исправить ошибку  
docs: обновить документацию
style: изменения стиля
refactor: рефакторинг кода
test: добавить тесты
chore: обновить зависимости
```

## 📄 Лицензия

MIT License - см. файл [LICENSE](LICENSE)

## 📞 Поддержка

- Issues: [GitHub Issues](https://github.com/your-username/messenger-frontend/issues)
- Документация: [Wiki](https://github.com/your-username/messenger-frontend/wiki)
- Email: support@messenger.com

## 🙏 Благодарности

- React Team за отличный фреймворк
- Tailwind CSS за удобные утилиты
- Socket.IO за WebSocket коммуникацию
- Сообщество разработчиков за поддержку