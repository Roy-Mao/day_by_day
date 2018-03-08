# How to coercwe a result to boolean value in Ruby?

Use the bang-bang trick. For example: !!(user&&user.authenticate('foobar'))

# What will this command generate?

> rails generate integration_test users_login

This command will generate a integration testing file of which the name should be users_login_test.rb in the directory of test/integration

# How to run (and only one) test file using rails test command?

> rails test test/integration/users_login_test.rb

# Difference between flash and flash.now

Unlike flash, the contents of flash.now disappear as soon as there is an additional request.

# What is the rails command to run the full test suite?

> rails test

# What is the difference between a temporary session cookie and the persistant session cookie?

The difference is weather the it expires upon the closing of browser. A temporary session cookie expires when we close the browser while a persistant sessision cookie will persist even if we close the browser.

# What is special about generating a session controller compared with other normal controller?

## First of all, why should the Session Controller be special?

Because implementing sessions will involve large number of related functions across **many many controllers and views**, that is why we need to make it special.

## So, how to make it special enough to facilitate the implemention in multiple controllers and views?

The answer is to adjsut the superclass of all the controllers, which is ApplicationController, whose inherites from ApplicationController::Base.

```ruby
  class ApplciationController < ActionController::Base
  	protect_from_forgery with::exception
  	include Session
  end
``` 

As for the views, this helper is automatically included in Rails views.

# What is session method in Ruby?

The session method is a **pre-defined** method in Ruby. We can treat it as a hash.

# The difference between cookie method and session method in Ruby.

  * One is used to create a temporary cookie which expires upon the closing of a browser, one is persistant which still exists even if we close the current browser. 

  * The permanent cookie is vulnerable to session hijacking attack. Be careful about the info you stored in the client browser!!


# How to login a user in Rails?

Location: After we create the special Session controller, we can find a log_in method in the SessionsHelper module. We just need to add the following line to securely log in the user.

> session[:user_id] = user.id

# What is the differences between find and find_by in Rails?

  To put it simple, the difference is the return value of non-existant searching result. While find will return an ActiveRecord Error when no results found, the find_by method will return nil.

## What is more important is to understand why? Why find return an ActiveRecord error while find_by returns nil?

  > One sentence answer: find method is used for **searching a record** while find_by is used for **matching a record**

  Asking yourself if you are certain there is a record in the database or not? That is to say, find is used when you are certain there is record in the database, you are using this method to find a record, you are **sure there is record in the database.** If you are not sure and the element is not **expected** to be exist in the database, then use find_by will be the appropriate approach.

  For details: [StackFlow Answer](https://stackoverflow.com/questions/4966430/rails-2-model-find1-gives-activerecord-error-when-id-1-does-not-exist/4966553#4966553)

# How to define the current user? and the 'or equals' operator

```ruby
  def current_user
  	@current_user = @current_user || User.find_by(id:session[:user_id])
  end
```

# What is fixture in Ruby?

Here is the [documentation](http://api.rubyonrails.org/classes/ActiveRecord/FixtureSet.html)

To put it simple, it is a way of organizing data to be loaded into the test database. Fixture is used for testing that has something to do with the database. Check out the detailed documentation above when we do TDD and need use some data for unit test.

# How to add a digest method for use in fixtures.

Location: app/models/user.rb

```ruby
  def User.digest(string)
  	cost = ActiveModel::SecurePassword.min_cost ? BCrypt::Engine::MIN_COST :BCrypt::Engine.cost
  	BCrypt::Password.create(string, cost: cost)
  end
```

# What is the difference in the Sessions Controller. sessions#new and sessions#create?

sessions#new is used for a login page while sessions#create is to complete the login.

# Where should we place the customizerd log_in and log_out method?

We should place these two special methods inside the SessionsHelper module like this:

```ruby
module SessionsHelper
  def log_in(user)
    session[:user_id] = user.id
  end

  def log_out
    session.delete(:user_id)
    @current_user = nil
  end
end
```

After which , we define the destroy method in the SessionsController

Location: app/controllers/sessions_controller.rb

Example code
```ruby
  class SessionsController < ApplicationController
    def destroy
      log_out #the definition of this method can be found in the sessions helper
      redirect_to root_url
    end
  end
```

# What is the Integration tests used for?

Integration tests can verify correct routes, database updates and proper changes to the layout.

# [Firesheep](http://codebutler.github.io/firesheep/) is an amazing FireFox extension to demonstrate the importance of cyber security!!

# As a general rule, if a method doesn’t need an instance of an object, it should be a class method.

For example:

Location: app/models/user.rb

Within our User class

```ruby
class User < ApplicationRecord
  # Returns the hash digest of the giving string
  def User.digest(string)
  	cost ActiveModel::SecurePassword.min_cost?BCrypt::Engine::MIN_COST :BCrypt::Engine.cost
  	BCrypt::Password.create(string, cost: cost)
  end

  # Returns a random token
  def User.new_token
    SecureRandom.ulrsafe_base64
  end

  # Remembers a user in the database for use in persistent sessions
  def remember
    self.remember_token = User.new_token
    update_attribute(:remember_digest, User.digest(remember_token))
  end
end
``` 


