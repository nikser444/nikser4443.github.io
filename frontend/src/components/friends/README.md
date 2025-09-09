# 👥 Компоненты системы друзей

Этот каталог содержит все компоненты React для управления друзьями в мессенджере. Система друзей включает добавление, поиск, управление заявками и взаимодействие с друзьями в реальном времени.

## 📁 Структура файлов

```
frontend/src/components/friends/
├── FriendsList.tsx      # Основной список друзей
├── FriendItem.tsx       # Элемент друга в списке
├── AddFriendModal.tsx   # Модальное окно добавления друга
├── FriendRequests.tsx   # Управление заявками в друзья
├── index.ts             # Центральный экспорт компонентов
└── README.md           # Эта документация
```

## 🧩 Компоненты

### FriendsList.tsx
Основной компонент для отображения списка друзей пользователя.

**Функции:**
- Отображение всех друзей с их статусом (онлайн/оффлайн)
- Поиск по имени пользователя или email
- Фильтрация по статусу (все/онлайн/оффлайн)
- Сортировка по статусу и активности
- Интеграция с AddFriendModal

**Используемые хуки:**
- `useFriends()` - основной хук для работы с друзьями
- `useSelector()` - доступ к состоянию Redux

### FriendItem.tsx
Компонент для отображения одного друга в списке.

**Функции:**
- Показ аватара с индикатором онлайн статуса
- Отображение последнего сообщения
- Кнопки быстрых действий (чат, звонок, видеозвонок)
- Контекстное меню с дополнительными действиями
- Подтверждение удаления из друзей

**Props:**
```typescript
interface FriendItemProps {
  friend: Friend;
  isSelected: boolean;
  isOnline: boolean;
  onClick: () => void;
  onStartChat: () => void;
}
```

### AddFriendModal.tsx
Модальное окно для поиска и добавления новых друзей.

**Функции:**
- Поиск по email или имени пользователя
- Валидация входных данных
- Отправка заявок в друзья
- Отображение результатов поиска
- Статус отправленных заявок

**Props:**
```typescript
interface AddFriendModalProps {
  isOpen: boolean;
  onClose: () => void;
}
```

### FriendRequests.tsx
Компонент для управления входящими и исходящими заявками в друзья.

**Функции:**
- Просмотр входящих заявок
- Просмотр исходящих заявок
- Принятие/отклонение заявок
- Отмена отправленных заявок
- Статистика заявок

**Props:**
```typescript
interface FriendRequestsProps {
  isOpen: boolean;
  onClose: () => void;
}
```

## 🔧 Интеграция

### Redux Store
Компоненты используют `friendSlice` для управления состоянием:

```typescript
// Состояние друзей в store
interface FriendsState {
  friends: Friend[];
  incomingRequests: FriendRequest[];
  outgoingRequests: FriendRequest[];
  searchResults: FriendSearchResult[];
  onlineFriends: string[];
  selectedFriend: Friend | null;
  isLoading: boolean;
  error: string | null;
}
```

### API Endpoints
Компоненты взаимодействуют с backend через следующие эндпоинты:

- `GET /api/friends` - получить список друзей
- `GET /api/friends/search` - поиск пользователей
- `POST /api/friends/requests/{id}/accept` - принять заявку
- `POST /api/friends/requests/{id}/decline` - отклонить заявку
- `DELETE /api/friends/{id}` - удалить из друзей
- `POST /api/friends/block/{id}` - заблокировать пользователя

### WebSocket События
Компоненты реагируют на события в реальном времени:

```typescript
// События Socket.io
'friend:request:received'  // Получена новая заявка
'friend:request:accepted'  // Заявка принята
'friend:removed'          // Друг удален
'friend:online'           // Друг вошел в сеть
'friend:offline'          // Друг вышел из сети
```

## 🎣 Кастомные хуки

### useFriends()
Основной хук для работы с системой друзей:

```typescript
const {
  friends,              // Список всех друзей
  sortedFriends,        // Отсортированный список
  incomingRequests,     // Входящие заявки
  outgoingRequests,     // Исходящие заявки
  onlineFriends,        // Друзья онлайн
  isLoading,           // Статус загрузки
  
  // Действия
  sendRequest,         // Отправить заявку
  acceptRequest,       // Принять заявку
  removeFriendById,    // Удалить друга
  searchFriends,       // Поиск друзей
} = useFriends();
```

### useFriend(friendId)
Хук для работы с конкретным другом:

```typescript
const {
  friend,              // Данные друга
  isOnline,           // Статус онлайн
  formatLastSeen,     // Форматированное время
} = useFriend(friendId);
```

### useFriendsStats()
Хук для получения статистики:

```typescript
const {
  totalFriends,        // Общее количество друзей
  onlineFriends,       // Количество онлайн
  pendingRequests,     // Ожидающие заявки
  hasUnreadRequests,   // Есть ли непрочитанные заявки
} = useFriendsStats();
```

## 📱 Использование компонентов

### Основное использование

```tsx
import { FriendsList, FriendRequests, AddFriendModal } from '../components/friends';

function FriendsPage() {
  const [showRequests, setShowRequests] = useState(false);
  
  return (
    <div className="friends-page">
      <FriendsList />
      
      <FriendRequests
        isOpen={showRequests}
        onClose={() => setShowRequests(false)}
      />
    </div>
  );
}
```

### Интеграция с чатами

```tsx
import { useFriends } from '../hooks/useFriends';
import { useChat } from '../hooks/useChat';

function ChatSidebar() {
  const { friends, selectFriend } = useFriends();
  const { startChatWithUser } = useChat();
  
  const handleStartChat = (friendId: string) => {
    startChatWithUser(friendId);
  };
  
  return (
    <FriendsList onStartChat={handleStartChat} />
  );
}
```

## 🎨 Стилизация

Компоненты используют Tailwind CSS для стилизации:

```css
/* Основные классы */
.friend-item-online     /* Друг онлайн */
.friend-item-offline    /* Друг оффлайн */
.friend-item-selected   /* Выбранный друг */
.friend-request-new     /* Новая заявка */

/* Цветовая схема */
.text-green-500         /* Статус онлайн */
.text-gray-400          /* Статус оффлайн */
.bg-blue-50            /* Выделение выбранного */
```

## 🔒 Безопасность

### Валидация данных
- Email валидация при поиске
- Проверка длины имени пользователя (3-30 символов)
- Санитизация поисковых запросов

### Приватность
- Возможность скрыть статус онлайн
- Настройки приватности друзей
- Блокировка нежелательных пользователей

## ⚡ Производительность

### Оптимизации
- Мемоизация компонентов с `React.memo`
- Виртуализация длинных списков
- Дебаунс поиска (300мс)
- Ленивая загрузка изображений

### Кеширование
- Кеширование результатов поиска
- Сохранение состояния друзей в Redux
- Оптимистичные обновления UI

## 🐛 Обработка ошибок

### Типы ошибок
- Сетевые ошибки при API запросах
- Ошибки валидации данных
- Ошибки WebSocket соединения
- Ошибки доступа к ресурсам

### Обработка
```typescript
// Пример обработки ошибки
const { sendRequest, error } = useFriends();

const handleSendRequest = async (userId: string) => {
  const result = await sendRequest(userId);
  
  if (!result.success) {
    toast.error(`Ошибка: ${result.error}`);
  } else {
    toast.success('Заявка отправлена!');
  }
};
```

## 🧪 Тестирование

### Unit тесты
```bash
# Запуск тестов компонентов
npm test -- --testPathPattern=friends

# Тесты с покрытием
npm test -- --coverage --testPathPattern=friends
```

### Примеры тестов
```typescript
describe('FriendsList', () => {
  test('displays friends correctly', () => {
    render(<FriendsList />);
    expect(screen.getByText('Друзья')).toBeInTheDocument();
  });
  
  test('filters friends by search query', () => {
    // Тест фильтрации
  });
});
```

## 🔧 Конфигурация

### Константы
```typescript
export const FRIENDS_CONSTANTS = {
  MAX_SEARCH_RESULTS: 10,
  MIN_USERNAME_LENGTH: 3,
  ONLINE_THRESHOLD_MINUTES: 5,
  REQUEST_EXPIRY_DAYS: 30,
  MAX_FRIENDS_COUNT: 1000
};
```

### Настройки по умолчанию
- Максимум результатов поиска: 10
- Таймаут онлайн статуса: 5 минут  
- Срок действия заявок: 30 дней
- Максимум друзей: 1000

## 📚 Типы TypeScript

### Основные интерфейсы
```typescript
interface Friend extends User {
  friendshipId: string;
  friendedAt: Date;
  lastMessage?: LastMessage;
}

interface FriendRequest {
  id: string;
  senderId: string;
  receiverId: string;
  sender: User;
  receiver: User;
  status: FriendRequestStatus;
  createdAt: Date;
}

enum FriendRequestStatus {
  PENDING = 'pending',
  ACCEPTED = 'accepted',
  DECLINED = 'declined'
}
```

## 🚀 Развертывание

### Требования
- React 18+
- TypeScript 4.5+
- Redux Toolkit
- Socket.io Client
- Tailwind CSS

### Сборка
```bash
# Разработка
npm run dev

# Продакшн
npm run build
```

## 🔄 Миграции

### Версия 1.0 → 1.1
- Добавлена поддержка групповых чатов
- Улучшена система уведомлений
- Оптимизирована производительность

### Планы на будущее
- Видеозвонки в группах
- Статусы активности
- Интеграция с календарем
- Расширенная приватность

## 🤝 Вклад в разработку

### Код стайл
- Используйте TypeScript для всех компонентов
- Следуйте правилам ESLint
- Добавляйте JSDoc комментарии
- Пишите тесты для новых функций

### Пример PR
1. Создайте ветку `feature/friends-improvement`
2. Внесите изменения
3. Добавьте тесты
4. Обновите документацию
5. Создайте Pull Request

## 📞 Поддержка

При возникновении проблем:
1. Проверьте консоль браузера на ошибки
2. Убедитесь в корректности API эндпоинтов
3. Проверьте WebSocket соединение
4. Обратитесь к разработчикам через Issues/friends/requests` - отправить заявку
- `POST /api