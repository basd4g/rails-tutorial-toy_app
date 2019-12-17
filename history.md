# rails tutorial history

## Create repository

### rails new

```sh
$ rails _5.1.6_ new toy_app
$ cd toy_app/
$ vim Gemfile
```

```Gemfile
source 'https://rubygems.org'

gem 'rails',        '5.1.6'
gem 'puma',         '3.9.1'
gem 'sass-rails',   '5.0.6'
gem 'uglifier',     '3.2.0'
gem 'coffee-rails', '4.2.2'
gem 'jquery-rails', '4.3.1'
gem 'turbolinks',   '5.0.1'
gem 'jbuilder',     '2.7.0'

group :development, :test do
  gem 'sqlite3', '1.3.13'
  gem 'byebug',  '9.0.6', platform: :mri
end

group :development do
  gem 'web-console',           '3.5.1'
  gem 'listen',                '3.1.5'
  gem 'spring',                '2.0.2'
  gem 'spring-watcher-listen', '2.0.1'
end

group :production do
  gem 'pg', '0.20.0'
end

# Windows環境ではtzinfo-dataというgemを含める必要があります
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

```sh
$ bundle update
$ bundle install --without production
$ rails s
```

### hello,world!

```sh
$ vim app/controllers/application_controller.rb 
$ vim config/routes.rb 
$ rails s
```

```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def hello
    render html: "hello,world!"
  end
end
```

```config/routes.rb
Rails.application.routes.draw do
  resources :users
  root 'application#hello'
end
```

### Create User

```sh
$ rails generate scaffold User name:string email:string
$ rails db:migrate
$ rails s
```

### Fix routing

```diff
diff --git a/config/routes.rb b/config/routes.rb
index 6f37c78..34fca7a 100644
--- a/config/routes.rb
+++ b/config/routes.rb
@@ -1,4 +1,4 @@
 Rails.application.routes.draw do
   resources :users
-  root 'application#hello'
+  root 'users#index'
 end
```

### Create Micropost

```sh
$ rails generate scaffold Micropost content:text user_id:integer
$ rails db:migrate
```

### Restrict content length of Micropost 

```diff
diff --git a/app/models/micropost.rb b/app/models/micropost.rb
index 4f72910..e861145 100644
--- a/app/models/micropost.rb
+++ b/app/models/micropost.rb
@@ -1,2 +1,3 @@
 class Micropost < ApplicationRecord
+  validates :content, length: {maximum: 140}
 end
```
