Plugin-Tutorial
===============
перевод [руководства](http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial) по созданию плагинов для Redmine

##руководство по созданию плагинов Redmine

данное руководство расчитано на Redmine 2.x. Руководство по   созданию плагинов для Redmine 1.x
[здесь](http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial?version=66). Данное руководство предпологает что вы знакомы Ruby on Rails framework.

###Создание новых плагинов
Рекомендуется установить переменную окружения RAILS_ENV используя комманду ниже

$ export RAILS_ENV="production"

в Windows

$ export RAILS_ENV=production

Создать новый плагин вы можете используя генератор плагинов Redmine.
Генератор плагинов Redmine имеет следующий синтаксис


ruby script/rails generate redmine_plugin <plugin_name>

