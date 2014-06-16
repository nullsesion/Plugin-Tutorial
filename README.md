Plugin-Tutorial
===============
перевод [руководства](http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial) по созданию плагинов для Redmine

##руководство по созданию плагинов Redmine

данное руководство расчитано на Redmine 2.x. Руководство по   созданию плагинов для Redmine 1.x
[здесь](http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial?version=66). Данное руководство предпологает что вы знакомы Ruby on Rails framework.

###Создание новых плагинов
Рекомендуется установить переменную окружения RAILS_ENV используя комманду ниже
<pre>
$ export RAILS_ENV="production"
</pre>
в Windows
<pre>
$ export RAILS_ENV=production
</pre>
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

###Генерация модели

Сейчас плагин не может сохронять никакую информацию. Давайте создадим простую модель опроса(Poll) для нашего плагина.
Для создания модели используйте следующий синтаксис:
<pre>
ruby script/rails generate redmine_plugin_model &lt;plugin_name&gt &lt;model_name&gt; [field[:type][:index] field[:type][:index] ...]
</pre>
выполним команду в командной строке
<pre>
$ ruby script/rails generate redmine_plugin_model polls poll question:string yes:integer no:integer
      create  plugins/polls/app/models/poll.rb
      create  plugins/polls/test/unit/poll_test.rb
      create  plugins/polls/db/migrate/001_create_polls.rb
</pre>
Это действие создаст модель Poll и соответствующий файл миграции  file 001_create_polls.rb в plugins/polls/db/migrate:
<pre>
class CreatePolls &lt; ActiveRecord::Migration
  def change
    create_table :polls do |t|
      t.string :question
      t.integer :yes, :default => 0
      t.integer :no, :default => 0
    end
  end
end
</pre>
Вы можете настроить ваш файл миграции (те. значения по умолчанию...) затем перенести базу данных с помощью следующей команды(прогнать миграцию):
<pre>
$ rake redmine:plugins:migrate

Migrating polls (Polls plugin)...
==  CreatePolls: migrating ====================================================
-- create_table(:polls)
   -> 0.0410s
==  CreatePolls: migrated (0.0420s) ===========================================
</pre>
Обратите внимание, что каждый модуль имеет свой собственный набор миграций.

Давайте добавим несколько опросов в консоли, так что нам было с чем работать. Консоль, где вы можете в интерактивном режиме работы и изучить Redmine среды и является очень информативной, чтобы поиграть. Но сейчас нам просто необходимо создать два Poll объекта
<pre>
ruby script/rails console
&#91;rails 3&#93; rails console
>> Poll.create(:question => "Can you see this poll")
>> Poll.create(:question => "And can you see this other poll")
>> exit
</pre>
Отредактируйте plugins/polls/app/models/poll.rb в вашей директории плагинов и добавте метод vote который будет вызыватся из нашего контролера:
<pre>
class Poll &lt; ActiveRecord::Base
  def vote(answer)
    increment(answer == 'yes' ? :yes : :no)
  end
end
</pre>