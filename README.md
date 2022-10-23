## THIS IS A RAILS CHEAT SHEET

## INSTALLING RAILS APP WITH POSTGRES

rails new myapp --database=postgresql

## CHANGING DATABASE

add your username and password to database.yml

## CREATING A DATABASE

    rails db:create

## ADDING DEVISE

    gem 'devise'
    bundle install
    rails generate devise:install
    (add config.action_mailer.default_url_options = { host: 'localhost', port: 3000 } to config/environmrnt/development.rb)
    (add root 'posts#index' to routes.rb)
    rails generate devise:views
    rails generate devise User
    rails db:migrate

## CREATING BASE SCAFFOLD WITH REFERENCE TO USER

    rails generate scaffold Post title:string body:text user:references
    rails db:migrate

## ADD BEFORE ACTION TO BASE CONTROLLER (POSTS CONTROLLER)

    This ensures that a user is logged in before they can create a post
    before_action :authenticate_user!

## ADD EXTRA FIELDS TO USER TABLE

    rails generate migration add_fieldname_to_tablename fieldname:string
    rails generate migration add_name_to_users name:string
    rails db:migrate

## ADD EXTRA INPUT FIELDS IN DEVISE REGISTRATION FORM

## ADD HAS MANY TO USER MODEL

    has_many :posts

## ADD CODE TO ALLOW ONE TO ADD EXTRA DETAILS ON LOGIN

in app/controllers/application_controller.rb

    before_action :configure_permitted_parameters, if: :devise_controller?

    protected

    def configure_permitted_parameters
      devise_parameter_sanitizer.permit(:sign_up, keys: [:firstname, :lastname])
    end

## ADD EXTRA FIELS IN VIEWS FOR REGISTRATION

<div class="field">
    <%= f.label :name %><br />
    <%= f.text_field :name, autofocus: true, autocomplete: "name" %>
  </div>

## Add :turbo_stream as a navigational format. This line goes in config/initializers/devise.rb.

        config.navigational_formats = [:html, :turbo_stream]

## REMOVE EXTRA ALERTS FROM LAYOUTS/APP.HTML.ERB

    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>

remove these two

## REMOVE USER ID FROM NEW POST VIEWS

    <div>
    <%= form.label :user_id, style: "display: block" %>
    <%= form.text_field :user_id %>

  </div>

## CONFIGURE CURRENT USER IN POSTS CONTROLLER

add this line to create , new and edit methods
@post.user = current_user

## REMOVE USER ID IN POST PARAMS IN POSTS CONTROLLER

    def post_params
      params.require(:post).permit(:title, :text)
    end

## TO ONLY SEE POSTS FOR CURRENT USER

    def index
      @posts = Post.where(user_id: current_user.id)
    end

## ADD NESTED SCAFFOLD GEM

        gem 'nested_scaffold'
        bundle install

## CREATED SCAFFOLD

            rails generate nested_scaffold PARENT_NAME/NAME [field:type field:type] [options]

            rails g nested_scaffold post/comment  text:string  post:references user:references
            rails db:migrate

## CONFIGURE NESTED ROUTES

    resources :posts do
      resources :comments
    end

## REMOVE USER ID AND POST FROM NEW COMMENT VIEW

    <div>
    <%= form.label :user_id, style: "display: block" %>
    <%= form.text_field :user_id %>
    </div>

    <div>
    <%= form.label :post_id, style: "display: block" %>
    <%= form.text_field :post_id %>
    </div>

## ADD CODE TO ALLOW DELETING POSTS THAT HAVE COMMENTS

has_many :comments , dependent: :destroy

## ADD CODE TO CONFIGURE CURRENT USER TO COMMENT

add this line to comments controller (create , new and edit methods)
@comment.user_id = current_user.id

## CHANGE UPDATE METHOD IN COMMENTS CONTROLLER

     def update
        @comment.user_id = current_user.id
         @comment.post_id = @post.id
        @comment.update(comment_params)
         redirect_to post_comments_url(@post)

     end

## CHANGE DELETE BUTTON TO

 <td><%= button_to 'Destroy', [comment.post, comment], method: :delete, data: { confirm: 'Are you sure?' } %></td>

## TO HAVE A HOME PAGE BEFORE AUTHETICATION AND WITH LINKS FOR SIGN IN AND SIGN UP

    rails generate controller home index

## ADD THIS TO ROUTES

     authenticated :user do
    root "categories#index", as: :authenticated_root

end

root to: "home#index"

## ADD THIS TO HOME INDEX VIEW

<%= link_to "Sign up", new_user_registration_path %>
<%= link_to "Log in", new_user_session_path %>

## FOR SIGNING OUT ADD THIS TO THE ROUTES

devise_scope :user do
get '/users/sign_out' => 'devise/sessions#destroy'
end

## TO SIGN OUT

<p><%= link_to "Sign Out", destroy_user_session_path %></p>
<p><%= link_to "Sign In", new_user_session_path %></p>
<p><%= link_to "Sign Up", new_user_registration_path %></p>

## PUSHING TO HEROKU

switch to master branch

        heroku login
        heroku create
        heroku stack

        heroku stack:set heroku-18

        git push heroku master

        heroku run rails db:migrate

## CREATING RAILS API ONLY APP

rails new my_first_rails_api --api --database=postgresql

## CREATE MODEL

rails g model dog name:string age:integer

## CREATE MIGRATION

rails db:migrate

## CREATE CONTROLLER

rails g controller Api/v1/dogs
