# My Rails Tutorial

![Rails 7 Blog](rails7blog.png)

This project is to teach myself Ruby on Rails. I am using Ubuntu in WSL2 (Windows Subsystem for Linux 2) and Ruby 2.7. This project requires Ruby and SQLite3. I will be using [this](https://guides.rubyonrails.org/getting_started.html) guide to get started.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Add a route](#add-a-route)
- [Autoloading](#autoloading)
- [Generate a model](#generate-a-model)
- [List all of our articles](#list-all-of-our-articles)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

Install gpg2

```bash
sudo apt install gnupg2 -y
curl -sSL https://rvm.io/mpapis.asc | gpg2 
```

Import keys and install RVM (Ruby Version Manager)

```bash
--import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
\curl -sSL https://get.rvm.io | bash -s stable
source /home/loftwah/.rvm/scripts/rvm
rvm install 2.7 # Rails 7 requires Ruby 2.7
```

Check that Ruby is installed.

```bash
ruby -v        
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-linux]
```

Install SQLite3

```bash
sudo apt install sqlite3 -y
sqlite3 --version
3.31.1 2020-01-27 19:55:54 3bfa9cc97da10598521b342961df8f5f68c7388fa117345eeb516eaa837balt1
```

Jnstall Rails

```bash
gem install rails
rails --version
Rails 7.0.3.1
```

We can now create a new Blog application as per the guide we are following.

```bash
rails new blog
```

> *Note:* `rails new --help` will show you all the options available.

The following directories will be created and this is the explanation of what they are and what they do.

| File/Folder | Purpose |
| ------------ | ------- |
| app/ | Contains the controllers, models, views, helpers, mailers, channels, jobs, and assets for your application. You'll focus on this folder for the remainder of this guide. |
| bin/ | Contains the `rails` script that starts your app and can contain other scripts you use to set up, update, deploy, or run your application. | 
| config/ | Contains configuration for your application's routes, database, and more. This is covered in more detail in [Configuring Rails Applications](https://guides.rubyonrails.org/configuring.html). |
| config.ru | Rack configuration for Rack-based servers used to start the application. For more information about Rack, see the [Rack website](https://rack.github.io/). |
| db/ | Contains your current database schema, as well as the database migrations. |
| Dockerfile | Contains the Dockerfile and container requirements for your application. |
| docker-compose.yml | Contains instructions on how to run your application in a Docker container using Docker Compose. |
| Gemfile & Gemfile.lock | These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the [Bundler website](https://bundler.io). |
| lib/ | Extended modules for your application. |
| log/ | Application log files. |
| public/ | Contains static files and compiled assets. When your app is running, this directory will be exposed as-is. |
| Rakefile | This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing `Rakefile`, you should add your own tasks by adding files to the `lib/tasks` directory of your application. |
| README.md | This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on. |
| storage/ | Active Storage files for Disk Service. This is covered in [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html). |
| test/ | Unit tests, fixtures, and other test apparatus. These are covered in [Testing Rails Applications](https://guides.rubyonrails.org/testing.html). |
| tmp/ | Temporary files (like cache and pid files). |
| vendor/ | A place for all third-party code. In a typical Rails application this includes vendored gems. |
| .gitattributes | This file defines metadata for specific paths in a git repository. This metadata can be used by git and other tools to enhance their behavior. See the [gitattributes documentation](https://git-scm.com/docs/gitattributes) for more information. |
| .gitignore | This file tells git which files (or patterns) it should ignore. See [GitHub - Ignoring files](https://help.github.com/articles/ignoring-files) for more information about ignoring files. |
| .ruby-version | This file contains the default Ruby version. |

## Running the Application

To start the web server, run the following command.

```bash
bin/rails server
```

This starts the Puma web server on port 3000. Navigate to [localhost:3000](http://localhost:3000) to see the application.

## Add a route

Add a route to the `config/routes.rb` file.

```ruby
Rails.application.routes.draw do
  get "/articles", to: "articles#index"

  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

We can then create the Articles controller, so open a new terminal and enter the following.

```bash
bin/rails generate controller Articles index --skip-routes
```

Rails will create a few files for us. Next we want to open `app/views/articles/index.html.erb` and add the following.

```erb
<h1>Hello, Rails!</h1>
<p>This is the index page for the Articles controller.</p>
```

Now we want to set it to be the primary page for our Rails server.

Open `config/routes.rb` and add the following.

```ruby
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
end
```

## Autoloading

Require is used to load files when they are needed.

You only need `require` calls for two use cases:

- To load files under the lib directory.
- To load gem dependencies that have require: false in the Gemfile.

## Generate a model

```bash
bin/rails generate model Article title:string body:text
```

This creates a bunch of files. The two files we'll focus on are the migration file (`db/migrate/<timestamp>_create_articles.rb`) and the model file (`app/models/article.rb`).

The migration can be run with the following command.

```bash
bin/rails db:migrate
```

That will populate `db/development.sqlite3`

Now we can interact with the table using our model. Launch the console with:

```bash
bin/rails console
```

Initialize the model with the following.

```ruby
article = Article.new(title: "Hello Loftwah", body: "Hi, my name is Loftwah and I am on Rails!")
```

This only initializes the model. We need to save it to the database with the following.

```ruby
article.save
```

We can check that the article exists with the following:

```ruby
Article.find(1)
```

Or, if we wanted to check all of the articles we could use the following:

```ruby
Article.all
```

## List all of our articles