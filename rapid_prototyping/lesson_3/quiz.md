1. What's the difference between rendering and redirecting? What's the impact with regards to instance variables, view templates?

    Redirecting will issue a new http request to the specified route. Rendering displays a template using any instance variables that were set up in the controller action. Once a redirect occurs, those instance variables that were prevously declared will not be available.

2. If I need to display a message on the view template, and I'm redirecting, what's the easiest way to accomplish this?
    
    Use `flash`, which will save the message in the session.

3. If I need to display a message on the view template, and I'm rendering, what's the easiest way to accomplish this?
    
    Use `flash.now`. This will display the message once the template is rendered.

4. Explain how we should save passwords to the database.

    Passwords should be saved by using a one way hashing algorithm. The passwords will be saved in the database, but will be saved as a token. In Rails, this can be accomplished by using the `has_secure_password`functionality in a user model. In the database, a `password digest` column will be used to store the passwords. The `bcrypt-ruby` gem is used to process the password strings to be authenticated. 

5. What should we do if we have a method that is used in both controllers and views?
    
    It should be defined in `applicationController.rb` and referenced in the
     `helper_method` option.


6. What is memoization? How is it a performance optimization?
    
    Memoization is the technique of using a method to set a variable. It will first check if the variable is set, if not, it will run the specified code to set the variable. This can be a performance boost if the database needs to be queried in order to set the value. In our example, a `current_user` method is used to set a `@current_user` instance variable. Since there are many times in our application that we need to check if there is a currrent user, this will prevent a database query from being run during the current session.
    
7. If we want to prevent unauthenticated users from creating a new comment on a post, what should we do?
    
    In the application controller, create a `logged_in?` method that can be used in the views (remember to specify this as a `helper_method`). In the view, use this method to prevent the form from being shown. Also, there should be a `before_action` in the corresponding controller to prevent the comment from being created, and display an error message and redirect

8. Suppose we have the following table for tracking "likes" in our application. How can we make this table polymorphic? Note that the "user_id" foreign key is tracking who created the like.

    ``` ruby
    class CreateLikes < ActiveRecord::Migration
      def change
        create_table :likes do |t|
          t.integer :user_id
          t.string :likeable_type
          t.integer :likeable_id

          t.timestamps
      end
    end
    ```
    
    there is also a shorthand version by using `t.references`

    ``` ruby
    class CreateLikes < ActiveRecord::Migration
      def change
        create_table :likes do |t|
          t.integer :user_id
          t.references :likeable, polymorphic: true

          t.timestamps
      end
    end
    ```

    this will create a table that looks like this:

    | id | user_id | likeable_type | likeable_id |
    | ---| ------- | ------------- | ----------- |
    | 1 | 4 | Video | 12 |
    | 2 | 7 | Post | 3 |
    | 3 | 2 | Photo | 6 |



9. How do we set up polymorphic associations at the model layer? Give example for the polymorphic model (eg, Vote) as well as an example parent model (the model on the 1 side, eg, Post).

    First, we need to set up the model for a Vote and specify that it is polymorphic.

    ```ruby
    class Vote < ActiveRecord::Base
      belongs_to :user 
      belongs_to :voteable, polymorphic: true
    end
    ```

    Assuming we have a `User` model that will be voting on other models. This will capture the 1:M relationship between `User` and `Vote`

    ```ruby
    class User < ActiveRecord::Base
      has_many :votes
    end
    ```

    Last, all models that can be voted on need to set up the 1:M relationship as well as specify the polymorphic relationship.
    
    ```ruby
    class Post < ActiveRecord::Base
      has_many :votes, as: :voteable
    end
    ```


10. What is an ERD diagram, and why do we need it?

    An ERD is an Entity Relationship Diagram. It is a way to visually map out models that will be used in an application. During development, this can be a very helpful reference when creating the database schema and model associations.