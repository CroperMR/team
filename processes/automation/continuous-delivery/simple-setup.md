# Упрощенная настройка Continuous Delivery

### Шаг 1. Jenkins

- Создаем новый проект. При создании выбираем *Copy from existing project: iOS-deploy-config-project*. Название выбираем по формату *Project.Name.iOS.Deploy*.

  ![Screenshot](/resources/complex-deploy-1.jpg)

- В разделе *Source Code Management* вводим адрес git-репозитория проекта.

  ![Screenshot](/resources/complex-deploy-2.jpg)

- Сохраняем изменения.

  ![Screenshot](/resources/complex-deploy-3.jpg)

- Создаем еще один новый проект. При создании выбираем *Copy from existing project: iOS-nightly-config-project*. Название выбираем по формату *Project.Name.iOS.Nightly*.

  ![Screenshot](/resources/complex-deploy-4.jpg)

- В разделе *Source Code Management* вводим адрес git-репозитория проекта.
- Сохраняем изменения.

### Шаг 2. Настройка Apple Developer Center

- Заведите новый Application Identifier для основного приложения вида `main_app_bundle_id.enterprise`. Проверьте, что все capabilities соответствуют исходному приложению.

### Шаг 3. Настройка Slack

- Заходите в [настройки нашей команды в Slack](https://ramblercoteam.slack.com/apps/manage/custom-integrations).
- Добавьте новый webhook: *Incoming WebHooks -> Add Configuration -> Выбираем канал -> Копируем Webhook URL*.
- В настройках Jenkins в разделе *Slack Notifications -> Расширенные* указываем правильный канал для бота.

### Шаг 4. Базовая настройка fastlane

- В корне вашего проекта создайте папку *fastlane*.
- Заполните файл `.env.default` по [образцу](/processes/automation/continuous-delivery/env-default-example.md). Не забудьте добавить полученный на предыдущем шаге webhook.
- Создайте новый `Fastfile` и добавьте в него следующий код

  ```ruby
  import_from_git(url: 'https://github.com/rambler-digital-solutions/fastlane-flows',
                 path: 'fastlane/Fastfile')
  ```
  
- Заполните `Appfile` по [образцу](/processes/automation/continuous-delivery/appfile-example.md). Названия lane'ов не меняются.
- В файл `.env.default` добавьте список групп Fabric, которым нужно автоматически рассылать билд. Группа RDS QA включена по умолчанию.

  ```
  FABRIC_GROUPS=group1,group2,group3
  ```
  
