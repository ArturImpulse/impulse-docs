# Yii2 Migrations — README

Этот файл описывает набор миграций проекта на **Yii 2**: назначение, порядок применения и «шпаргалку» по работе с миграциями.

---

## Как применять миграции

### 1) Настройка БД
Убедись, что в конфиге приложения корректно настроен компонент `db` (DSN/логин/пароль).

### 2) Применение
```bash
php yii migrate
```

### 3) Откат (при необходимости)
```bash
php yii migrate/down 1
# или
php yii migrate/down all
```

### 4) Миграции в нестандартных путях
Если миграции лежат не в `@app/migrations`, укажи путь:
```bash
php yii migrate --migrationPath=@app/migrations
```

---

## Важно про порядок и зависимости

- Миграции **выполняются по времени в имени файла** (`mYYMMDD_HHMMSS_*` или близкий формат).
- Многие миграции добавляют/меняют FK и поля — **не меняй порядок вручную**, если не понимаешь зависимости.
- В списке ниже есть **повторы и опечатки в названиях** (как в исходных данных). В кодовой базе ориентируйся на **реально существующие файлы** и историю применения в таблице `migration`.

---

## Краткая карта доменов (что где появляется)

- **Пользователи и профили**: `users`, `user_profile`, `user_refresh_tokens`, `admin_passwords`
- **Учебный контент**: `direction`, `course_*`, `topic_*`, `lesson_*`, `course_material`, `home_task*`
- **Планы обучения**: `course_plan`, `material_plan`, `lesson_plan`
- **Домашние задания / результаты**: `home_work*`, `home_work_result`, `home_work_unique_tasks`
- **Расписание и журнал**: `timetable*`, `timetable_report`, `journal_timetable`, `timetable_members`, `lesson_abstract*`
- **CRM/продажи**: `lead*`, `manager`, `head_of_sales*`, `status`, `source`, `traffic_finances`
- **Платежи**: `payment`, `order_payments`, `promo_code`, доп. поля к заказам

---

## Полный список миграций (как предоставлено)

> Формат: **№ | Название (путь) | Что делает (русский)**

| № | Название (путь) | Что делает (русский) |
|---:|---|---|
| 1 | m210317_000001_create_admin_passwords_table.php | Создаёт таблицу admin_passwords – хранит пароли админов. |
| 2 | m220324_041743_create_user_table.php | Создаёт таблицу пользователей users с основными полями. |
| 3 | m220328_043012_create_direction_table.php | Создаёт таблицу направлений (courses). |
| 4 | m220328_044123_create_direction_course_table.php | Связывает направления и курсы – создаёт связующую таблицу. |
| 5 | m220328_045005_create_course_topic_table.php | Создаёт таблицу тем курса. |
| 6 | m220328_053703_create_course_lesson_table.php | Создаёт таблицу уроков курсов. |
| 7 | m220328_063149_create_home_task_table.php | Создаёт таблицу домашних заданий. |
| 8 | m220331_025242_create_study_group_table.php | Создаёт таблицу групп обучения. |
| 9 | m220331_033336_create_study_group_childs_table.php | Создаёт таблицу детей в группах. |
| 10 | m220725_025751_create_user_profile_table.php | Создаёт таблицу профилей пользователей. |
| 11 | m220725_031021_drop_user_name_columns_from_user_table.php | Удаляет столбцы имени из таблицы users. |
| 12 | m220727_044934_create_user_refresh_tokens_table.php | Создаёт таблицу токенов обновления авторизации. |
| 13 | m220801_042244_add_education_column_to_user_profile_table.php | Добавляет колонку education в профиль пользователя. |
| 14 | m220801_042511_add_column_work_experience_to_user_profile_table.php | Добавляет опыт работы в профиль. |
| 15 | m220801_053048_add_about_column_to_user_profile_table.php | Добавляет колонку about в профиль. |
| 16 | m220801_053220_add_notes_column_to_user_profile_table.php | Добавляет заметки к профилю. |
| 17 | m220801_053517_add_contracts_column_to_user_profile_table.php | Добавляет список контрактов в профиль. |
| 18 | m220801_063801_drop_name_column_from_user_profile_table.php | Удаляет колонку name из профиля пользователя. |
| 19 | m220801_064347_change_structure_user_profile_table.php | Перестраивает схему таблицы user_profile (изменён порядок/типы). |
| 20 | m220802_021030_move_name_column_from_user_profile_to_user_table.php | Переносит поле name из профиля в основную таблицу users. |
| 21 | m220803_025146_change_direction_table_structure.php | Модифицирует структуру таблицы direction. |
| 22 | m220803_031523_create_direction_filter_table.php | Создаёт таблицу фильтров для направлений. |
| 23 | m220803_032934_add_filter_id_column_to_direction_table.php | Добавляет FK filter_id в таблицу directions. |
| 24 | m220803_034556_create_direction_tariff_table.php | Создаёт тарифы по направлениям. |
| 25 | m220803_041204_alter_code_column_in_direction_course_table.php | Изменяет тип/размер колонки code в таблице direction_course. |
| 26 | m220804_064127_change_structure_direction_course_table.php | Перестраивает структуру таблицы direction_course. |
| 27 | m220804_072121_change_structure_home_task_table.php | Изменяет схему домашнего задания. |
| 28 | m220804_094530_alter_change_column_name_option_not_all_mismatch_on_home_task_table.php | Переименовывает колонку option в home_task, добавляя проверку. |
| 29 | m220804_110634_create_course_material_table.php | Создаёт таблицу материалов курса. |
| 30 | m220804_111343_create_material_lesson_table.php | Создаёт таблицу связей материала‑урок. |
| 31 | m220804_144633_create_lesson_tasks_table.php | Создаёт таблицу задач урока. |
| 32 | m220804_145148_create_control_tasks_table.php | Создаёт контрольные задачи (контрольный тест). |
| 33 | m220804_150815_change_structure_course_material_table.php | Перестраивает таблицу course_material. |
| 34 | m220804_151946_create_colloquium_task_table.php | Создаёт таблицу задач коллоквиума. |
| 35 | m220804_152158_create_colloquium_tasks_table.php | Создаёт таблицу колл‑задач (многоуровневая). |
| 36 | m220804_155143_add_column_sorting_to_course_material_table.php | Добавляет поле сортировки в course_material. |
| 37 | m220805_025501_create_topic_lesson_table.php | Создаёт таблицу урока темы. |
| 38 | m220805_031853_change_structure_topic_lesson_table.php | Перестраивает структуру таблицы topic_lesson. |
| 39 | m220805_034145_change_structure_lesson_tasks_table.php | Перестраивает структуру lesson_tasks. |
| 40 | m220807_082227_add_course_id_column_to_course_material_table.php | Добавляет FK course_id в таблицу course_material. |
| 41 | m220808_023949_add_estimated_at_column_to_topic_lesson_table.php | Добавляет колонку времени оценки в topic_lesson. |
| 42 | m220808_040454_create_user_contract_table.php | Создаёт таблицу договоров пользователей. |
| 43 | m220808_073614_add_estimated_at_to_course_material_table.php | Добавляет поле estimated_at в course_material. |
| 44 | m220808_080101_add_gallery_column_to_course_material_table.php | Добавляет колонку галереи к материалу курса. |
| 45 | m220809_071059_add_sorting_column_to_colloquium_gallery_table.php | Добавляет поле сортировки в таблицу галереи коллоквиума. |
| 46 | m220811_041437_add_options_columns_to_home_task_table.php | Добавляет колонку options к домашнему заданию (варианты ответов). |
| 47 | m220809_112453_alter_phone_column_to_user_table.php | Изменяет тип/размер колонки phone в users. |
| 48 | m220811_085838_alter_name_probe_file_path_column_on_course_material_table.php | Переименовывает колонку probe_file_path в course_material. |
| 49 | m220811_090801_create_probe_tasks_table.php | Создаёт таблицу задач «проба». |
| 50 | m220815_142159_add_cildren_profile_info_to_user_profile_table.php | Добавляет колонку children_profile_info в профиль пользователя. |
| 51 | m220817_064236_add_payment_amount_column_to_direction_orders_table.php | Добавляет поле суммы платежа в заказ направления. |
| 52 | m220818_040812_create_direction_group_table.php | Создаёт таблицу групп направлений. |
| 53 | m220818_064250_add_comment_column_to_direction_orders_table.php | Добавляет колонку комментария к заказу направления. |
| 54 | m220818_041844_create_children_groups_table.php | Создаёт таблицу дочерних групп. |
| 55 | m220818_022848_add_type_and_old_price_columns_to_direction_tariffs_table.php | Добавляет тип и старую цену в тарифы направления. |
| 56 | m220818_022721_add_old_price_column_to_direction_table.php | Добавляет колонку old_price к direction. |
| 57 | m220822_120007_create_material_plan_table.php | Создаёт таблицу планов материалов. |
| 58 | m220822_121105_create_lesson_plan_table.php | Создаёт таблицу планов уроков. |
| 59 | m220826_045651_creaet_student_private_table.php | Создаёт таблицу личных данных студента (ошибка в имени). |
| 60 | m220826_040436_create_home_work_result_table.php | Создаёт таблицу результатов домашнего задания. |
| 61 | m220822_102040_add_direction_id_column_to_course_plan_table.php | Добавляет FK direction_id в план курса. |
| 62 | m220822_101538_create_course_plan_table.php | Создаёт таблицу плана курса. |
| 63 | m220822_073156_create_lesson_task_answers_table.php | Создаёт таблицу ответов на задачи урока. |
| 64 | m220822_044905_change_relation_children_group_on_timetable_table.php | Меняет связь child‑group в расписании. |
| 65 | m220822_024010_alter_file_path_name_column_in_additional_probe_table.php | Изменяет тип/имя колонки file_path в additional_probe. |
| 66 | m220822_022409_create_additional_probe_table.php | Создаёт таблицу дополнительных «проб». |
| 67 | m220819_061958_create_timetable_table.php | Создаёт таблицу расписания (timetable). |
| 68 | m220819_061250_add_status_and_paid_up_to_columns_to_children_groups_table.php | Добавляет статус и дату оплаты в дочерние группы. |
| 69 | m220818_064546_add_group_id_column_to_direction_orders_table.php | Добавляет FK group_id к заказу направления. |
| 70 | m220817_074952_add_learning_class_column_to_user_profile_table.php | Добавляет колонку learning_class в профиль пользователя. |
| 71 | m220826_050540_add_status_column_to_student_private_table.php | Добавляет статус в таблицу student_private. |
| 72 | m220817_073242_alter_learning_class_type_column.php | Изменяет тип колонки learning_class. |
| 73 | m220822_030444_alter_created_at_and_updated_at_type_in_additional_probe_table.php | Переносит типы timestamps в additional_probe. |
| 74 | m220831_121459_alter_correct_answer_column_hom… | Исправляет тип/размер колонки correct_answer в home_task. |
| 75 | m220831_045940_create_home_work_result_table.php | Создаёт таблицу результатов домашней работы (повтор). |
| 76 | m220830_102103_add_direction_id_and_course_id_column_to_home_work_table.php | Добавляет FK direction_id и course_id в home_work. |
| 77 | m220830_094308_change_home_work_result_table.php | Изменяет структуру таблицы результатов домашней работы. |
| 78 | m220828_053011_add_social_columns_to_user_profile_table.php | Добавляет соцсети в профиль пользователя. |
| 79 | m220827_172955_modify_column_gpa_in_direction_tariff_table.php | Изменяет колонку gpa в тарифах направления. |
| 80 | m220826_055703_add_direction_id_column_to_student_private_table.php | Добавляет FK direction_id к student_private. |
| 81 | m220817_064027_alter_buy_month_column_name.php | Переименовывает колонку buy_month. |
| 82 | m220826_054459_rename_columns_student_id_in_plan_tables.php | Переименовывает student_id в таблицах планов. |
| 83 | m220817_063734_add_document_column_to_direction_orders_table.php | Добавляет колонку документа к заказу направления. |
| 84 | m220905_100604_add_comment_and_score_columns_to_home_work_result_table.php | Добавляет колонки comment и score в результаты домашней работы. |
| 85 | m220817_063208_add_teacher_id_column_to_direction_orders_table.php | Добавляет FK teacher_id к заказу направления. |
| 86 | m220906_044155_create_probe_result_table.php | Создаёт таблицу результатов «проба». |
| 87 | m220906_050024_change_fk_probe_result_table.php | Меняет внешний ключ в probe_result. |
| 88 | m220906_045439_add_group_id_column_to_probe_result_table.php | Добавляет FK group_id к результату пробы. |
| 89 | m220906_040116_add_verified_date_column_to_home_work_table.php | Добавляет дату проверки в home_work. |
| 90 | m220905_113153_add_prompt_and_video_solution_columns_… | Добавляет колонки prompt, video_solution к результату домашней работы. |
| 91 | m220905_102641_add_group_id_column_to_home_work_table.php | Добавляет FK group_id в home_work. |
| 92 | m220817_062433_change_structure_course_order_table.php | Перестраивает таблицу order_course. |
| 93 | m220906_052322_add_work_id_column_to_probe_result_table.php | Добавляет FK work_id к результату пробы. |
| 94 | m220906_051634_add_additional_columns_to_material_plan_table.php | Добавляет дополнительные поля в material_plan. |
| 95 | m220817_053649_moved_avatar_path_column_from_user_profile_to_user_tabel.php | Переносит поле avatar_path из профиля в users (помилка в имени). |
| 96 | m220817_043912_add_avatar_path_to_user_profile_table.php | Добавляет колонку avatar_path к профилю. |
| 97 | m220908_053047_change_fk_for_probe_result_table.php | Меняет внешний ключ в probe_result. |
| 98 | m220908_062507_change_structure_material_plan_table.php | Перестраивает структуру material_plan. |
| 99 | m220908_054824_add_score_and_correct_answer_and_comment_to_probe_result_table.php | Добавляет поля score, correct_answer, comment к probe_result. |
| 100 | m220908_052714_add_probe_files_column_to_probe_work_table.php | Добавляет колонку файлов в таблицу probe_work. |
| 101 | m220808_080101_add_gallery_column_to_course_material_table.php | Добавляет колонку gallery в таблицу материалов курса. |
| 102 | m220808_073614_add_estimated_at_to_course_material_table.php | Добавляет поле estimated_at – время оценки материала. |
| 103 | m220808_040454_create_user_contract_table.php | Создаёт таблицу договоров пользователей (user_contracts). |
| 104 | m220808_023949_add_estimated_at_column_to_topic_lesson_table.php | Добавляет поле estimated_at в таблицу уроков темы. |
| 105 | m220808_040454_create_user_contract_table.php | Создаёт таблицу договоров пользователей (user_contracts). |
| 106 | m220808_073010_change_colloquium_structure.php | Изменяет структуру таблицы коллоквиумов. |
| 107 | m220808_073614_add_estimated_at_to_course_material_table.php | Добавляет поле estimated_at в таблицу материалов курса. |
| 108 | m220808_080101_add_gallery_column_to_course_material_table.php | Добавляет колонку gallery (массив изображений) в таблицу материалов курса. |
| 109 | m220808_085322_create_colloquium_gallery_table.php | Создаёт таблицу colloquium_gallery. |
| 110 | m220808_110635_create_home_task_files_table.php | Создаёт таблицу файлов домашнего задания. |
| 111 | m220809_035504_change_structure_home_task_table.php | Изменяет структуру таблицы домашних заданий. |
| 112 | m220809_035952_alter_exam_weight_column_to_home_task_table.php | Модифицирует колонку exam_weight в таблице домашних заданий. |
| 113 | m220809_071059_add_sorting_column_to_colloquium_gallery_table.php | Добавляет поле сортировки в таблицу коллоквиум‑галереи. |
| 114 | m220809_112453_alter_phone_column_touser_table.php | Изменяет колонку phone в таблице пользователей. |
| 115 | m220811_041437_add_options_columns_to_home_task_table.php | Добавляет колонки options к домашнему заданию. |
| 116 | m220811_085135_add_probe_file_path_column_to_course_material_table.php | Добавляет колонку пути файла проба в таблицу материалов курса. |
| 117 | m220811_085838_alter_name... | Изменяет название колонки probe_file_path в таблице материалов курса. |
| 118 | m220811_090801_create_probe_tasks_table.php | Создаёт таблицу probe_tasks. |
| 119 | m220811_094906_add_column_sorting_to_probe_tasks.php | Добавляет поле сортировки в таблицу задач проба. |
| 120 | m220811_101400_add_sorting... | Добавляет поле сортировки к заданиям уроков. |
| 121 | m220812_072448_add_sorting_column_to_control_tasks_table.php | Добавляет колонку сортировки в таблицу контрольных заданий. |
| 122 | m220815_090715_add_seminarion_id... | Добавляет поле seminarion_id к таблице направлений. |
| 123 | m220815_091839_add_permission... | Добавляет права доступа permission в таблицу направлений. |
| 124 | m220815_092350... | Добавляет колонку direction_id в таблицу задач‑топика. |
| 125 | m220815_092711_add_direction_id_column_to_task_exam_number_table.php | Добавляет колонку direction_id к таблице экзаменационных вопросов. |
| 126 | m220816_091525_add_direction_id_column_to_task_subtopic_table.php | Добавляет поле direction_id в таблицу под‑топиков. |
| 127 | m220817_041851_create_course_order_table.php | Создаёт таблицу заказов курсов (course_orders). |
| 128 | m220817_043912_add_avatar_path_to_user_profile_table.php | Добавляет колонку avatar_path в профиль пользователя. |
| 129 | m220817_053649_moved_avatar_path_column_from_user_profile_to_user_tabel.php | Переносит колонку avatar_path из профиля пользователя в таблицу user. |
| 130 | m220817_062433_change_structure_course_order_table.php | Изменяет структуру таблицы заказов курсов. |
| 131 | m220817_063208_add_teacher_id_column_to_direction_orders_table.php | Добавляет колонку teacher_id в таблицу заказов направлений. |
| 132 | m220817_063734_add_document_column_to_direction_orders_table.php | Добавляет колонку document. |
| 133 | m220817_064027_alter_buy_month_column_name.php | Переименовывает колонку buy_month. |
| 134 | m220817_064236_add_payment_amount_column_to_direction_orders_table.php | Добавляет поле payment_amount в таблицу заказов. |
| 135 | m220817_073242_alter_learning_class_type_column.php | Изменяет колонку learning_class_type. |
| 136 | m220817_074952_add_learning_class_column_to_user_profile_table.php | Добавляет поле learning_class в профиль пользователя. |
| 137 | m220818_022721_add_old_price_column_to_direction_table.php | Добавляет колонку old_price в таблицу направлений. |
| 138 | m220818_022848_add_type_and_old_price_columns_to_direction_tariffs_table.php | Добавляет колонки type, old_price. |
| 139 | m220818_040812_create_direction_group_table.php | Создаёт таблицу групп направлений. |
| 140 | m220818_041844_create_children_groups_table.php | Создаёт таблицу дочерних групп. |
| 141 | m220818_064250_add_comment_column_to_direction_orders_table.php | Добавляет колонку comment. |
| 142 | m220818_064546_add_group_id_column_to_direction_orders_table.php | Добавляет поле group_id в таблицу заказов. |
| 143 | m220819_061250_add_status_and_paid_up_to_columns_to_children_groups_table.php | Добавляет колонки status, paid_up_to. |
| 144 | m220819_061958_create_timetable_table.php | Создаёт таблицу расписания (timetable). |
| 145 | m220822_022409_create_additional_probe_table.php | Создаёт таблицу дополнительного проба. |
| 146 | m220822_024010_alter_file_path_name_column_in_additional_probe_table.php | Изменяет колонку file_path_name. |
| 147 | m220822_030444_alter_created_at_and_updated_at_type_in_additional_probe_table.php | Переопределяет типы полей created_at, updated_at. |
| 148 | m220822_044905_change_relation_children_group_on_timetable_table.php | Меняет связь детей‑группы в расписании. |
| 149 | m220822_073156_create_lesson_task_answers_table.php | Создаёт таблицу ответов к уроковым задачам. |
| 150 | m220822_101538_create_course_plan_table.php | Создаёт таблицу планов курса (course_plan). |
| 151 | m220822_102040_add_direction_id_column_to_course_plan_table.php | Добавляет поле direction_id. |
| 152 | m220822_120007_create_material_plan_table.php | Создаёт таблицу плана материалов. |
| 153 | m220822_121105_create_lesson_plan_table.php | Создаёт таблицу планов уроков. |
| 154 | m220826_040436_create_home_work_result_table.php | Создаёт таблицу результатов домашнего задания (home_work_result). |
| 155 | m220826_045651_creaet_student_private_table.php | Создаёт таблицу приватных данных студента. |
| 156 | m220826_050540_add_status_column_to_student_private_table.php | Добавляет поле status. |
| 157 | m220826_054459_rename_columns_student_id_in_plan_tables.php | Переименовывает колонку student_id в таблицах плана. |
| 158 | m220826_055703_add_direction_id_column_to_student_private_table.php | Добавляет поле direction_id. |
| 159 | m220827_172955_modify_column_gpa_in_direction_tariff_table.php | Изменяет колонку gpa в таблице тарифов. |
| 160 | m220828_053011_add_social_columns_to_user_profile_table.php | Добавляет колонки соцсетей к профилю пользователя. |
| 161 | m220830_094308_change_home_work_result_table.php | Изменяет структуру таблицы результатов домашнего задания. |
| 162 | m220830_102103_add_direction_id_and_course_id_column_to_home_work_table.php | Добавляет поля direction_id, course_id в таблицу домашних заданий. |
| 163 | m220831_045940_create_home_work_result_table.php | Создаёт таблицу результатов домашнего задания. |
| 164 | m220831_121459_alter_correct_answer_column_in_home_task_table.php | Изменяет тип/размер колонки correct_answer в таблице home_task. |
| 165 | m220905_100604_add_comment_and_score_columns_to_home_work_result_table.php | Добавляет колонки comment и score к таблице результатов домашнего задания. |
| 166 | m220905_102641_add_group_id_column_to_home_work_table.php | Добавляет колонку group_id в таблицу домашних заданий. |
| 167 | m220905_113153_add_prompt_and_video_solution_columns_to_home_work_result_table.php | Добавляет колонки prompt и video_solution к результатам домашнего задания. |
| 168 | m220906_040116_add_verified_date_column_to_home_work_table.php | Добавляет колонку verified_date в таблицу домашних заданий. |
| 169 | m220906_044155_create_probe_result_table.php | Создаёт таблицу результатов проверки (probe). |
| 170 | m220906_045439_add_group_id_column_to_probe_result_table.php | Добавляет колонку group_id в таблицу probe_result. |
| 171 | m220906_050024_change_fk_probe_result_table.php | Изменяет внешний ключ в таблице probe_result. |
| 172 | m220906_051634_add_additional_columns_to_material_plan_table.php | Добавляет дополнительные колонки в таблицу material_plan. |
| 173 | m220906_052322_add_work_id_column_to_probe_result_table.php | Добавляет колонку work_id в таблицу probe_result. |
| 174 | m220906_054459_add_time_left_column_to_material_plan_table.php | Добавляет колонку time_left в материалплан. |
| 175 | m220907_025535_alter_left_time_in_material_plan_table.php | Изменяет поле left_time в таблице material_plan. |
| 176 | m220907_050926_add_start_date_column_to_material_plan_table.php | Добавляет колонку start_date в материалплан. |
| 177 | m220907_101655_add_deadline_date_to_home_work_table.php | Добавляет дату deadline к домашнему заданию. |
| 178 | m220907_103933_add_correct_answer_column_to_home_work_result_table.php | Добавляет колонку correct_answer в таблицу результатов домашнего задания. |
| 179 | m220907_105637_alter_expired_date_column_on_home_task_table.php | Изменяет поле expired_date в таблице home_task. |
| 180 | m220908_051029_create_probe_work_table.php | Создаёт таблицу probe_work (рабочие проверки). |
| 181 | m220908_052714_add_probe_files_column_to_probe_work_table.php | Добавляет колонку probe_files в таблицу probe_work. |
| 182 | m220908_053047_change_fk_for_probe_result_table.php | Меняет внешний ключ в таблице probe_result. |
| 183 | m220908_054824_add_score_and_correct_answer_and_comment_to_probe_result_table.php | Добавляет колонки score, correct_answer и comment к probe_result. |
| 184 | m220908_062507_change_structure_material_plan_table.php | Изменяет структуру таблицы material_plan. |
| 185 | m220909_025017_add_update_date_column_on_probe_work_table.php | Добавляет колонку update_date в таблицу probe_work. |
| 186 | m220909_062001_add_verified_date_to_probe_work_table.php | Добавляет колонку verified_date в probe_work. |
| 187 | m220912_054225_add_decided_right_column_to_probe_work_table.php | Добавляет колонку decided_right к таблице probe_work. |
| 188 | m220912_062230_create_control_work_table.php | Создаёт таблицу control_work (контрольные задания). |
| 189 | m220912_093630_add_decided_right_column_to_probe_result_table.php | Добавляет колонку decided_right в probe_result. |
| 190 | m220915_061024_add_comment_files_column_to_probe_result_table.php | Добавляет колонки comment_files к таблице probe_result. |
| 191 | m220915_080210_add_comment_column_to_home_work_table.php | Добавляет колонку comment в домашнее задание. |
| 192 | m220915_080219_add_comment_column_to_probe_result_table.php | Добавляет колонку comment к probe_result. |
| 193 | m220919_015924_create_recording_lesson_table.php | Создаёт таблицу recording_lesson (записи уроков). |
| 194 | m220919_021343_add_fk_for_topic_id_column_to_recording_lesson_table.php | Добавляет внешний ключ topic_id в таблицу recording_lesson. |
| 195 | m220919_083313_add_pause_date_column_to_control_work_table.php | Добавляет колонку pause_date к control_work. |
| 196 | m220919_085256_create_control_work_result_table.php | Создаёт таблицу результатов контрольных заданий. |
| 197 | m220920_020533_add_answer_type_column_to_control_work_result_table.php | Добавляет колонку answer_type в таблицу control_work_result. |
| 198 | m220920_030353_change_relation_control_task_on_control_work_result_table.php | Изменяет связь между control_task и control_work_result. |
| 199 | m220920_040521_add_comment_verified_date_columns_to_control_work_table.php | Добавляет колонки comment и verified_date к таблице control_work. |
| 200 | m220920_080314_alter_table_comment_column_on_home_work_result_table.php | Изменяет тип/описание столбца comment в таблице home_work_result. |
| 201 | m220920_113918_add_correct_answer_column_to_control_work_result_table.php | Добавляет колонку correct_answer к таблице control_work_result (сохраняет правильный ответ). |
| 202 | m220922_032702_create_home_work_unique_tasks_table.php | Создаёт новую таблицу home_work_unique_tasks, которая хранит уникальные задания для домашней работы. |
| 203 | m220922_040406_add_home_work_type_column_to_home_work_table.php | Добавляет колонку home_work_type в таблицу home_work (тип домашней работы). |
| 204 | m220923_114622_change_structure_timetable_table.php | Перестраивает структуру таблицы timetable (изменены типы, названия столбцов и их порядок). |
| 205 | m220923_123030_alter_column_teacher_id_on_timetable_table.php | Модифицирует колонку teacher_id в таблице timetable (обновлён тип/связь). |
| 206 | m220925_072726_rename_lesson_type_column_on_timetable_table.php | Переименовывает столбец lesson_type на таблице timetable. |
| 207 | m220925_073838_alter_type_for_created_at_and_updated_at_columns_on_timetable_table.php | Изменяет типы полей created_at и updated_at в таблице timetable (на datetime/nullable). |
| 208 | m220927_074642_add_repeat_day_column_to_timetable_table.php | Добавляет колонку repeat_day к таблице timetable (день повторения расписания). |
| 209 | m220928_044608_add_cancel_type_column_to_timetable_table.php | Вставляет поле cancel_type в таблицу timetable (тип отмены события). |
| 210 | m220928_052238_create_lesson_abstract_table.php | Создаёт новую таблицу lesson_abstract, где хранятся абстрактные данные о уроках. |
| 211 | m220928_055149_create_lesson_abstract_files_table.php | Создаёт таблицу lesson_abstract_files для хранения файлов, связанных с абстракцией урока. |
| 212 | m220928_075102_create_timetable_report_table.php | Добавляет таблицу timetable_report, предназначенную для отчётов по расписанию. |
| 213 | m220929_044706_change_timetable_structure.php | Перестраивает структуру таблицы timetable (переименовывает/удаляет столбцы). |
| 214 | m220929_081823_add_is_cancel_column_to_timetable_table.php | Добавляет булевый флаг is_cancel в таблицу timetable (указывает, отменено ли событие). |
| 215 | m221001_062156_add_sorting_column_to_direction_table.php | Вставляет колонку sorting в таблицу direction для сортировки направлений. |
| 216 | m221001_062620_add_sorting_column_to_direction_course_table.php | Добавляет поле sorting к таблице direction_course (порядок курсов). |
| 217 | m221001_062643_add_sorting_column_to_direction_filter_table.php | Вставляет колонку sorting в таблицу direction_filter. |
| 218 | m221003_030406_add_plan_id_column_to_lesson_abstract_table.php | Добавляет колонку plan_id в таблицу lesson_abstract (ссылка на план). |
| 219 | m221003_033404_drop_lesson_id_column_on_lesson_abstract_table.php | Удаляет столбец lesson_id из таблицы lesson_abstract. |
| 220 | m221003_042753_add_verified_date_column_to_lesson_abstract_table.php | Добавляет колонку verified_date в таблицу lesson_abstract (дата проверки). |
| 221 | m221003_074417_create_journal_timetable_table.php | Создаёт таблицу journal_timetable, где фиксируются записи о проведённых уроках. |
| 222 | m221006_050600_create_timetable_members_table.php | Создаёт таблицу timetable_members (участники расписания). |
| 223 | m221006_054052_add_name_column_to_timetable_table.php | Добавляет колонку name в таблицу timetable (название события). |
| 224 | m221010_024021_change_structure_journal_timetable_table.php | Перестраивает структуру таблицы journal_timetable (изменены типы/поля). |
| 225 | m221010_071245_add_comment_column_to_journal_timetable_table.php | Добавляет колонку comment в таблицу journal_timetable. |
| 226 | m221011_030725_create_seminar_presentation_table.php | Создаёт таблицу seminar_presentation (презентации семинаров). |
| 227 | m221013_031151_create_report_parent_table.php | Создаёт таблицу report_parent, где хранятся родительские отчёты. |
| 228 | m221016_080000_create_manager_table.php | Создаёт таблицу manager (данные менеджеров). |
| 229 | m221016_080010_create_lead_table.php | Создаёт таблицу lead (контакты потенциальных клиентов). |
| 230 | m221016_080020_create_lead_meta_table.php | Добавляет таблицу lead_meta для метаданных лидов. |
| 231 | m221016_080030_create_head_of_sales_table.php | Создаёт таблицу head_of_sales (руководители отдела продаж). |
| 232 | m221016_080040_create_head_of_sales_meta_table.php | Добавляет таблицу head_of_sales_meta с дополнительными данными. |
| 233 | m221016_080050_create_status_table.php | Создаёт таблицу status (статусы сущностей). |
| 234 | m221016_080100_create_source_table.php | Создаёт таблицу source (источники данных). |
| 235 | m221016_080110_create_lead_meta_type_table.php | Добавляет таблицу lead_meta_type – типы метаданных для лидов. |
| 236 | m221016_080120_create_traffic_finances_table.php | Создаёт таблицу traffic_finances (финансовые показатели трафика). |
| 237 | m221020_064230_add_direction_id_column_to_timetable_table.php | Добавляет колонку direction_id в таблицу timetable (ссылка на направление). |
| 238 | m221024_054418_create_payment_table.php | Создаёт таблицу payment (платёжные операции). |
| 239 | m221025_030759_create_promo_code_table.php | Создаёт таблицу promo_code для хранения промокодов. |
| 240 | m221025_041313_create_order_payments_table.php | Создаёт таблицу order_payments (платежи по заказам). |
| 241 | m221025_065933_add_external_order_id_column_to_payment_table.php | Добавляет колонку external_order_id в таблицу payment (внешний ID заказа). |
| 242 | m221025_080111_add_additional_columns_to_order_payments_table.php | Вставляет дополнительные поля в таблицу order_payments (например, дата оплаты). |
| 243 | m221107_043841_change_structure_student_private_table.php | Перестраивает таблицу student_private (обновлены типы и порядок столбцов). |
| 244 | m221108_015625_add_buy_price_column_to_direction_orders_table.php | Добавляет колонку buy_price в таблицу direction_orders. |
| 245 | m221108_031902_drop_left_columns_from_student_private_table.php | Удаляет лишние столбцы из таблицы student_private (отключает устаревшие поля). |
| 246 | m221108_085332_alter_type_buy_hours_on_direction_orders_table.php | Изменяет тип колонки buy_hours в таблице direction_orders. |
| 247 | m221108_102150_add_column_order_id_to_lesson_plan_table.php | Добавляет колонку order_id к таблице lesson_plan (ссылка на заказ). |
| 248 | m221110_051144_create_direction_seminarian_table.php | Создаёт таблицу direction_seminarian (семинарники по направлению). |
| 249 | m221111_022248_change_type_buy_hour_column_in_order_payments_table.php | Изменяет тип колонки buy_hour в таблице order_payments. |
| 250 | m221117_035209_add_column_is_filled_column_to_report_parent_table.php | Добавляет булевый флаг is_filled в таблицу report_parent (указывает, заполнен ли отчёт). |

---

## Полезные команды Yii2 (быстро)

```bash
php yii help migrate
php yii migrate/history
php yii migrate/new
php yii migrate/to 0
```

---

## Примечания по качеству списка

- В исходном перечне присутствуют **дубли** (например, некоторые миграции с `m220808_*` перечислены несколько раз).
- Есть строки с обрезанным названием (`...`) — их лучше уточнить по имени файла в репозитории.
- Если хочешь, я могу:
  1) привести список к **уникальному** виду (по имени файла),
  2) сгруппировать по доменам/модулям,
  3) добавить ссылки на файлы и таблицы (если дашь структуру папок/репо).
