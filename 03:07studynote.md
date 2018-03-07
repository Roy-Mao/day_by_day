# Generated files among model, resouce and scaffold.

## What files do the following ccommands generate?

1. rails g model Test name:text
2. rails g resource Test name:text
3. rails g scaffold Test name:text

## Answers to the generated files

1. model generates two files:

  * A Model file, in which the class is derived from the ActiveRecord::Base superclass

  ```ruby
    class Test < ActiveRecord::Base
    end
  ```

  * A Migration file, in which the class is derived from the ActiveRecord::Migration superclass

  ```ruby
    class CreateTests < ActiveRecord::Migration
      def change
        creat_table :tests do |t|
          t.text :name
          t.timestamps
        end
      end
    end
  ```

  2. resource generates 4 files:

    * A Model file as above

    * A Migration file as above

 	* A Controller file, in which the class is derived from ApplicationController

 	```ruby
 	  class TestsController < ApplicationController
 	  end
 	```

 	* a resources :tests routes in your routes.rb file

 	3. scaffold generates 5 files:

 	* A Model file as above

 	* A Migration file as above

 	* A Controller file with 5 public methods and 2 private methods as above

 	* A resources :tests routes in your routes.rb file

 	4. 7 corresponding view files in your view directory