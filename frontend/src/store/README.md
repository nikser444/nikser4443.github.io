# Redux Store для мессенджера

Полная реализация глобального состояния приложения с использованием Redux Toolkit.

## 📁 Структура

```
src/store/
├── index.ts              # Главный store и конфигурация
├── authSlice.ts          # Состояние авторизации
├── chatSlice.ts          # Состояние чатов и сообщений
├── userSlice.ts          # Состояние пользователя и настроек
├── friendSlice.ts        # Состояние друзей и заявок
├── callSlice.ts          # Состояние звонков и WebRTC
├── notificationSlice.ts  # Состояние уведомлений
├── hooks.ts              # Типизированные хуки
├── middleware.ts         # Кастомные middleware
├── StoreProvider.tsx     # Провайдер для React
└── README.md             # Эта документация
```

## 🚀 Использование

### Подключение к приложению

```tsx
// App.tsx
import { StoreProvider } from './store/StoreProvider';

function App() {
  return (
    <StoreProvider>
      <YourApp />
    </StoreProvider>
  );
}
```

### Использование типизированных хуков

```tsx
// В компонентах
import { useAuth, useChat, useAppDispatch } from '../store/hooks';
import { loginUser } from '../store/authSlice';

const LoginComponent = () => {
  const { user, isLoading, error } = useAuth();
  const dispatch = useAppDispatch();

  const handleLogin = (email: string, password: string) => {
    dispatch(loginUser({ email, password }));
  };

  // ...
};
```

## 📊 Слайсы состояния

### AuthSlice - Авторизация
Управляет состоянием аутентификации пользователя.

**Состояние:**
- `user`: Данные текущего пользователя
- `token`: JWT токен
- `isAuthenticated`: Статус авторизации
- `isLoading`: Состояние загрузки
- `error`: Ошибки авторизации

**Основные действия:**
- `loginUser()` - Вход в систему
- `registerUser()` - Регистрация
- `logoutUser()` - Выход из системы
- `checkAuthUser()` - Проверка авторизации
- `refreshToken()` - Обновление токена

### ChatSlice - Чаты и сообщения
Управляет чатами, сообщениями и связанной функциональностью.

**Состояние:**
- `chats`: Список чатов
- `messages`: Сообщения по чатам
- `activeChat`: Активный чат
- `typingUsers`: Пользователи, которые печатают
- `unreadCounts`: Количество непрочитанных сообщений

**Основные действия:**
- `fetchChats()` - Загрузка чатов
- `fetchMessages()` - Загрузка сообщений
- `sendMessage()` - Отправка сообщения
- `createChat()` - Создание чата
- `searchMessages()` - Поиск сообщений

### UserSlice - Пользователь и настройки
Управляет профилем пользователя и настройками.

**Состояние:**
- `profile`: Профиль пользователя
- `status`: Статус (онлайн/оффлайн/занят)
- `settings`: Настройки приложения
- `onlineUsers`: Список онлайн пользователей

**Основные действия:**
- `fetchUserProfile()` - Загрузка профиля
- `updateUserProfile()` - Обновление профиля
- `uploadAvatar()` - Загрузка аватара
- `updateUserSettings()` - Обновление настроек

### FriendSlice - Друзья и заявки
Управляет системой друзей и заявками.

**Состояние:**
- `friends`: Список друзей
- `sentRequests`: Отправленные заявки
- `receivedRequests`: Полученные заявки
- `blockedUsers`: Заблокированные пользователи
- `suggestions`: Рекомендации друзей

**Основные действия:**
- `fetchFriends()` - Загрузка друзей
- `sendFriendRequest()` - Отправка заявки в друзья
- `acceptFriendRequest()` - Принятие заявки
- `blockUser()` - Блокировка пользователя

### CallSlice - Звонки и WebRTC
Управляет голосовыми и видеозвонками.

**Состояние:**
- `currentCall`: Текущий звонок
- `incomingCall`: Входящий звонок
- `callHistory`: История звонков
- `mediaSettings`: Настройки медиа
- `participants`: Участники конференции

**Основные действия:**
- `initiateCall()` - Инициация звонка
- `acceptCall()` - Принятие звонка
- `endCall()` - Завершение звонка
- `createConference()` - Создание конференции

### NotificationSlice - Уведомления
Управляет системой уведомлений.

**Состояние:**
- `notifications`: Список уведомлений
- `toasts`: Временные уведомления
- `unreadCount`: Количество непрочитанных
- `settings`: Настройки уведомлений

**Основные действия:**
- `fetchNotifications()` - Загрузка уведомлений
- `markAsRead()` - Отметить как прочитанное
- `addToast()` - Добавить toast уведомление
- `requestNotificationPermission()` - Запрос разрешений

## 🔧 Middleware

### apiErrorMiddleware
Обрабатывает ошибки API и показывает соответствующие уведомления.

### activityMiddleware
Отслеживает активность пользователя для поддержания сессии.

### settingsMiddleware
Автоматически сохраняет настройки пользователя.

### validationMiddleware
Проверяет консистентность состояния (только в development).

### websocketMiddleware
Обрабатывает действия для отправки через WebSocket.

## 🎣 Кастомные хуки

Все хуки типизированы и предоставляют удобный доступ к состоянию:

```tsx
// Основные хуки для получения состояния
useAuth()           // Состояние авторизации
useChat()           // Состояние чатов
useUser()           // Состояние пользователя
useFriend()         // Состояние друзей
useCall()           // Состояние звонков
useNotification()   // Состояние уведомлений

// Специфические хуки
useIsFriend(userId)         // Проверка дружбы
useIsUserOnline(userId)     // Проверка онлайн статуса
useChatMessages(chatId)     // Сообщения чата
useUnreadCount(chatId)      // Непрочитанные сообщения
useIsDoNotDisturb()         // Режим "Не беспокоить"
```

## 💾 Персистентность

Состояние сохраняется в localStorage с помощью redux-persist:

- ✅ **Сохраняются:** auth, user (настройки и профиль)
- ❌ **Не сохраняются:** chat, call, notification (данные реального времени)

## 🔄 Интеграция с WebSocket

Store интегрирован с WebSocket для обновлений в реальном времени:

```tsx
// Пример обработки WebSocket событий
socket.on('message:receive', (message) => {
  dispatch(addMessage(message));
});

socket.on('user:online', (userId) => {
  dispatch(addOnlineUser(userId));
});
```

## 🚨 Обработка ошибок

- Автоматические toast уведомления для критических ошибок
- Логирование ошибок в development режиме
- Автоматический logout при истечении сессии

## 📱 Типизация

Все действия, состояния и селекторы полностью типизированы с TypeScript:

```tsx
// Типы экспортируются из store
import type { RootState, AppDispatch } from './store';

// Все хуки возвращают типизированные данные
const { user, isLoading }: {
  user: User | null;
  isLoading: boolean;
} = useAuth();
```

## 🎯 Селекторы

Предопределенные селекторы для удобного доступа к данным:

```tsx
import { 
  selectAuth, 
  selectChats, 
  selectActiveChat 
} from './store';

const authState = useAppSelector(selectAuth);
```

## ⚡ Производительность

- Использование Redux Toolkit для оптимизированных обновлений
- Мемоизация селекторов
- Ленивая загрузка данных через async thunks
- Пагинация для больших списков (сообщения, история звонков)

## 🛠️ Разработка и отладка

- Redux DevTools интеграция
- Подробное логирование в development режиме
- Валидация состояния
- Hot reloading поддержка

---

Этот store обеспечивает полное управление состоянием для всех функций мессенджера: авторизации, чатов, звонков, друзей и уведомлений с поддержкой TypeScript и современными практиками Redux Toolkit.