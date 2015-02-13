---
layout: post
title: Quiz 1
---

<strong>1. Why do they call it a relational database?</strong>
<p>
  They call it a relational database because it is organized in such a way that we can access and modify the data withought having to reorganize it. A database can have different groups of data called a table. Each table consists of rows and column as data. The rows are data for that particular entry. The columns are the attribute values for that entry. Upon creation of each row, a unique id is given to it. With these unique id, other table can relate to it using an attribute called forgein key.
</p>

<strong>2. What is SQL?</strong>
<p>
  SQL stands for Structured Query Language. SQL allows us to access and manipulate databases. We can do things like select, insert, update, and delete data.
</p>

<strong>3. There are two predominant views in a relational database. What are they, and how are they different?</strong>
<p>
  The two predominant views in in a relational database are schema view and data view.
<strong>schema view</strong> shows us the data type of each attribute of a table in a database
<strong>data view</strong> shows us the actual data in a table in a database
</p>

4. In a table, what do we call the column that serves as the main identifier for a row of data? We're looking for the general database term, not the column name.
Primary key

5. What is a foreign key, and how is it used?
A foregin key is a field in a table of a relational database. It is used to form an association between data in another table by matching with a primary key.

6. At a high level, describe the ActiveRecord pattern. This has nothing to do with Rails, but the actual pattern that ActiveRecord uses to perform its ORM duties.
Active record is an abstraction of a database. It does this by wrapping a database table into a class. This means that each row in a table is an object instance. Any object that gets loaded with retrieve information from the database. When an object is updated, its corresponding row in the table is also updated. The wrapper class also implements accessor methods or properties for each column in the table.

7. If there's an ActiveRecord model called "CrazyMonkey", what should the table name be?
The table name should be <bold>crazy_monkeys</bold>. You can use rails method .tableize to help you determine the table name.

8. If I'm building a 1:M association between Project and Issue, what will the model associations and foreign key be?
The model association will be:
<div style='white-space: pre-wrap'>
class Project < ActiveRecord::Base
  has_many :issues
end

class Issue < ActiveRecord::Base
  belongs_to :project, foreign_key: "project_id"
end
</div>

9. Given this code
<div style='white-space: pre-wrap'>
class Zoo < ActiveRecord::Base
  has_many :animals
end
</div>

  1. What do you expect the other model to be and what does database schema look like?
    The other model should look like:
    <div style='white-space: pre-wrap'>
    class Animal < ActiveRecord::Base
      belongs_to :zoo
    end
    </div>

    The database schema should look like:
    <div style='white-space: pre-wrap'>
    class CreateAnimals < ActiveRecord::Migration
      def change
        create_table :animals do |animal|
          t.string :name
          t.integer :zoo_id
      end
    end

    class CreateZoos < ActiveRecord::Migration
      def change
        create_table :zoos do |zoo|
          t.string :name
      end
    end
    </div>

  2. What are the methods that are now available to a zoo to call related to animals?
    zoo.animals, zoo.animals.name, zoo.animals.zoo_id

  3. How do I create an animal called "jumpster" in a zoo called "San Diego Zoo"?
    $ zoo = Zoo.create(name: "San Diego Zoo")
    $ Animal.create(name: "jumpster", zoo_id: zoo.id)

10. What is mass assignment? What's the non-mass assignment way of setting values?
Mass assignment allows you to assign values to an object as you're creating it (see question 9.3). A non-mass assignment way of setting values would be to create an empty object first, then assigning values afterwards. An example would be, 
<div style='white-space: pre-wrap'>
$ animal = Animal.create
$ animal.name = "jumpster"
$ animal.zoo_id = 1
</div>

11. What does this code do? Animal.first
It retrieves the first object or row in the 'animals' table.

12. If I have a table called "animals" with columns called "name", and a model called Animal, how do I instantiate an animal object with name set to "Joe". Which methods makes sure it saves to the database?
<div style='white-space: pre-wrap'>
$ animal = Animal.new(name: "Joe")
$ animal.save

$ Animal.create(name: "Joe")
</div>
Using new only creates a new object in memory. In order to save it to the database, you need to call save to that object. Or, you can call the create method, which will save it into the database.

13. How does a M:M association work at the database level?
The M:M association work by having a join table. Both the primary model will have a 1:M association with this join table. The two primary model will then have an assocation with each other by have a has_many through association.

14. What are the two ways to support a M:M association at the ActiveRecord model level? Pros and cons of each approach?
has_many through requires an explicit join model and a join table, but it is more flexible and we can add additional attributes to the join table. 
has_and_belongs_to_many doesn't require a join model but still requires a join table, but it is less flexible and we cannot add additional attributes to the join table.

15. Suppose we have a User model and a Group model, and we have a M:M association all set up. How do we associate the two?
<div style='white-space: pre-wrap'>
class User < ActiveRecord::Base
  has_many :user_groups
  has_many :groups, through: user_groups
end

class UserGroup < ActiveRecord::Base
  belongs_to :user
  belongs_to :group
end

class Group < ActiveRecord::Base
  has_many :user_groups
  has_many :users, through: user_groups
end
</div>