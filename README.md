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

ruby script/rails generate redmine_plugin &lt;plugin_name&gt;.

Итак откройте командную строку и перейдите в каталог redmine,
затем выполните следуюшую команду:

<pre>
$ ruby script/rails generate redmine_plugin Polls
      create  plugins/polls/app
      create  plugins/polls/app/controllers
      create  plugins/polls/app/helpers
      create  plugins/polls/app/models
      create  plugins/polls/app/views
      create  plugins/polls/db/migrate
      create  plugins/polls/lib/tasks
      create  plugins/polls/assets/images
      create  plugins/polls/assets/javascripts
      create  plugins/polls/assets/stylesheets
      create  plugins/polls/config/locales
      create  plugins/polls/test
      create  plugins/polls/README.rdoc
      create  plugins/polls/init.rb
      create  plugins/polls/config/routes.rb
      create  plugins/polls/config/locales/en.yml
      create  plugins/polls/test/test_helper.rb
</pre>

Данная команда создала скелет вашего плагина в папке plugins/pools. Отредактируйте файл plugins/pools/init.rb и внесите туда необходимую информацию о плагине (название, Автор, описание, и версия)
<pre>
Redmine::Plugin.register :polls do
  name 'Polls plugin'
  author 'John Smith'
  description 'A plugin for managing polls'
  version '0.0.1'
end
</pre>
Перезапустите приложение и зайдите в браузере на страницу (http://localhost:3000/admin/plugins)
После входа под профилем админа вы сможете увидить новый плагин в списке плагинов
![новый плагин в списке плагинов](http://www.redmine.org/attachments/download/7656/plugins_list1.png)  
Примечание: любые изменения init.rb файл плагина требует перезапуска приложения, поскольку оно не перезагружает init.rb после каждого запроса на каждый запрос.
