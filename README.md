# Let's Build: With Ruby on Rails  - Job Board with Payments
```
mkdir workspace
cd workspace
rails new job_board_pay
cd job_board_pay
git init
git status
git add .
git commit -m "initial commit"
git remote add origin https://github.com/shenzhoudance/job_board_pay.git
git push -u origin master
atom .
rails servwe
http://localhost:3000/
```
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fq8rjie6uqj31ao1261kx.jpg)

```
git checkout -b gem
---
gem 'devise', '~> 4.4.3'
gem 'bulma-rails', '~> 0.6.2'
gem 'simple_form', '~> 3.5.1'
gem 'gravatar_image_tag', github: 'mdeering/gravatar_image_tag'
gem 'sidekiq', '~> 5.0'
gem 'figaro'
gem 'carrierwave', '~> 1.0'
gem 'mini_magick', '~> 4.8'
gem 'stripe'
gem 'trix', '~> 0.11.1'
group :development, :test do
  gem 'better_errors', '~> 2.4'
  gem 'guard', '~> 2.14'
  gem 'guard-livereload', '~> 2.5'
end
```

```
rails generate simple_form:install
rails generate devise:install
rails generate devise User
rake db:migrate
rails s
http://localhost:3000/
```
```
app/views/layouts/application.html.erb
---
<!DOCTYPE html>
<html>
  <head>
    <title>JobBoardPay</title>
    <%= csrf_meta_tags %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
---
<!DOCTYPE html>
<html>
  <head>
    <title><%= Rails.configuration.application_name %></title>
    <%= csrf_meta_tags %>

    <meta name="viewport" content="width=device-width, initial-scale=1">
    <%= stylesheet_link_tag 'https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css' %>
    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', "https://js.stripe.com/v2/", "https://js.stripe.com/v3/", 'data-turbolinks-track': 'reload' %>

    <%= tag :meta, name: "stripe-public-key", content: Figaro.env.stripe_publishable_key %>

  </head>

  <body class="<%= yield (:body_class) %>">
    <% if flash[:notice] %>
      <div class="notification is-success global-notification">
        <p class="notice"><%= notice %></p>
      </div>
    <% end %>

    <% if flash[:alert] %>
    <div class="notification is-danger global-notification">
      <p class="alert"><%= alert %></p>
    </div>
    <% end %>

     <nav class="navbar is-light" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <%= link_to root_path, class:"navbar-item" do %>
          <h1 class="title is-5"><%= Rails.configuration.application_name %></h1>
        <% end  %>
        <div class="navbar-burger burger" data-target="navbar">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>

      <div id="navbar" class="navbar-menu">
        <div class="navbar-end">
          <div class="navbar-item">
            <div class="field is-grouped">
              <p class="control">
                <%= link_to 'Post Job', new_job_path, class: 'navbar-item button is-primary is-rounded' %>
              </p>
              <% if user_signed_in? %>
              <div class="navbar-item has-dropdown is-hoverable">
                <%= link_to 'Account', edit_user_registration_path, class: "navbar-link" %>
                <div class="navbar-dropdown is-right">
                  <%= link_to current_user.name, edit_user_registration_path, class:"navbar-item" %>
                  <%= link_to "Log Out", destroy_user_session_path, method: :delete, class:"navbar-item" %>
                </div>
              </div>
            <% else %>
            <p class="control">
              <%= link_to "Sign In", new_user_session_path, class:"navbar-item button is-rounded" %>
            </p>
            <p class="control">
              <%= link_to "Sign up", new_user_registration_path, class:"navbar-item button is-rounded"%>
            </p>
            <% end %>

          </div>
        </div>
      </div>
    </div>
  </nav>

    <%= yield %>

  <footer class="footer">
    <div class="container">
      <div class="content has-text-centered">
        <p>
        Visit <a href="https://web-crunch.com" target="_blank">Web-Crunch.com</a> for more builds like this one.
        </p>
      </div>
    </div>
  </footer>

  </body>
</html>
---

git checkout -b welcome
rails g controller welcome index
