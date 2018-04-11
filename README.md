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
rails g devise:views
rake db:migrate
rails s
http://localhost:3000/
```
```
git checkout -b welcome
rails g controller welcome index
---
app/views/welcome/index.html.erb
---
<h1>欢迎来到才华横溢</h1>
<p>这里是改变你命运的地方！</p>

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
    <title>才华横溢</title>
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
          <h1 class="title is-5">才华横溢</h1>
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
                <%= link_to 'Post Job', "#", class: 'navbar-item button is-primary is-rounded' %>
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
        Visit <a href="https://superxschool.com" target="_blank">superxschool.com</a> for more builds like this one.
        </p>
      </div>
    </div>
  </footer>

  </body>
</html>

---
app/views/devise/registrations/new.html.erb
---
<h2>Sign up</h2>

<%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
  <%= f.error_notification %>

  <div class="form-inputs">
    <%= f.input :email, required: true, autofocus: true %>
    <%= f.input :password, required: true, hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length) %>
    <%= f.input :password_confirmation, required: true %>
  </div>

  <div class="form-actions">
    <%= f.button :submit, "Sign up" %>
  </div>
<% end %>

<%= render "devise/shared/links" %>
---
<div class="section">
  <div class="container">
  <div class="columns is-centered">

    <div class="column is-4">

    <h2 class="title is-2">Sign Up</h2>

    <%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
    <%= f.error_notification %>

    <div class="field">
      <div class="control">
      <%= f.input :name, required: true, autofocus: true, input_html: { class:"input" }, wrapper: false, label_html: { class:"label" } %>
      </div>
    </div>

    <div class="field">
      <div class="control">
      <%= f.input :email, required: true, input_html: { class:"input" }, wrapper: false, label_html: { class:"label" } %>
      </div>
    </div>

    <div class="field">
      <div class="control">
        <%= f.input :password, required: true, input_html: { class:"input" }, wrapper: false, label_html: { class:"label" }, hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length) %>
      </div>
    </div>

    <div class="field">
      <div class="control">
        <%= f.input :password_confirmation, required: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" } %>
      </div>
    </div>

    <div class="field">
      <div class="control">
        <%= f.button :submit, "Sign up", class:"button is-primary is-rounded" %>
      </div>
    </div>

    <% end %>
      <br />
      <%= render "devise/shared/links" %>
    </div>
    </div>
  </div>
</div>
---
app/views/devise/registrations/edit.html.erb
---
<h2>Edit <%= resource_name.to_s.humanize %></h2>

<%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name), html: { method: :put }) do |f| %>
  <%= f.error_notification %>

  <div class="form-inputs">
    <%= f.input :email, required: true, autofocus: true %>

    <% if devise_mapping.confirmable? && resource.pending_reconfirmation? %>
      <p>Currently waiting confirmation for: <%= resource.unconfirmed_email %></p>
    <% end %>

    <%= f.input :password, autocomplete: "off", hint: "leave it blank if you don't want to change it", required: false %>
    <%= f.input :password_confirmation, required: false %>
    <%= f.input :current_password, hint: "we need your current password to confirm your changes", required: true %>
  </div>

  <div class="form-actions">
    <%= f.button :submit, "Update" %>
  </div>
<% end %>

<h3>Cancel my account</h3>

<p>Unhappy? <%= link_to "Cancel my account", registration_path(resource_name), data: { confirm: "Are you sure?" }, method: :delete %></p>

<%= link_to "Back", :back %>
---
app/views/devise/sessions/new.html.erb
---
<h2>Log in</h2>

<%= simple_form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
  <div class="form-inputs">
    <%= f.input :email, required: false, autofocus: true %>
    <%= f.input :password, required: false %>
    <%= f.input :remember_me, as: :boolean if devise_mapping.rememberable? %>
  </div>

  <div class="form-actions">
    <%= f.button :submit, "Log in" %>
  </div>
<% end %>

<%= render "devise/shared/links" %>

---
<section class="section">
	<div class="container">
		<div class="columns is-centered">
			<div class="column is-4">
				<h2 class="title is-2">Log in</h2>
				<%= simple_form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>

				  <div class="field">
				  	<div class="control">
				    	<%= f.input :email, required: false, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
				    </div>
				  </div>

				  <div class="field">
				  	<div class="control">
				    <%= f.input :password, required: false, input_html: { class: "input"}, wrapper: false, label_html: { class: "label" } %>
						</div>
					</div>

					<div class="field">
				  	<div class="control">
				    <%= f.input :remember_me, wrapper: false, as: :boolean if devise_mapping.rememberable? %>
				  	</div>
					</div>

				  <%= f.button :submit, "Log in", class:"button is-rounded is-primary" %>
				<% end %>
				<br/>
				<%= render "devise/shared/links" %>
			</div>
		</div>
	</div>
</section>
```
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fq8s7uwiusj31kw0g6jtu.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fq8s7q73t2j31kw0niwhf.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fq8s7k28kuj31kw0ohtbl.jpg)
