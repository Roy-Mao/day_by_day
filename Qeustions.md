# Understanding protect_from_forgerey with::exception

Location: app/controllers/application_controller.rb

Context:
```ruby
  class ApplicationController < ActionController::Base
    protect_from_forgery with::exception
    include SessionHelper
  end
```

Specifics:
1. Why should we include this line in the ApplicationController?
2. What is a forgery?
3. What with::exception refers to?


# There is one paragraph I can hardly understand in Chapter 9, which goes as follows:

> After logging in, we can check if the user has been remembered by looking for the remember_token key in the cookies. Ideally, we would check that the cookie’s value is equal to the user’s remember token, but as currently designed there’s no way for the test to get access to it: the user variable in the controller has a remember token attribute, but (because remember_token is virtual) the @user variable in the test doesn’t. Fixing this minor blemish is left as an exercise (Section 9.3.1.1), but for now we can just test to see if the relevant cookie is nil or not.

Question: What does this paragrah mean exactly? I understand that the user varibale in the controller has a remember token attribute, and the @user in variable in the test does not. But I can not understand **(because remember_token is virtual)**, where does the word **[virtual]** come from? The following paragraph suggests just to change the variable in create method of User model to @user, indicating that user is a local variable while @user is an instance variable. Once it is an instance variable, it will be accessible in the test code by the **assign()** method, passing the instance variable as an symbol argument, like assign(:user).

This particular part is worth of looking back when I finished reading this bool.
