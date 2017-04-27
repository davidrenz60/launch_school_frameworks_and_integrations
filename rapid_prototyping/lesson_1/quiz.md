1. Why do they call it a relational database?

    A relational database orders data into tables composed of rows and columns. What makes a database relational are the relationships between data in different tables.

2. What is SQL?

    SQL stands for Structured Query Language. It is a domain specific language designed to interact with a relational database.

3. There are two predominant views into a relational database. What are they, and how are they different?

    Schema view: Shows how the database is setup. It will show what columns are available and what type of data is permitted. It does not show any actual rows of data.

    Data view: Will show actual rows of data in each table.

4. In a table, what do we call the column that serves as the main identifier for a row of data? We're looking for the general database term, not the column name.
    
    The primary key. 

5. What is a foreign key, and how is it used?
    
    A foriegn key is used to relate data between two models. When an association is made between two models, a foriegn key on one object will reference the primary key on another object. The foreign key will always be on the 'many' side of the association

6. At a high level, describe the ActiveRecord pattern. This has nothing to do with Rails, but the actual pattern that ActiveRecord uses to perform its ORM duties.

    ActiveRecord is a way to interact with a databse using Ruby code. ActiveRecord models are a way to access and manipulate data as well as track associations between other models. A class of an ActiveRecord corresponds to a table in the database. An object of this class corresponds to a row of data in the table

7. If there's an ActiveRecord model called "CrazyMonkey", what should the table name be?
    
    "crazy_monkeys"
    Note: call the `tableize` method to figure out table name


8. If I'm building a 1:M association between Project and Issue, what will the model associations and foreign key be?
    
    In models/project.rb
    ```ruby
    class Project < ActiveRecord::Base
      has_many :issues
    end
    ```

    In models/issue.rb
    ```ruby
    class Issue < ActiveRecord::Base
      belongs_to :project
    end
    ```

    `issues` table will have a foreign key named `project_id`

9. Given this code

    ``` ruby
    class Zoo < ActiveRecord::Base
      has_many :animals
    end
    ```

    What do you expect the other model to be and what does database schema look like?

    The other model should be `Animal`
    
    `animals` table:
    
    zoo_id (foreign key, integer)
    id (primary key, integer)

    `zoos` table:
    
    id (primary key, integer)

    What are the methods that are now available to a zoo to call related to animals?

    `zoo.animals`
    `zoo.animals.first`
    `zoo.animals.each`

    How do I create an animal called "jumpster" in a zoo called "San Diego Zoo"?
 
    ```ruby
    Zoo.create(name: "San Diego Zoo")  
    Animal.create(name: "jumpster", zoo_id: 1)  # assuming new id is 1
    ```

10. What is mass assignment? What's the non-mass assignment way of setting values?
    
    Mass assignment is a way of setting attributes of an ActiveRecord model by passing in multiple key-value pairs as a hash. Most of the time this is done by setting a virtual attribute. A virtual attribute is one that is not explicitly defined in a table, but is created through an ActiveRecord association.

    An example would be using the create method as used above with creating a new `Animal` instance

    To not use mass assignment you would have to change each individual attribute in the table schema.

    ```ruby
    animal = Animal.new
    animal.name = "jumpster"
    animal.zoo_id = 1
    ```

11. Suppose Animal is an ActiveRecord model. What does this code do? `Animal.first`
    
    This will return the first record in the `animals` table

12. If I have a table called "animals" with a column called "name", and a model called Animal, how do I instantiate an animal object with name set to "Joe". Which methods makes sure it saves to the database?

    ```ruby
    animal = Animal.new(name: "Joe")  # will not save, need to call save method
    animal.save 
    ```

    ```ruby
    Animal.create(name: "Joe") # this will save to db
    ```


13. How does a M:M association work at the database level?

    A M:M association means that both models have a has_many relationship with each other. In the databse, this can be modeled by a join table, where both models' foreign keys can be recorded

14. What are the two ways to support a M:M association at the ActiveRecord model level? Pros and cons of each approach?
    
    `has_many :through`
    In this case, a join model is used to set up the association. The advantage is that the join model can add additional attributes. The downside is that there is an extra model to add

    `has_and_belongs_to_namy`
    This will set up an implicit join table in the db. You do not need to set up a join model. However, because of this, no additional attributes can be added to the join table.


15. Suppose we have a User model and a Group model, and we have a M:M association all set up. How do we associate the two?

    It's best to use a `has_many :through` association. This will require setting up a join model, which in this case should be named `UserGroup`

    models/user.rb
    ```ruby
    class User < ActiveRecord::Base
      has_many :user_groups
      has_many :groups, through: :user_groups
    end
    ```

    models/group.rb
    ```ruby
    class Group < ActiveRecord::Base
      has_many :user_groups
      has_many :users, through: :user_groups
    end
    ```

    models/user_group.rb
    ```ruby
    class UserGroup < ActiveRecord::Base
      belongs_to :user
      belongs_to :group
    end
    ```
    
