# Ansible role для установки Vector

## Содержание
- [Обзор](#обзор)
- [Требования](#требования)
- [Переменные](#переменные)
- [Использование](#использование)
- [Логика работы](#логика-работы-роли)

## Обзор
Роль vector-role предназначена для установки и настройки сервиса Vector непосредственно из репозитория, а также для замены конфигурационного файла с применением шаблона. Роль позволяет легко интегрировать установку и конфигурацию Vector в ваши Ansible-проекты, обеспечивая модульную и удобную поддержку.

## Требования

- **Ansible:** Версия 2.9 или выше.
- **Управляемые хосты:**
  - Операционная система: Совместима с дистрибутивами на основе RPM (например, CentOS, RHEL, Fedora).
  - Менеджер пакетов: Должен быть доступен yum.
- **Сетевой доступ:**
  - Возможность скачивания пакетов с внешних URL.
  - SSH-доступ к управляемым хостам.

## Переменные

# defaults/main.yml

- vector_version – версия Vector, которую можно переопределить при необходимости.  
  *Пример:* "0.41.1"

# vars/main.yml

- vector_rpm_version – версия RPM пакета Vector, которая используется для формирования URL установки.  
  *Пример:* "0.41.1-1"

## Использование

1. **Установка роли:**  
   Склонируйте или добавьте роль vector-role в ваш проект Ansible.

2. **Интеграция в playbook:**  
    Добавьте роль в ваш основной playbook:
    ```yml
    - name: Install Vector
      hosts: vector
      become: true
      tags: vector
      roles:
        - vector
    ```

## Логика работы роли

1. **Установка Vector:**  
    Роль скачивает и устанавливает RPM-пакет Vector по URL, сформированному с учётом переменной vector_rpm_version. Если установка проходит успешно, срабатывает уведомление для перезапуска сервиса.

2. **Конфигурация Vector:**
    Роль заменяет конфигурационный файл Vector в /etc/vector/vector.yaml с использованием шаблона vector.toml.j2 и применения заданных настроек. После замены также вызывается обработчик перезапуска сервиса.

3. **Перезапуск сервиса:**  
    Обработчик Restart Vector Service обеспечивает автоматический перезапуск сервиса Vector при изменении конфигурации или после установки пакета.
