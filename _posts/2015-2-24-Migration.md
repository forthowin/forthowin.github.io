---
layout: post
title: Migration
---

<p>
Migration allows you to create and alter your database schema. To begin creating a table in the database, type
</p>
<pre>
$ rails generate migration create&lowbar;users
</pre>
<p>
Rails will then generate a file in <code>/db/migrate</code> called something like <code>20150225221319&lowbar;create&lowbar;users.rb</code>. Where does that number come from and what does it mean? When we call <code>generate</code>, rails will generate a file and pre-append a timestamp of creation infront of the rb file name. The significance of it will be explained shortly. The <code>create</code> is optional, but it is a shortcut which tells rails to create a <code>create&lowbar;table :users do end</code> for us. If we don't specify the <code>create</code>, we can easily code it in ourselves, no big deal. So if we were to open up the file we just created, it should look something this:
</p>
<pre>
class CreateUsers &lt; ActiveRecord::Migration
    def change
      create&lowbar;table :users do |t|
      
      end
    end
end
</pre>
<p>
If we wanted our users table to have a column with a username, we can add it in like this:
</P>
<pre>
class CreateUsers &lt; ActiveRecord::Migration
    def change
      create&lowbar;table :users do |t|
        t.string :username
        t.timestamps
      end
    end
end
</pre>
<p>
Our table will now have a column name as <code>username</code> and it's datatype is a string. We can specify other datatypes as <code>t.integer</code> or <code>t.text</code> if we wanted to. <code>t.timestamps</code> will add the <code>created&lowbar;at</code> and <code>updated&lowbar;at</code> column for us as <code>datetime</code> datatype, which will be really useful for us to use. We don't need to fill out these two column, rails will automatically do it for us. In order for us to create the schema, run:
</p>
<pre>
$ rake db:migrate
</pre>
<p>
Which will generate a sqlite3 file in the db folder for us. From here, we can use tools ike SQLite Database Browser to look at our database. Now lets say we want to make a change to our database schema. We want to add a column named age to our schema. You might be tempted to hop into <code>20150225221319&lowbar;create&lowbar;users.rb</code> and add <code>t.integer :age</code> and then <code>rake db:migrate</code>, but that will not work. The way rails handle migration is that during each migration, it will look at the timestamp that is pre-appended infront of the file, and see if that version is ran yet. If it is, it will not run it again. Thats why editing our <code>20150225221319&lowbar;create&lowbar;users.rb</code> will not do anything for us since we already ran <code>rake db:migrate</code> on it already. There are two ways to solve this problem. The first way is to run:
</p>
<pre>
$ rake db:rollback
</pre>
<p>
This will rollback the last migration. The consequence of doing it this way is that the current data will be wiped. Also, if you are working in a team and you have already shared this code with someone, do a rollback, then do another migration, then share your project again, that person on the other end will not know about the rollback and this can cause their database schema to look differently from what you have. So the practice of rollback is dangerous when working in a team. The alternative solution is better in practice and safer. What you will need to do is to create another migration like so:
</p>
<pre>
$ rails generate migration add&lowbar;age&lowbar;to&lowbar;users
</pre>
<p>
The name of the file doesn't matter, as long is it makes sense, in this case we want to add age to the users table. The generate file looks like this <code>20150225225330&lowbar;add&lowbar;age&lowbar;to&lowbar;users.rb</code>. Open it up and type in:
</p>
<pre>
class AddAgeToUsers &lt; ActiveRecord::Migration
    def change
        add&lowbar;column :users, :age, :integer
    end
end
</pre>
<p>
<code> add&lowbar;column</code> will add a new column to our table. The first parameter we pass in specifies which table we want to add a new column to, in this case the <code>:users</code> table. The second parameter is the name of the column, in this case, <code>:age</code>. Finally, we specify the datatype. Run <code>rake db:migrate</code> and our users table will be updated with an age column.
</p>