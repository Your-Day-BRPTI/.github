## Доступ к приложению
### Интерфейс - https://m2.n.nikonov.fvds.ru

### API - https://m2.n.nikonov.fvds.ru/api/


## Репозиторий
- Пространство - https://github.com/Your-Day-BRPTI (вход по приглашениям)
- Фронтенд - https://github.com/Your-Day-BRPTI/frontend
- Бэкенд - https://github.com/Your-Day-BRPTI/backend
- Инфраструктура - https://github.com/Your-Day-BRPTI/infra

## Пайплайны

1. Frontend CI/CD Pipeline
**Trigger**: Процесс запускается при пуше в ветку master репозитория frontend.
Шаги:
- **Checkout Code**: Использование actions/checkout@v2 для извлечения кода из репозитория.
- **Get Commit Hash**: Определение короткого хэша коммита для версионирования Docker-образа.
- **Build Image**: Создание Docker-образа фронтенда с использованием Dockerfile.
- **Login to Docker Hub**: Аутентификация в Docker Hub с помощью токена доступа.
- **Push Image**: Пуш собранного образа с тегом последнего коммита и тега latest в Docker Hub.
- **Trigger Infrastructure Build**: После успешного пуша образа инициируется триггер через repository_dispatch для деплоя инфраструктуры.
2. Backend CI/CD Pipeline
**Trigger**: Запуск при пуше в ветку master репозитория backend.
Шаги:
- **Checkout Code**: Извлечение кода из репозитория.
- **Create .env File**: Создание файла окружения .env с переменными среды для разработки.
- **Get Commit Hash**: Получение короткого хэша текущего коммита.
- **Build Image**: Сборка Docker-образа backend с использованием Dockerfile.
- **Login to Docker Hub**: Аутентификация в Docker Hub для публикации образов.
- **Push Image**: Публикация образа в Docker Hub с тегами последнего коммита и latest.
- **Trigger Infrastructure Build**: Инициирование деплоя инфраструктуры через repository_dispatch.
3. Infrastructure CI/CD Pipeline
- **Trigger**:
  - Пайплайн запускается при пуше в ветку master репозитория infra.
  - Также он может быть запущен через repository_dispatch события, которые приходят от frontend или backend репозиториев с типами событий build_front_end или build_back_end.
Шаги:
- **Checkout Code**: Извлечение кода инфраструктуры.
- **Create .env File**: Создание файла .env с переменными окружения для инфраструктуры.
- **Set Up SSH Key**: Настройка SSH для подключения к серверу деплоя с использованием приватного ключа.
- **Copy Files to Server**: Копирование файлов на удаленный сервер по SSH.
- **Deploy to Server**: 
  - Аутентификация в Docker Hub на сервере.
  - Остановка текущих контейнеров с помощью docker-compose down.
  - Вытягивание последних версий образов с помощью docker-compose pull.
  - Запуск контейнеров в фоновом режиме с помощью docker-compose up -d.
