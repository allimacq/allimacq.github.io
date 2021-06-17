---
layout: post
title:      "Devise with OmniAuth: A Tutorial"
date:       2021-06-17 16:52:16 +0000
permalink:  devise_with_omniauth_a_tutorial
---


For my third portfolio project at Flatiron, I continued with the accessible park idea. Setting up my models, controllers, views, etc was fine, but what I struggled with was setting up Google Omniauth with Devise. This is what I will cover here.

From the [Devise](http:https://github.com/heartcombo/devise//) README, 
> Devise is a flexible authentication solution for Rails based on Warden. It: 
> * Is Rack based;
> * Is a complete MVC solution based on Rails engines;
> * Allows you to have multiple models signed in at the same time;
> * Is based on a modularity concept: use only what you really need. 

Sounds great, right? Anyways, Devise comes with Omniauth built-in, so if you're new to Rails or authentication, you need to be careful during setup.

I'm going to demonstrate how to configure Devise with Google and I'm going to assume your Devise model (generally user) is already set up. To start, here are the gems that you will need:
```
gem 'omniauth-google-oauth2'
gem 'omniauth-rails_csrf_protection'
```
Next, you will need to add two columns to your Devise model and then run the migration:

```
rails g migration AddOmniauthToUsers uid:string provider:string
```
Great! Now, if you were using Omniauth sans Devise, you would put the following into *config/initializers/omniauth.rb*, **BUT** since Devise comes with Omniauth support out of the box, we're going to use **config/initializers/devise.rb**

So go into config/initializers/devise.rb and add:
```
config.omniauth :google_oauth2, Rails.application.credentials.dig(:google, :google_client_id),
  Rails.application.credentials.dig(:google, :google_client_secret), scope:'userinfo.email,userinfo.profile'
```
Now we need to get our credentials from [Google](https://console.developers.google.com), select your project, go to Credentials, and select the Oauth consent screen. Make a note of your **client_id** and **client_secret** because you need them in the next step!

In your terminal make sure you are in your project directory and enter the following:
```
EDITOR="code --wait" rails credentials:edit
```
*(Note: "code" was entered here because the editor I use is visual studio. For example, if you use Atom, replace 'code' with 'atom' )*

A file should open up and your command should be left haning in the terminal if you did this correctly. 

In the file, add your client_id and client_secret like so (beware of whitespace!):

```
google:
  google_client_id: 1#numberlettercombohere
  google_client_secret: #numberlettercombohere

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: #this is already provided for you!
```

Now save and close the file. To make sure you did this correctly, enter the following into your terminal:

```
rails credentials:show
```
You should now see what you just entered output in your terminal!


Phew! We are almost there guys!

Now, let's go into our Devise model (mine is User) and add the following to the devise block:

```
:omniauthable, omniauth_providers: [:google_oauth2]
```

We are going to also add the following method to our user model:

```
def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0, 20]
      #user.name = auth.info.name   # assuming the user model has a name
      #user.image = auth.info.image # assuming the user model has an image
      # If you are using confirmable and the provider(s) you use validate emails, 
      # uncomment the line below to skip the confirmation emails.
      # user.skip_confirmation!
    end
  end
	
```

Now, let's go to our config/routes.rb file. After `devise_for :users`, please add the following:
```
controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }
```

All together it should look like this:

```
devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }

```

As you may have noticed above, we mentioned a controller we don't have, so let's add it now!

Add a folder called "users" under "app/controllers" and then create a file called "omniauth_callbacks_controller.rb" and add the following:

```
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
    skip_before_action :verify_authenticity_token, only: :google_oauth2

    def google_oauth2
      # You need to implement the method below in your model (e.g. app/models/user.rb)
      @user = User.from_omniauth(request.env["omniauth.auth"])
      if @user.persisted?
        sign_in_and_redirect @user, event: :authentication #this will throw if @user is not activated
        set_flash_message(:notice, :success, kind: "Google") if is_navigational_format?
      else
        session["devise.google_oauth2_data"] = request.env["omniauth.auth"]
        redirect_to new_user_registration_url
      end
    end
  
    def failure
      redirect_to root_path
    end
  end
```

As you can see, #google_oauth2 uses the User class method #from_omniauth that we snuck in earlier, and now you know why!

Now, the last thing we need to do is add the link to our views! I used the button_to instead of link_to since the newer version requires that 'link_to' be used with 'method: post' (and fun fact! button_to is the same as link_to with method post!)

```
<%= button_to "Sign in with Google", user_google_oauth2_omniauth_authorize_path %>
```

Huzzah! I really hope this helps someone and saves them some time :)






