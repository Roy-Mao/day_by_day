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
