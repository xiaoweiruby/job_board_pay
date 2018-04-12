# Let's Build: With Ruby on Rails  - Job Board with Payments


![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mmd5ufhj31kw0mvn07.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fq9mmaemu7j31kw0smn0v.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mm41jxuj31kw0pcmze.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mlpfkprj31kw0jxwh0.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fq9mlj5siwj31kw0j7mzb.jpg)

这个案例是一个非常好的商业闭环的网站的设计。

# 在案例的学习过程中，可以完成五个环节的学习；

- （1）功能体系：主要是完成一个招聘广告的发布，其中包含图片的上传；
- （2）用户体系：使用 gem 'devise' 具备普通用户和管理员用户，没有管理员的后台？需要加上管理员的后台吗？
- （3）设计体系：使用 gem 'bulma-rails'完成页面的设计工作，满足页面设计的需要；
- （4）支付体系：使用 gem 'stripe' 完成支付，在这个体系使用的时候，思考 RMVC 的架构，处在 job controller 里面完成 create 广告功能。
- （5）部署体系：使用 herkou 和 aliyun 完成部署，这一部分，没有完成，但是我可以完成教学；

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
bundle install
rails generate simple_form:install
bundle exec guard init
bundle exec guard

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

```
git add .
git commit -m "add welcome index & devise"
git push origin welcome
```
```
git checkout -b cart
---
rails g migration add_card_info_to_users
---
db/migrate/20180411082034_add_card_info_to_users.rb
---
class AddCardInfoToUsers < ActiveRecord::Migration[5.1]
  def change
    add_column :users, :stripe_id, :string
    add_column :users, :card_brand, :string
    add_column :users, :card_last4, :string
    add_column :users, :card_exp_month, :string
    add_column :users, :card_exp_year, :string
    add_column :users, :expires_at, :datetime
  end
end
---
rake db:migrate

```
```
rails g migration add_admin_to_users admin:boolean
rake db:migrate
---
rails c
2.3.1 :001 > user = User.last
2.3.1 :002 > user.admin = true
2.3.1 :003 > user.save
2.3.1 :004 > user
2.3.1 :005 > exit
---
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fq8sxoqqiyj31kw0fodkg.jpg)
```
git checkout -b job-scaffold

rails g scaffold Job title:string description:text url:string job_type:string location:string job_author:string remote_ok:boolean apply_url:string
rake db:migrate

---
app/views/jobs/_form.html.erb
---
<%= simple_form_for(@job) do |f| %>
  <%= f.error_notification %>

  <div class="form-inputs">
    <%= f.input :title %>
    <%= f.input :description %>
    <%= f.input :url %>
    <%= f.input :job_type %>
    <%= f.input :location %>
    <%= f.input :job_author %>
    <%= f.input :remote_ok %>
    <%= f.input :apply_url %>
  </div>

  <div class="form-actions">
    <%= f.button :submit %>
  </div>
<% end %>
---
<%= simple_form_for @job, html: { multipart: true } do |f| %>
  <%= f.error_notification %>

      <div class="field">
        <div class="control">
        <%= f.input :title, required: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" }, placeholder: "Frontend Developer" %>
        </div>
      </div>

      <div class="field">
        <div class="control">
          <%= f.input :apply_url, required: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" }  %>
        </div>
      </div>


    <div class="columns">
      <div class="field column is-4">
        <div class="control">
          <label class="label">Job Type:</label>
          <div class="control has-icons-left">
            <span class="select">
              <%= f.input_field :job_type, collection: Job::JOB_TYPES, prompt: "Select a job type" %>
            </span>
            <span class="icon is-small is-left">
              <i class="fa fa-briefcase"></i>
            </span>
          </div>
        </div>
      </div>

      <div class="column">

        <div class="field">
          <div class="control">
            <%= f.input :location, require: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" }, hint: 'Examples: "San Francisco, CA", "Seattle", "Anywhere"' %>
          </div>
        </div>

        <div class="field">
          <div class="control">
            <label for="job[remote_ok]">
              <%= f.input :remote_ok, required: false, input_html: { class: "checkbox"}, wrapper: false, label: false %>
              Work can be done remotely
            </label><i class="fa fa-wifi"></i>
          </div>
        </div>
      </div>
    </div>

      <div class="field">
        <div class="control">
          <%= f.input :description, required: true, input_html: { class: "textarea" }, wrapper: false, label_html: { class: "label" } %>
        </div>
      </div>


    <div class="columns">

      <div class="field column">
        <div class="control">
          <%= f.input :job_author, required: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" }, label: "Company" %>
        </div>
      </div>

      <div class="field column">
        <div class="control">
          <%= f.input :url, required: true, input_html: { class: "input" }, wrapper: false, label_html: { class: "label" }, label: "Company Website", placeholder: "http://company.com" %>
        </div>
      </div>

    </div>

    <div class="columns">
      <div class="field column is-4">
        <div class="control">
          <label class="label">Logo Image</label>
          <div class="file">
            <label class="file-label">
              <%= f.input :avatar, as: :file, required: false, input_html: { class:"file-input job-avatar" }, label: false, wrapper: false %>
                <span class="file-cta">
                  <span class="file-icon"><i class="fa fa-upload"></i></span>
                  <span class="file-label">Choose a file…</span>
                </span>
            </label>
          </div>
        </div>
      </div>
      <div class="column">
        <output id="list"></output>
      </div>
    </div>

    <hr />

    <%#= render 'payment' %>

    <%= f.button :submit, class: "button is-primary is-large is-rounded mv4" %>

<% end %>

```
```
app/assets/stylesheets/_functions.scss
---
:root {
 --spacing-none: 0;
 --spacing-extra-small: .25rem;
 --spacing-small: .5rem;
 --spacing-medium: 1rem;
 --spacing-large: 2rem;
 --spacing-extra-large: 4rem;
 --spacing-extra-extra-large: 8rem;
 --spacing-extra-extra-extra-large: 16rem;
}

/* Functional Styles: http://tachyons.io/docs/ */
.pa0 { padding: var(--spacing-none); }
.pa1 { padding: var(--spacing-extra-small); }
.pa2 { padding: var(--spacing-small); }
.pa3 { padding: var(--spacing-medium); }
.pa4 { padding: var(--spacing-large); }
.pa5 { padding: var(--spacing-extra-large); }
.pa6 { padding: var(--spacing-extra-extra-large); }
.pa7 { padding: var(--spacing-extra-extra-extra-large); }

.pl0 { padding-left: var(--spacing-none); }
.pl1 { padding-left: var(--spacing-extra-small); }
.pl2 { padding-left: var(--spacing-small); }
.pl3 { padding-left: var(--spacing-medium); }
.pl4 { padding-left: var(--spacing-large); }
.pl5 { padding-left: var(--spacing-extra-large); }
.pl6 { padding-left: var(--spacing-extra-extra-large); }
.pl7 { padding-left: var(--spacing-extra-extra-extra-large); }

.pr0 { padding-right: var(--spacing-none); }
.pr1 { padding-right: var(--spacing-extra-small); }
.pr2 { padding-right: var(--spacing-small); }
.pr3 { padding-right: var(--spacing-medium); }
.pr4 { padding-right: var(--spacing-large); }
.pr5 { padding-right: var(--spacing-extra-large); }
.pr6 { padding-right: var(--spacing-extra-extra-large); }
.pr7 { padding-right: var(--spacing-extra-extra-extra-large); }

.pb0 { padding-bottom: var(--spacing-none); }
.pb1 { padding-bottom: var(--spacing-extra-small); }
.pb2 { padding-bottom: var(--spacing-small); }
.pb3 { padding-bottom: var(--spacing-medium); }
.pb4 { padding-bottom: var(--spacing-large); }
.pb5 { padding-bottom: var(--spacing-extra-large); }
.pb6 { padding-bottom: var(--spacing-extra-extra-large); }
.pb7 { padding-bottom: var(--spacing-extra-extra-extra-large); }

.pt0 { padding-top: var(--spacing-none); }
.pt1 { padding-top: var(--spacing-extra-small); }
.pt2 { padding-top: var(--spacing-small); }
.pt3 { padding-top: var(--spacing-medium); }
.pt4 { padding-top: var(--spacing-large); }
.pt5 { padding-top: var(--spacing-extra-large); }
.pt6 { padding-top: var(--spacing-extra-extra-large); }
.pt7 { padding-top: var(--spacing-extra-extra-extra-large); }

.pv0 {
 padding-top: var(--spacing-none);
 padding-bottom: var(--spacing-none);
}
.pv1 {
 padding-top: var(--spacing-extra-small);
 padding-bottom: var(--spacing-extra-small);
}
.pv2 {
 padding-top: var(--spacing-small);
 padding-bottom: var(--spacing-small);
}
.pv3 {
 padding-top: var(--spacing-medium);
 padding-bottom: var(--spacing-medium);
}
.pv4 {
 padding-top: var(--spacing-large);
 padding-bottom: var(--spacing-large);
}
.pv5 {
 padding-top: var(--spacing-extra-large);
 padding-bottom: var(--spacing-extra-large);
}
.pv6 {
 padding-top: var(--spacing-extra-extra-large);
 padding-bottom: var(--spacing-extra-extra-large);
}

.pv7 {
 padding-top: var(--spacing-extra-extra-extra-large);
 padding-bottom: var(--spacing-extra-extra-extra-large);
}

.ph0 {
 padding-left: var(--spacing-none);
 padding-right: var(--spacing-none);
}

.ph1 {
 padding-left: var(--spacing-extra-small);
 padding-right: var(--spacing-extra-small);
}

.ph2 {
 padding-left: var(--spacing-small);
 padding-right: var(--spacing-small);
}

.ph3 {
 padding-left: var(--spacing-medium);
 padding-right: var(--spacing-medium);
}

.ph4 {
 padding-left: var(--spacing-large);
 padding-right: var(--spacing-large);
}

.ph5 {
 padding-left: var(--spacing-extra-large);
 padding-right: var(--spacing-extra-large);
}

.ph6 {
 padding-left: var(--spacing-extra-extra-large);
 padding-right: var(--spacing-extra-extra-large);
}

.ph7 {
 padding-left: var(--spacing-extra-extra-extra-large);
 padding-right: var(--spacing-extra-extra-extra-large);
}
.ma0  {  margin: var(--spacing-none); }
.ma1 {  margin: var(--spacing-extra-small); }
.ma2  {  margin: var(--spacing-small); }
.ma3  {  margin: var(--spacing-medium); }
.ma4  {  margin: var(--spacing-large); }
.ma5  {  margin: var(--spacing-extra-large); }
.ma6 {  margin: var(--spacing-extra-extra-large); }
.ma7 { margin: var(--spacing-extra-extra-extra-large); }

.ml0  {  margin-left: var(--spacing-none); }
.ml1 {  margin-left: var(--spacing-extra-small); }
.ml2  {  margin-left: var(--spacing-small); }
.ml3  {  margin-left: var(--spacing-medium); }
.ml4  {  margin-left: var(--spacing-large); }
.ml5  {  margin-left: var(--spacing-extra-large); }
.ml6 {  margin-left: var(--spacing-extra-extra-large); }
.ml7 { margin-left: var(--spacing-extra-extra-extra-large); }

.mr0  {  margin-right: var(--spacing-none); }
.mr1 {  margin-right: var(--spacing-extra-small); }
.mr2  {  margin-right: var(--spacing-small); }
.mr3  {  margin-right: var(--spacing-medium); }
.mr4  {  margin-right: var(--spacing-large); }
.mr5  {  margin-right: var(--spacing-extra-large); }
.mr6 {  margin-right: var(--spacing-extra-extra-large); }
.mr7 { margin-right: var(--spacing-extra-extra-extra-large); }

.mb0  {  margin-bottom: var(--spacing-none); }
.mb1 {  margin-bottom: var(--spacing-extra-small); }
.mb2  {  margin-bottom: var(--spacing-small); }
.mb3  {  margin-bottom: var(--spacing-medium); }
.mb4  {  margin-bottom: var(--spacing-large); }
.mb5  {  margin-bottom: var(--spacing-extra-large); }
.mb6 {  margin-bottom: var(--spacing-extra-extra-large); }
.mb7 { margin-bottom: var(--spacing-extra-extra-extra-large); }

.mt0  {  margin-top: var(--spacing-none); }
.mt1 {  margin-top: var(--spacing-extra-small); }
.mt2  {  margin-top: var(--spacing-small); }
.mt3  {  margin-top: var(--spacing-medium); }
.mt4  {  margin-top: var(--spacing-large); }
.mt5  {  margin-top: var(--spacing-extra-large); }
.mt6 {  margin-top: var(--spacing-extra-extra-large); }
.mt7 { margin-top: var(--spacing-extra-extra-extra-large); }

.mv0   {
 margin-top: var(--spacing-none);
 margin-bottom: var(--spacing-none);
}
.mv1  {
 margin-top: var(--spacing-extra-small);
 margin-bottom: var(--spacing-extra-small);
}
.mv2   {
 margin-top: var(--spacing-small);
 margin-bottom: var(--spacing-small);
}
.mv3   {
 margin-top: var(--spacing-medium);
 margin-bottom: var(--spacing-medium);
}
.mv4   {
 margin-top: var(--spacing-large);
 margin-bottom: var(--spacing-large);
}
.mv5   {
 margin-top: var(--spacing-extra-large);
 margin-bottom: var(--spacing-extra-large);
}
.mv6  {
 margin-top: var(--spacing-extra-extra-large);
 margin-bottom: var(--spacing-extra-extra-large);
}
.mv7  {
 margin-top: var(--spacing-extra-extra-extra-large);
 margin-bottom: var(--spacing-extra-extra-extra-large);
}

.mh0   {
 margin-left: var(--spacing-none);
 margin-right: var(--spacing-none);
}
.mh1   {
 margin-left: var(--spacing-extra-small);
 margin-right: var(--spacing-extra-small);
}
.mh2   {
 margin-left: var(--spacing-small);
 margin-right: var(--spacing-small);
}
.mh3   {
 margin-left: var(--spacing-medium);
 margin-right: var(--spacing-medium);
}
.mh4   {
 margin-left: var(--spacing-large);
 margin-right: var(--spacing-large);
}
.mh5   {
 margin-left: var(--spacing-extra-large);
 margin-right: var(--spacing-extra-large);
}
.mh6  {
 margin-left: var(--spacing-extra-extra-large);
 margin-right: var(--spacing-extra-extra-large);
}
.mh7  {
 margin-left: var(--spacing-extra-extra-extra-large);
 margin-right: var(--spacing-extra-extra-extra-large);
}

/* Color */
.bg-gray { background-color: #f8f8f8; }

/* Font Size */
.f1 { font-size: 3rem; }
.f2 { font-size: 2.25rem; }
.f3 { font-size: 1.5rem; }
.f4 { font-size: 1.25rem; }
.f5 { font-size: 1rem; }
.f6 { font-size: .875rem; }
.f7 { font-size: .75rem; }

/* Font Weight */
.fw4 { font-weight: 400; }
.fw5 { font-weight: 500; }
.fw6 { font-weight: 600; }
.fw7 { font-weight: 600; }

.text-sans-serif { font-family: sans-serif; }
.text-serif { font-family: serif; }
.text-uppercase { text-transform: uppercase; }

/* Border */
.border-light { border: 1px solid #dddddd; }
.border { border: 1px solid #dedede; }

.border-bottom {
 border-bottom: 1px solid #dddddd;
 &:last-of-type {
   border-bottom: 0;
 }
}

.border-top { border-top: 1px solid #dddddd; }
.border-right { border-right: 1px solid #dddddd; }

.border-radius-3 { border-radius: 3px; }

.justify-center { justify-content: center; }
.align-items-center { align-items: center; }

/* Display */
.inline-block { display: inline-block; }
.block { display: block; }

/* Background */
.bg-white { background-color: white; }
.bg-light { background-color: whitesmoke; }
---
app/assets/stylesheets/application.scss
---
// *= require trix

@import "bulma";
@import "functions";
@import "jobs";
@import "stripe";

.notification {
  border-radius: 0;
}

.notification:not(:last-child) {
  margin-bottom: 0;
}

.hint {
  font-size: small;
}

.tag {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 9px !important;
}
---
app/views/jobs/show.html.erb
---
<p id="notice"><%= notice %></p>

<p>
  <strong>Title:</strong>
  <%= @job.title %>
</p>

<p>
  <strong>Description:</strong>
  <%= @job.description %>
</p>

<p>
  <strong>Url:</strong>
  <%= @job.url %>
</p>

<p>
  <strong>Job type:</strong>
  <%= @job.job_type %>
</p>

<p>
  <strong>Location:</strong>
  <%= @job.location %>
</p>

<p>
  <strong>Job author:</strong>
  <%= @job.job_author %>
</p>

<p>
  <strong>Remote ok:</strong>
  <%= @job.remote_ok %>
</p>

<p>
  <strong>Apply url:</strong>
  <%= @job.apply_url %>
</p>

<%= link_to 'Edit', edit_job_path(@job) %> |
<%= link_to 'Back', jobs_path %>
---

<div class="columns pt4 pb7">
  <div class="column is-7 is-offset-1">

    <p class="f7"><i class="fa fa-clock"></i> Posted <%= time_ago_in_words(@job.created_at) %> ago</p>
    <h1 class="title is-2"><%= @job.title %></h1>

    <ul class="list pb4">
      <li class="inline-block f6 pr2"><%#= job_type(@job.job_type) %></li>
      <li class="inline-block f6 ph2"><i class="fa fa-pin"></i> <%= @job.location %></li>
      <% if @job.remote_ok? %>
        <li class="inline-block f6 ph2"><i class="fa fa-wifi"></i> Remote Job</li>
      <% end %>
    </ul>

    <div class="content text-serif f4">
      <%= @job.description.html_safe %>
    </div>

    <%= link_to 'Apply to this job', @job.apply_url, class:"button is-rounded is-large is-fullwidth is-link" %>

  </div>

  <div class="column is-2 is-offset-1 has-text-centered">

    <%# if !@job.avatar.file.nil? %>
      <%#= link_to image_tag(@job.avatar_url(:thumb), alt: @job.job_author, class: "has-text-centered"), @job.url %>
    <%# end %>

    <h5 class="is-5 has-text-centered"><%= link_to @job.job_author, @job.url %></h5>

    <div class="mt2 mb4">
    <%= link_to @job.url do %>
      <i class="fa fa-globe"></i>
    <% end %>
    </div>

    <%= link_to 'Apply to this job', @job.apply_url, class:"button is-rounded is-fullwidth is-link" %>

    <%# if current_user.try(:admin) || job_author(@job) %>
      <ul class="pv3">
        <li class="pv1 f6">Admin controls: </li>
        <li class="pv1 inline-block">
          <%= link_to 'View', @job, class: 'button is-small is-link is-outlined' %></li>
        <li class="pv1 inline-block">
          <%= link_to 'Edit', edit_job_path(@job), class: 'button is-small is-link is-outlined' %></li>
        <li class="pv1 inline-block">
          <%= link_to 'Delete', @job, method: :delete, data: { confirm: 'Are you sure?' }, class: 'button is-small is-link is-outlined' %></li>
      </ul>
    <%# end %>
  </div>
</div>
---
app/views/jobs/index.html.erb
---
<p id="notice"><%= notice %></p>

<h1>Jobs</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Description</th>
      <th>Url</th>
      <th>Job type</th>
      <th>Location</th>
      <th>Job author</th>
      <th>Remote ok</th>
      <th>Apply url</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @jobs.each do |job| %>
      <tr>
        <td><%= job.title %></td>
        <td><%= job.description %></td>
        <td><%= job.url %></td>
        <td><%= job.job_type %></td>
        <td><%= job.location %></td>
        <td><%= job.job_author %></td>
        <td><%= job.remote_ok %></td>
        <td><%= job.apply_url %></td>
        <td><%= link_to 'Show', job %></td>
        <td><%= link_to 'Edit', edit_job_path(job) %></td>
        <td><%= link_to 'Destroy', job, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Job', new_job_path %>
---
<div class="columns border-top">

  <div class="column is-2 bg-light">
    <div class="pl3 pr1">
      <%= render 'panel' %>
    </div>
  </div>

  <div class="column is-9 pl6">
    <% @jobs.each do |job| %>
    <div class="columns border-bottom pt4">
      <div class="column is-1">
        <% if !job.avatar.file.nil? %>
          <%= link_to image_tag(job.avatar_url(:thumb), alt: job.job_author, width: 100, height: 100), job.url %>
        <% end %>
      </div>
      <div class="column is-8">
        <h3 class="title is-4 index-title"><%= link_to job.title, job %></h3>
          <ul>
            <li><%= link_to job.job_author, job.url %></li>
          </ul>
        <div class="pv2 f6">
          <%= sanitize(job.description.truncate(200, separator: '</p>')) %>
        </div>

        <% if current_user.try(:admin) || job_author(job) %>
          <ul class="pv3">
            <li class="inline-block f6">Admin controls: </li>
            <li class="inline-block">
              <%= link_to 'View', job, class: 'button is-small is-link is-outlined' %></li>
            <li class="inline-block">
              <%= link_to 'Edit', edit_job_path(job), class: 'button is-small is-link is-outlined' %></li>
            <li class="inline-block">
              <%= link_to 'Delete', job, method: :delete, data: { confirm: 'Are you sure?' }, class: 'button is-small is-link is-outlined' %></li>
          </ul>
        <% end %>

        </div>
        <div class="column has-text-right">
          <%= job_type(job.job_type) %>
          <p class="pt2 f6"><%= job.location %></p>
        </div>
      </div>
    <% end %>
  </div>

</div>
----
app/views/jobs/_panel.html.erb
---
<nav class="panel">

  <p class="text-uppercase has-text-grey pv1 f6">filter by job type</h3>

  <ul class="mb3 pa0">

    <li class="pv1">
      <%= link_to 'All', jobs_path, class: 'button is-fullwidth is-dark' %>
    </li>


    <li class="pv1">
      <%= link_to 'Full-time', jobs_path(job_type: "Full-time"), class: 'button is-fullwidth is-primary' %>
    </li>

    <li class="pv1">
      <%= link_to 'Part-time', jobs_path(job_type: "Part-time"), class: 'button is-fullwidth is-link' %>
    </li>

    <li class="pv1">
      <%= link_to 'Freelance', jobs_path(job_type: "Freelance"), class: 'button is-fullwidth is-warning' %>
    </li>

    <li class="pv1">
      <%= link_to 'Contract', jobs_path(job_type: "Contract"), class: 'button is-fullwidth is-info' %>
    </li>
  </ul>

</nav>
---
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq8tyyg0xjj31kw0mywhg.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fq8tysspnjj31kw0l8mzq.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fq8tynqiixj31kw0p3q77.jpg)

```
git add .
git commit -m "edit job index & show & form add panel"
git push origin job-scaffold
```

```
git checkout -b uploader
rails generate uploader Avatar
---
app/uploaders/avatar_uploader.rb
---
include CarrierWave::MiniMagick

version :thumb do
  process resize_to_fit: [50, 50]
end

def extension_whitelist
  %w(jpg jpeg gif png)
end

---
rails g migration add_avatar_to_jobs avatar:string
rake db:migrate
---

app/models/user.rb
---
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  has_many :jobs
end

app/models/job.rb
---
class Job < ApplicationRecord
  belong_to :user
  mount_uploader :avatar, AvatarUploader

  JOB_TYPES = ["Full-time", "Part-time", "Contract", "Freelance"]
end
---

```
![image](https://ws1.sinaimg.cn/large/006tKfTcly1fq8vepsd8rj31fk0omdna.jpg)
```
git add .
git commit -m "add uploader"
git push origin uploader
```

```
git checkout -b figaro
bundle exec figaro install
rails g migration add_user_id_to_jobs user_id:integer
rake db:migrate
---
app/controllers/jobs_controller.rb
---
class JobsController < ApplicationController
  before_action :set_job, only: [:show, :edit, :update, :destroy]
  before_action :authenticate_user!, except: [:index, :show]

  # GET /jobs
  # GET /jobs.json
  def index
    if(params.has_key?(:job_type))
      @jobs = Job.where(job_type: params[:job_type]).order("created_at desc")
    else
      @jobs = Job.all.order("created_at desc")
    end
  end

  # GET /jobs/1
  # GET /jobs/1.json
  def show
  end

  # GET /jobs/new
  def new
    @job = current_user.jobs.build
  end

  # GET /jobs/1/edit
  def edit
  end

  # POST /jobs
  # POST /jobs.json
  def create
    @job = current_user.jobs.build(job_params)

    token = params[:stripeToken]
    job_type = params[:job_type]
    job_title = params[:title]
    card_brand = params[:user][:card_brand]
    card_exp_month = params[:user][:card_exp_month]
    card_exp_year = params[:user][:card_exp_year]
    card_last4 = params[:user][:card_last4]

    charge = Stripe::Charge.create(
      :amount => 30000,
      :currency => "usd",
      :description => job_type,
      :statement_descriptor => job_title,
      :source => token,
    )
    current_user.stripe_id = charge.id
    current_user.card_brand = card_brand
    current_user.card_exp_month = card_exp_month
    current_user.card_exp_year = card_exp_year
    current_user.card_last4 = card_last4
    current_user.save!

    respond_to do |format|
      if @job.save
        format.html { redirect_to @job, notice: '🎉 Your job listing was purchased successfully!' }
        format.json { render :show, status: :created, location: @job }
      else
        format.html { render :new }
        format.json { render json: @job.errors, status: :unprocessable_entity }
      end
    end

    rescue Stripe::CardError => e
      flash.alert = e.message
      render action: :new
  end

  # PATCH/PUT /jobs/1
  # PATCH/PUT /jobs/1.json
  def update
    respond_to do |format|
      if @job.update(job_params)
        format.html { redirect_to @job, notice: 'Job was successfully updated.' }
        format.json { render :show, status: :ok, location: @job }
      else
        format.html { render :edit }
        format.json { render json: @job.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /jobs/1
  # DELETE /jobs/1.json
  def destroy
    @job.destroy
    respond_to do |format|
      format.html { redirect_to jobs_url, notice: 'Job was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_job
      @job = Job.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def job_params
      params.require(:job).permit(:title, :description, :url, :apply_url, :job_type, :job_author, :remote_ok, :location, :avatar)
    end
end

---
app/views/jobs/new.html.erb
---
<h1>New Job</h1>

<%= render 'form', job: @job %>

<%= link_to 'Back', jobs_path %>
---
<section class="section">
  <div class="container">
    <div class="columns">
      <div class="column is-8 is-offset-2">
        <h1 class="title is-3">Post new Job</h1>
        <%= render 'form', job: @job %>
      </div>
    </div>
  </div>
</section>
---
app/views/jobs/_payment.html.erb
---
<% if !@job.persisted? %>
  <h4 class="subtitle is-4">Payment</h4>

  <div class="notification is-light has-text-centered">
    <i class="fa fa-credit-card"></i>
    All jobs cost <strong>$300</strong> USD for 30 days.
    <span class="block mb4"></span>
  </div>

  <div class="form-row">
    <label for="card-element" class="label">
      Enter credit or debit card
    </label>

    <div id="card-element" class="form-control">
      <!-- a Stripe Element will be inserted here. -->
    </div>
    <!-- used to display Element errors -->
    <div id="card-errors" role="alert"></div>
  </div>

  <% if current_user.stripe_id.present? %>
    <div class="last-card bg-light pa4 border-radius-3 mt4">
      <h5 class="is-6 title pt2">Previously used card</h5>
      <table class="table is-narrow bg-light is-fullwidth">
        <thead>
          <th>Card Brand</th>
          <th>Last 4 Digits</th>
          <th>Expiration Month</th>
          <th>Expiration Year</th>
        </thead>
        <tr>
          <td><%= current_user.card_brand %></td>
          <td><%= current_user.card_last4 %></td>
          <td><%= current_user.card_exp_month %></td>
          <td><%= current_user.card_exp_year %></td>
        </tr>
      </table>
    </div>
  <% end %>
<% end %>
---
config/initializers/stripe.rb
---
Rails.configuration.stripe = {
  :publishable_key => ENV['stripe_publishable_key'],
  :secret_key      => ENV['stripe_api_key']
}

Stripe.api_key = ENV['stripe_api_key']
---
app/helpers/jobs_helper.rb
---

module JobsHelper

  def job_type(job_type)
    if job_type == "Full-time"
      content_tag :span, "#{job_type}", class: "tag is-primary"
    elsif job_type == "Part-time"
      content_tag :span, "#{job_type}", class: "tag is-link"
    elsif job_type == "Freelance"
      content_tag :span, "#{job_type}", class: "tag is-warning"
    elsif job_type == "Contract"
      content_tag :span, "#{job_type}", class: "tag is-info"
    else
      ""
    end
  end

  def job_author(job)
    user_signed_in? && current_user.id == job.user_id
  end
end
---
```
# 添加数据
```
user = User.new(
  id: 3,
  email: "xiaowei@example.com",
  password: "password",
  password_confirmation: "password"
)
user.save!

Job.create!([{
  title: "React Engineer",
  description: "Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna. Aenean lacinia bibendum nulla sed consectetur. Donec sed odio dui. Venenatis dapibus posuere. Nullam id dolor id nibh ultricies vehicula ut id elit. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Curabitur blandit tempus porttitor. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Nullam id dolor id nibh ultricies vehicula ut id elit. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Nulla vitae elit libero, a pharetra augue. Nulla vitae elit libero, a pharetra augue. Vestibulum id ligula porta felis euismod semper. Maecenas faucibus mollis interdum. Maecenas sed diam eget risus varius blandit sit amet non magna.",
  url: "https://facebook.com",
  apply_url: "https://facebook.com/apply",
  job_type: "Full-time",
  location: "San Francisco, CA",
  job_author: "Facebook",
  user_id: user.id,
  remote_ok: false
},
{
  title: "Product Designer",
  description: "Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna. Aenean lacinia bibendum nulla sed consectetur. Donec sed odio dui. Venenatis dapibus posuere. Nullam id dolor id nibh ultricies vehicula ut id elit. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Curabitur blandit tempus porttitor. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Nullam id dolor id nibh ultricies vehicula ut id elit. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Nulla vitae elit libero, a pharetra augue. Nulla vitae elit libero, a pharetra augue. Vestibulum id ligula porta felis euismod semper. Maecenas faucibus mollis interdum. Maecenas sed diam eget risus varius blandit sit amet non magna.",
  url: "https://slack.com",
  apply_url: "https://slack.com/apply",
  job_type: "Contract",
  location: "San Francisco, CA",
  job_author: "Slack",
  user_id: user.id,
  remote_ok: false
},
{
  title: "UI/UX Designer",
  description: "Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna. Aenean lacinia bibendum nulla sed consectetur. Donec sed odio dui. Venenatis dapibus posuere. Nullam id dolor id nibh ultricies vehicula ut id elit. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Curabitur blandit tempus porttitor. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Nullam id dolor id nibh ultricies vehicula ut id elit. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Nulla vitae elit libero, a pharetra augue. Nulla vitae elit libero, a pharetra augue. Vestibulum id ligula porta felis euismod semper. Maecenas faucibus mollis interdum. Maecenas sed diam eget risus varius blandit sit amet non magna.",
  url: "https://trello.com",
  apply_url: "https://trello.com/apply",
  job_type: "Full-time",
  location: "Anywhere",
  job_author: "Trello",
  user_id: user.id,
  remote_ok: true
}
])
---
rake db:drop
rake db:create
rake db:migrate
rake db:seed
rails server
---
```
```
rails c
2.3.1 :001 > user = User.last
2.3.1 :002 > user.admin = ture
2.3.1 :003 > user.admin = true
2.3.1 :004 > user.save
2.3.1 :005 > exit
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fq9mix268sj31kw0fmjvo.jpg)

# 结束表现形式
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mmd5ufhj31kw0mvn07.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fq9mmaemu7j31kw0smn0v.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mm41jxuj31kw0pcmze.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fq9mlpfkprj31kw0jxwh0.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fq9mlj5siwj31kw0j7mzb.jpg)
