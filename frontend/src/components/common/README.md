# Common Components 📦

Папка с общими React компонентами для мессенджера. Все компоненты созданы с использованием TypeScript и Tailwind CSS, поддерживают темную тему и адаптивность.

## Структура

```
common/
├── Avatar.tsx          🖼️ Компонент аватара пользователя
├── Button.tsx          🔘 Универсальная кнопка с вариантами
├── Header.tsx          🏠 Шапка приложения с навигацией
├── Input.tsx           ⌨️ Поля ввода и текстовые области
├── LoadingSpinner.tsx  ⏳ Компоненты загрузки
├── Modal.tsx           🪟 Модальные окна
├── Sidebar.tsx         📱 Боковая панель навигации
├── Tooltip.tsx         💬 Всплывающие подсказки
├── types.ts            📝 TypeScript типы
├── index.ts            📤 Файл экспорта
└── README.md           📚 Документация
```

## Компоненты

### 🖼️ Avatar
Компонент для отображения аватара пользователя с поддержкой статуса и инициалов.

```tsx
import { Avatar, AvatarGroup, AvatarWithName } from '../common';

// Базовое использование
<Avatar src="/avatar.jpg" alt="John Doe" size="md" status="online" />

// Группа аватаров
<AvatarGroup
  avatars={[
    { src: "/user1.jpg", alt: "User 1", status: "online" },
    { src: "/user2.jpg", alt: "User 2", status: "away" }
  ]}
  max={3}
/>

// Аватар с именем
<AvatarWithName
  src="/avatar.jpg"
  name="John Doe"
  subtitle="В сети"
  size="lg"
  status="online"
/>
```

**Пропсы:**
- `src` - URL изображения
- `alt` - Альтернативный текст
- `size` - Размер: `xs | sm | md | lg | xl | 2xl`
- `status` - Статус: `online | offline | away | busy`
- `shape` - Форма: `circle | square`

### 🔘 Button
Универсальная кнопка с различными вариантами и состояниями.

```tsx
import { Button, ButtonGroup, IconButton } from '../common';

// Базовое использование
<Button variant="primary" size="md" onClick={handleClick}>
  Отправить
</Button>

// Кнопка с загрузкой
<Button isLoading loadingText="Отправка...">
  Отправить
</Button>

// Группа кнопок
<ButtonGroup>
  <Button>Первая</Button>
  <Button>Вторая</Button>
  <Button>Третья</Button>
</ButtonGroup>

// Иконка кнопка
<IconButton
  icon={<SendIcon />}
  aria-label="Отправить сообщение"
  variant="primary"
/>
```

**Варианты:** `primary | secondary | danger | warning | success | ghost | outline`
**Размеры:** `xs | sm | md | lg | xl`

### ⌨️ Input
Поля ввода с поддержкой валидации, иконок и различных состояний.

```tsx
import { Input, Textarea } from '../common';

// Базовое поле ввода
<Input
  label="Email"
  type="email"
  placeholder="example@mail.com"
  error={errors.email}
  leftIcon={<EmailIcon />}
/>

// Поле пароля
<Input
  label="Пароль"
  type="password"
  placeholder="Введите пароль"
/>

// Текстовая область
<Textarea
  label="Сообщение"
  placeholder="Введите сообщение..."
  rows={4}
  resize="vertical"
/>
```

**Варианты:** `default | filled | outlined`
**Размеры:** `sm | md | lg`

### 🪟 Modal
Модальные окна с поддержкой различных размеров и настроек.

```tsx
import { Modal, ConfirmModal } from '../common';

// Базовое модальное окно
<Modal
  isOpen={isOpen}
  onClose={handleClose}
  title="Заголовок"
  size="md"
>
  Содержимое модального окна
</Modal>

// Модальное окно подтверждения
<ConfirmModal
  isOpen={showConfirm}
  onClose={() => setShowConfirm(false)}
  onConfirm={handleConfirm}
  title="Удалить чат?"
  message="Это действие нельзя отменить"
  variant="danger"
  confirmText="Удалить"
  cancelText="Отмена"
/>
```

**Размеры:** `sm | md | lg | xl | full`
**Варианты подтверждения:** `info | warning | danger`

### 🏠 Header
Шапка приложения с навигацией, поиском и уведомлениями.

```tsx
import { Header } from '../common';

<Header className="sticky top-0 z-10" />
```

**Функции:**
- Поиск пользователей и чатов
- Уведомления с счетчиком
- Меню профиля
- Поддержка темной темы

### 📱 Sidebar
Боковая панель навигации с меню и статусом подключения.

```tsx
import { Sidebar } from '../common';

<Sidebar
  isCollapsed={collapsed}
  onToggleCollapse={() => setCollapsed(!collapsed)}
/>
```

**Функции:**
- Сворачиваемое меню
- Индикаторы статуса
- Счетчики непрочитанных
- Список онлайн пользователей

### 💬 Tooltip
Всплывающие подсказки с автоматическим позиционированием.

```tsx
import { Tooltip } from '../common';

<Tooltip content="Это подсказка" position="top">
  <Button>Наведите мышь</Button>
</Tooltip>
```

**Позиции:** `top | bottom | left | right`
**Настройки:** задержка, отключение, кастомные стили

### ⏳ LoadingSpinner
Компоненты загрузки для различных сценариев.

```tsx
import { LoadingSpinner, Skeleton, DotsLoader } from '../common';

// Спиннер загрузки
<LoadingSpinner size="md" text="Загрузка..." />

// Скелетон для контента
<Skeleton variant="text" lines={3} />
<Skeleton variant="rectangular" width="100%" height="200px" />
<Skeleton variant="circular" width="40px" height="40px" />

// Точки загрузки
<DotsLoader size="md" color="bg-blue-600" />
```

## Использование

### Импорт компонентов

```tsx
// Импорт отдельных компонентов
import { Button, Input, Modal } from './components/common';

// Импорт типов
import type { ButtonProps, InputProps } from './components/common';
```

### Темизация

Все компоненты поддерживают темную тему через Tailwind CSS:

```tsx
// Компоненты автоматически адаптируются к теме
<div className="dark">
  <Button variant="primary">Кнопка в темной теме</Button>
</div>
```

### Кастомизация

Каждый компонент принимает `className` для кастомизации:

```tsx
<Button 
  className="my-custom-styles" 
  variant="primary"
>
  Кастомная кнопка
</Button>
```

## Доступность (A11y)

Все компоненты созданы с учетом принципов доступности:

- ✅ Поддержка клавиатурной навигации
- ✅ ARIA атрибуты
- ✅ Семантическая HTML разметка
- ✅ Поддержка скрин-ридеров
- ✅ Контрастные цвета
- ✅ Фокус индикаторы

## Адаптивность

Компоненты адаптивны и корректно отображаются на всех устройствах:

- 📱 Мобильные устройства (320px+)
- 📟 Планшеты (768px+) 
- 💻 Десктопы (1024px+)
- 🖥️ Широкие экраны (1280px+)

## Интеграция с проектом

Компоненты интегрированы с:

- 🔗 React Router для навигации
- 🏪 Redux/Zustand для состояния
- 🔌 Socket.io для реального времени
- 🎨 Tailwind CSS для стилей
- 📝 React Hook Form для форм

## Развитие

Для добавления новых компонентов:

1. Создайте файл компонента в папке `common/`
2. Добавьте экспорт в `index.ts`
3. Добавьте типы в `types.ts` (если нужно)
4. Обновите документацию

## Примеры использования

Смотрите примеры в других папках компонентов:
- `auth/` - использование в формах авторизации
- `chat/` - использование в интерфейсе чата
- `calls/` - использование в интерфейсе звонков
- `friends/` - использование в управлении друзьями