---
layout: post
title: Quiz 3
---

<strong>1. What's the difference between rendering and redirecting? What's the impact with regards to instance variables, view templates?</strong>
<p>
  Rendering displays a view template, while redirecting sends a status code of 302 back to the client along with the url on where to redirect to. Rendering requires an html.erb file, while redirecting requires a URL as input. Because redirecting requires the client to send another request, instance variables will not be saved. Rendering requires instance variables to be declared in the action before the view template can be used.
</p>

<strong>2. If I need to display a message on the view template, and I'm redirecting, what's the easiest way to accomplish this?</strong>
<p>
  Use <code>flash</code> which saves the message for the next request and for one session like so:
<pre>
  flash[:notice] = 'This message will show up on the next viewing template'
  redirect&lowbar;to root&lowbar;path
</pre>
</p>

<strong>3. If I need to display a message on the view template, and I'm rendering, what's the easiest way to accomplish this?</strong>
<p>
  Use <code>flash.now</code> which will display a message on on the view template when rendering, and won't persist on the next request.
</p>

<strong>4. Explain how we should save passwords to the database.</strong>
<p>
  To save passwords to the database, first install the bycrypt gem, which will create a one way hash of the saved password as a long string. Generate a migration with <code>password&lowbar;digest</code> as a column. Then add <code>has&lowbar;secure&lowbar;password</code> to the model with <code>password&lowbar;digest</code>, which will help with autenticating a BCrypt password.
</p>

<strong>5. What should we do if we have a method that is used in both controllers and views?</strong>
<p>
  We can extrapolate the method into <code>application&lowbar;controller.rb</code> and call <code>helper&lowbar;method :a&lowbar;method</code> in order for the view to access.
</p>

<strong>6. What is memoization? How is it a performance optimization?</strong>
<p>
  Memorization allows us to set a variable to something if it is nil. If it is not nil, then it will set the variable as itself. For example:
</p>
<pre>
current&lowbar;user ||= User.find(params[:id])
</pre>
<p>
  This is a performance optimization because we don't have to access the database every time to set the <code>current&lowbar;user</code> if it exists.
</p>

<strong>7. If we want to prevent unauthenticated users from creating a new comment on a post, what should we do?</strong>
<p>
  We can create a method in <code>application&lowbar;controller.rb</code> that checks if a user is logged in or not. If they are not logged in, display a message. We can call this method in <code>comments&lowbar;controller.rb</code> by calling <code>before&lowbar;method :application&lowbar;controller&lowbar;method</code> which will run the method everytime before an action is executed. Another way is to check if the user is logged in the view template and display the comment field only if they are. <code>helper&lowbar;method :method</code> must be included in the <code>application&lowbar;controller.rb</code> in order for the view template to use.
</p>

<strong>8. Suppose we have the following table for tracking "likes" in our application. How can we make this table polymorphic? Note that the "user&lowbar;id" foreign key is tracking who created the like.</strong>
<table class='table table-bordered'>
  <tr>
    <td>id</td>
    <td>user&lowbar;id</td>
    <td>photo&lowbar;id</td>
    <td>video&lowbar;id</td>
    <td>post&lowbar;id</td>
  </tr>
  <tr>
    <td>1</td>
    <td>4</td>
    <td> </td>
    <td>12</td>
    <td> </td>
  </tr>
  <tr>
    <td>2</td>
    <td>7</td>
    <td> </td>
    <td> </td>
    <td>3</td>
  </tr>
  <tr>
    <td>3</td>
    <td>2</td>
    <td>6</td>
    <td> </td>
    <td> </td>
  </tr>
</table>
<hr/>
<table class='table table-bordered'>
  <tr>
    <td>id</td>
    <td>user&lowbar;id</td>
    <td>likeable&lowbar;type</td>
    <td>likeable&lowbar;id</td>
  </tr>
  <tr>
    <td>1</td>
    <td>4</td>
    <td>Video</td>
    <td>12</td>
  </tr>
  <tr>
    <td>2</td>
    <td>7</td>
    <td>Post</td>
    <td>3</td>
  </tr>
  <tr>
    <td>3</td>
    <td>2</td>
    <td>Photo</td>
    <td>6</td>
  </tr>
</table>

<strong>9. How do we set up polymorphic associations at the model layer? Give example for the polymorphic model (eg, Vote) as well as an example parent model (the model on the 1 side, eg, Post).</strong>
<pre>
class Vote &lt; ActiveRecord::Base
    belongs&lowbar;to :voteable, polymorphic: true
end
</pre>

<pre>
class Post &lt; ActiveRecord::Base
    has&lowbar;many :votes, as: :voteable
end
</pre>

<strong>10. What is an ERD diagram, and why do we need it?</strong>
<p>
  An ERD diagram stands for an Entity-Relationship Diagram. An ERD diagram is like a blueprint to help build our models and show their association with eachother. It is very important and helpe in determining how we should set up our models and we can refer to it during development. It includes a table with the model name and the names of the column along with the data type. Each table will point to another table to show their association.
</p>