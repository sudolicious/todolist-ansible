Деплой приложения с помощью Ansible

Репозиторий содержит Ansible-конфигурацию для развертывания приложения 
ToDoList с использованием зашифрованных переменных через Ansible Vault.

Предварительные требования
- Ansible 2.9+
- Доступ к целевым серверам по SSH
- Пароль для расшифровки vault (будет запрошен при запуске)

Настройка

1. Клонирование репозитория:
git clone https://github.com/sudolicious/todolist-ansible.git
cd todolist-ansible

2.Запуск плейбука
ansible-playbook playbook.yml -i inventory/inventory.ini --ask-vault-pass

Использование.
Редактирование зашифрованных переменных:
ansible-vault edit inventory/vault.yml

Управление секретами
Все чувствительные данные хранятся в зашифрованном виде в:
inventory/vault.yml
group_vars/all/vault.yml

Структура проекта
.
├── ansible.cfg                # Конфигурация Ansible
├── group_vars/                # Переменные для групп хостов
│   └── all/                   # Переменные для всех хостов
│       └── vars.yml           # Основные переменные
│       └── vault.yml          # Зашифрованные переменные БД
├── inventory/                 # Инвентаризация
│   ├── inventory.ini          # Файл инвентари
│   └── vault.yml              # Зашифрованные данные инвентари
├── playbook.yml               # Основной плейбук
└── roles/                     # Роли Ansible
    ├── backend/               # Роль backend-приложения
    │   ├── tasks/             # Задачи
    │   └── templates/         # Шаблоны конфигураций
    ├── common/                # Общие настройки
    │   └── tasks/             # Общие задачи
    ├── frontend/              # Роль frontend-приложения
    │   └── tasks/             # Задачи
    └── postgres/              # Роль базы данных PostgreSQL
        └── tasks/             # Задачи
