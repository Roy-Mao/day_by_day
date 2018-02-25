# Self disambiguation in Ruby

```ruby
 class Post
   attr_accessor :title
   def replace_title(new_title)
     # Here title is a local variable. That is why in the below cade, print_title does not print out the value of "replace_title"
     # If we do wanna replace the value, we should use self.replace_title instead
     title = new_title
   end
   def print_title
     # Here it first looks for if there is a local variable called title
     # Then it assumes 'title' to be an instance method and called on 'self' implicitly, aka self.title
     puts title
   end
 end

pst = Post.new
pst.title = "Cream of Broccoli"
pst.replace_title("Cream of Spinach")
pst.print_title
# The output result is Cream of Broccoli
```