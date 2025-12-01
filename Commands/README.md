# Console Commands (Yii2 /commands)

Полная документация по консольным контроллерам, расположенным в
директории `commands/`.

## Общая информация

Все команды запускаются через:

    php yii <controller>/<action> [params]

Контроллеры используются для фоновых задач, крон-процессов, миграций
данных, обслуживания учебного процесса и инфраструктуры.

------------------------------------------------------------------------

## Список доступных консольных контроллеров

### ChatFixController

Утилиты для обработки и фикса чатов.

### CleanOldHomeworkFilesController

Удаляет старые файлы домашних заданий.

### CleanUpDeletedUsersController

Обрабатывает пользователей со статусом INACTIVE: - мягко удаляет
связанные `class_user` - отключает `direction_orders` - очищает связи с
родителями - чистит `user_id` в CRM лидах

### CronController

Основная пачка крон-задач: - уведомления - отчёты - платежи - регулярные
процессы

### DevController

Dev/отладочные задачи.

### FixDynamicsController

Фиксы динамических курсов: - lesson_user - восстановление различных
данных

### FixHomeWorkAnswersController

Корректировки ответов ДЗ.

### FixHomeWorkController

Правки/миграции данных домашек.

### FixTimetableController

Фиксы и миграции расписания.

### InitController

Инициализационные команды.

### MailingController

Отправка рассылок и управление почтовыми задачами.

### MigrateLessonMediaToBlocksController

Перенос медиа уроков в блоки.

### ParseTimetableController

Парсит расписание и создаёт/обновляет отчёты.

### PaymentMigrateController

Миграции/коррекция платежей.

### PhoneFixController

Исправление телефонных номеров.

### QueController

Вспомогательные задачи очередей.

### RbacController

Управление RBAC: - init - assign-role - create-rules

### SetClassDefaultPriceController

Проставляет дефолтные amount в class_user.

### SurveyController

Задачи по опросам.

### TestEmailController

Проверочные отправки почты.

### WebSocketServerController

Запуск и обслуживание websocket-сервера.

### RemoveDeletedClassUsersController

Удаляет строки из `class_user` с заполненным deleted_at.\
Перед удалением обнуляет связанные `order_payments.class_user_id`.

------------------------------------------------------------------------

## Список доступных команд (controller/action)

    chat-fix:
      cleanup-old-reads <date>

    clean-old-homework-files:
      index

    clean-up-deleted-users:
      index

    cron:
      timetable-report
      check-polymorph-payments
      generate-parent-reports [interval]
      generate-sop
      check-filling-parent-reports <type>
      seminarian-fine-for-parent-report
      notify-last-event-on-month
      notify-lecture-evaluation
      notify-overdue-payment [dutyDays=-2]
      notify-referral-program
      notify-four-payment-days
      notify-in-payment-day
      notify-minus-three-payment-days
      notify-minus-nine-payment-days
      notify-timetable-report [type=notify]
      notify-ss-info-for-sale-department <type>
      generate-communications
      orders-extention-via-balance
      send-lesson-minutes-before-notification
      delete-old-notifications
      send-payment-deadline-notification
      delete-old-timers
      test
      update-access [limit offset verbose]
      notify-untaken-leads [separator]
      run-regular-notifications
      assign-homeworks-all [date]

    dev:
      get-all-answers
      convert-match
      convert-order
      convert-multiple
      convert-exact
      convert-pass-words
      set-promo-code
      change-auth <id>
      check-answer
      check-lesson-plan
      check-plans
      update-probe-works <id> <workId>
      task-answer
      calculate-score
      add-task-to-probe-work
      fixed-probe-result
      add-children <email> <phone> <name>
      create-user-profile <id>
      check-double-lesson-plan
      init-seminar-types
      payment-test
      record-seminarians-by-direction
      duplicate-direction <id> <newName> <filterId>
      convert-lesson-video
      change-plan
      restore-student-plan <groupId> <studentId>
      restore-group-orders
      pulses
      update-home-task-direction-ids
      init-sop-questions
      add-group-material-plan <groupId> <materialId>
      set-order-to-plan
      check-missing-plans
      update-home-task-direction
      open-first-course
      create-admin
      test-vk-api
      test-vk-event
      change-vk-links
      check-payment <orderId> <amount> <hours>

    fix-dynamics:
      restore-dynamics
      populate-lesson-user
      homework-to-progress
      remove-extra
      calc-progress
      fix-dates
      add-dynamic-id

    fix-home-work-answers:
      index [update] [taskId]

    fix-home-work:
      set-default-teacher-id [batchSize]
      preview-control-work-direction-ids
      fix-control-work-direction-ids [apply]

    fix-timetable:
      set-default-speaker [batchSize]
      set-default-attendees [batchSize]
      sync-meeting-links [batchSize]
      set-groups-text [batchSize]
      fix-end-dates

    init:
      run
      change-password <userId>
      create-test-users

    mailing:
      send-test-email
      send <mailingId>
      test <email>
      listen
      send-scheduled

    migrate-lesson-media-to-blocks:
      index

    parse-timetable:
      index

    payment-migrate:
      legacy-to-polymorph
      fix-catalog-type
      fix-dates
      fix-negative-amounts

    phone-fix:
      index

    que:
      test-queue
      listen

    remove-deleted-class-users:
      index

    rbac:
      init
      assign-role <userId> <role>
      create-rules

    set-class-default-price:
      index

    survey:
      resend <survey_id>

    test-email:
      send-base-message
      send-email-password

    web-socket-server:
      start

------------------------------------------------------------------------

## RemoveDeletedClassUsersController --- важное примечание

Команда физически удаляет строки из `class_user`, где заполнено
`deleted_at`.\
Чтобы не потерять историю платежей, перед удалением выполняется: -
`order_payments.class_user_id = NULL` для всех связанных платежей.

------------------------------------------------------------------------

## Назначение всех команд

Этот набор позволяет: - поддерживать чистоту данных - обрабатывать
крон-процессы - поддерживать учебный процесс - синхронизировать
расписание - отправлять уведомления - управлять RBAC - мигрировать
платежи - администрировать пользователей - поддерживать стабильность и
консистентность данных в LMS

------------------------------------------------------------------------

## Расположение

    /commands/*.php

Файлы являются стандартными Yii2 Console Controllers.

------------------------------------------------------------------------