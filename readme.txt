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
ansible-playbook playbook.yml --ask-vault-pass

Использование.

Все чувствительные данные хранятся в зашифрованном виде в:
inventori.ini
vars.yml

Структура проекта
.
├── ansible.cfg                # Конфигурация Ansible
├── vars.yml                   # Зашифрованные переменные      
├── inventory.ini              # Зашифрованный файл инвентари
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
