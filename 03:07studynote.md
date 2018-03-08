# Generated files among model, controller, resouce and scaffold.

Why I write this post? The generator commands in Rails is rather confusing. Should we capitalize the firt letter of a model? Should it be singular form or plural form? There is no standarlized way of naming controllers, models, migration etc in Ruby. This is my version of ruby generator naming conventions. It helps me to clear my mind when writing my own Ruby code.

Do mind that do not abuse the use of Generator in Ruby, and [here](https://github.com/learn-co-curriculum/rails-generators-readme) is a great post to explain why, and why Controller Generator is less preferable to the Resource Generator.

# Generated files among model, resouce and scaffold.
 
## What files do the following ccommands generate?
 
 1. rails g model Test name:text
 2. rails g controller Tests new
 3. rails g resource Test name:text
 4. rails g scaffold Test name:text
 
 
注意 : The above commands, along with [rails g migration NAME ......], only creates or alters the table in the migration file. However, it does not actually add the table to the database schema. To actually apply the changes to the database schema, we should use the command either rake db:migrate or rails db:migrate

 
 ## Answers to the generated files
 
 1. model generates two files:
 
What is the Model used for? For creating the object and ORM (associating with the database table). Therefore, the a Model file and a Migration file should be created. Because the Model is used to generate one single instance. Therefore, it is better to write the singular Test instead of Tests

   * A Model file, in which the class is derived from the ActiveRecord::Base superclass
 
  ```ruby
     class Test < ActiveRecord::Base
     end
   ```

name:text: what this line is doing is 'migration fileの中身を指定する', which is used to generate the migration file. However, the name of the table in data base is in its plural form, which is Tests in stead of Test.

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

 
2. controller generates three files:

 What is a controller used for? It is the center of the MVC model, bridging the incoming request, retrieving the info from the database and using the retrived info to render the view. Therefore, this command should generate at least a view file, a controller file with the defined actions like new, open and a Restful route such as tests#open or tests#new. Because controller is responsible for handlig so many incoming requests, it is better to use the plural form Tests instead of the singular Test.Because it is TestsController < ApplicationController. The routes is based on the name defined used in the controller, the route name shoud be tests#new.

* A controller file 

```ruby
class TestsController < ApplicationController
  def new
  end
  def create
  end
  def destroy
  end
end 
```

* The RESTful route definede in the file routes.rb

the route should be tests#new

* The correspoinding views in the view directory


3. resource generates 4 files:
 
    * A Model file as above
 
    * A Migration file as above
 
  	* A Controller file, in which the class is derived from ApplicationController
 
  	```ruby
  	  class TestsController < ApplicationController
  	  end
  	```
 
  	* a [resources :tests] routes in your routes.rb file
 
4. scaffold generates 5 files:
 
  	* A Model file as above
 
  	* A resources :tests routes in your routes.rb file
 
  	* 7 corresponding view files in your view directory
   
