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
<code>schema view</code> shows us the data type of each attribute of a table in a database.</br>
<code>data view</code> shows us the actual data in a table in a database.
</p>

<strong>4. In a table, what do we call the column that serves as the main identifier for a row of data? We're looking for the general database term, not the column name.</strong>
<p>Primary key</p>

<strong>5. What is a foreign key, and how is it used?</strong>
<p>
  A foregin key is a field in a table of a relational database. It is used to form an association between data in another table by matching with a primary key.
</p>

<strong>6. At a high level, describe the ActiveRecord pattern. This has nothing to do with Rails, but the actual pattern that ActiveRecord uses to perform its ORM duties.</strong>
<p>
  Active record is an abstraction of a database. It does this by wrapping a database table into a class. This means that each row in a table is an object instance. Any object that gets loaded with retrieve information from the database. When an object is updated, its corresponding row in the table is also updated. The wrapper class also implements accessor methods or properties for each column in the table.
</p>

<strong>7. If there's an ActiveRecord model called "CrazyMonkey", what should the table name be?</strong>
<p>
  The table name should be "crazy_monkeys". You can use rails method <code>CrazyMonkey.tableize</code> to help you determine the table name.
</p>

<strong>8. If I'm building a 1:M association between Project and Issue, what will the model associations and foreign key be?</strong>
<p>The model association will be:</p>
<pre>
class Project &lt; ActiveRecord::Base
  has_many :issues
end
</pre>

<pre>
class Issue &lt; ActiveRecord::Base
  belongs_to :project, foreign_key: "project_id"
end
</pre>

<strong>9. Given this code</strong>
<pre>
class Zoo &lt; ActiveRecord::Base
  has_many :animals
end
</pre>
<ul>
<li><strong>What do you expect the other model to be and what does database schema look like?</strong></li>
    <p>The other model should look like:</p>
<pre>
class Animal &lt; ActiveRecord::Base
  belongs_to :zoo
end
</pre>
    <p>The database schema should look like:</p>
<pre>
class CreateAnimals &lt; ActiveRecord::Migration
  def change
    create_table :animals do |animal|
      t.string :name
      t.integer :zoo_id
    end
  end
end
</pre>

<pre>
class CreateZoos &lt; ActiveRecord::Migration
  def change
    create_table :zoos do |zoo|
      t.string :name
    end
  end
end
</pre>

  <li><strong>What are the methods that are now available to a zoo to call related to animals?</strong></li>
    <p><code>zoo.animals</code>, <code>zoo.animals.name</code>, <code>zoo.animals.zoo_id</code></p>

  <li><strong>How do I create an animal called "jumpster" in a zoo called "San Diego Zoo"?</strong></li>
<pre>$ zoo = Zoo.create(name: "San Diego Zoo")
$ Animal.create(name: "jumpster", zoo_id: zoo.id)
</pre>
</ul>

<strong>10. What is mass assignment? What's the non-mass assignment way of setting values?</strong>
<p>
  Mass assignment allows you to assign values to an object as you're creating it (see question 9.3). A non-mass assignment way of setting values would be to create an empty object first, then assigning values afterwards. An example would be,
</p>
<pre>
$ animal = Animal.create
$ animal.name = "jumpster"
$ animal.zoo_id = 1
</pre>

<strong>11. What does this code do? <code>Animal.first</code></strong>
<p>It retrieves the first object or row in the 'animals' table.</p>

<strong>12. If I have a table called "animals" with columns called "name", and a model called Animal, how do I instantiate an animal object with name set to "Joe". Which methods makes sure it saves to the database?</strong>
<pre>
$ animal = Animal.new(name: "Joe")
$ animal.save
</pre>

<pre>
$ Animal.create(name: "Joe")
</pre>
<p>
  Using <code>new</code> only creates a new object in memory. In order to save it to the database, you need to call <code>save</code> to that object. Or, you can call the <code>create</code> method, which will save it into the database.
</p>

<strong>13. How does a M:M association work at the database level?</strong>
<p>
  The M:M association work by having a join table. Both the primary model will have a 1:M association with this join table. The two primary model will then have an assocation with each other by have a <code>has&lowbar;many through</code> association.
</p>

<strong>14. What are the two ways to support a M:M association at the ActiveRecord model level? Pros and cons of each approach?</strong>
<p>
<code>has&lowbar;many through</code> requires an explicit join model and a join table, but it is more flexible and we can add additional attributes to the join table.<br/>
<code>has&lowbar;and&lowbar;belongs&lowbar;to&lowbar;many</code> doesn't require a join model but still requires a join table, but it is less flexible and we cannot add additional attributes to the join table.
</p>

<strong>15. Suppose we have a User model and a Group model, and we have a M:M association all set up. How do we associate the two?</strong>
<pre>
class User &lt; ActiveRecord::Base
  has_many :user_groups
  has_many :groups, through: user_groups
end
</pre>

<pre>
class UserGroup &lt; ActiveRecord::Base
  belongs_to :user
  belongs_to :group
end
</pre><br/>
<pre>
class Group &lt; ActiveRecord::Base
  has_many :user_groups
  has_many :users, through: user_groups
end
</pre>