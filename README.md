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
    (add config.action_mailer.default_url_options = { host: 'localhost', port: 3000 } to development.rb)
    (add root 'posts#index' to routes.rb)
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

## ADD NESTED SCAFFOLD GEM

        gem 'nested_scaffold'
        bundle install

## CREATED SCAFFOLD

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
