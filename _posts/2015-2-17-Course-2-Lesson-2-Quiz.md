---
layout: post
title: Quiz 2
---

<strong>1. Name all the 7 (or 8) routes exposed by the resources keyword in the routes.rb file. Also name the 4 named routes, and how the request is routed to the controller/action.</strong>
<pre>
  get '&sol;posts', to: posts#index             => posts&lowbar;path
  get '&sol;posts&sol;:id, to: posts#show         => post&lowbar;path(:id)
  get '&sol;posts&sol;new', to: posts#new       => new&lowbar;post&lowbar;path
  post '&sol;posts, to: posts#create
  get  '&sol;post&sol;:id&sol;edit, to: posts#edit      => edit&lowbar;post&lowbar;path(:id)
  put '&sol;posts&sol;:id', to: posts#update
  patch '&sol;posts&sol;:id', to: posts#update
  delete '&sol;posts&sol;:id', to: posts#destroy
</pre>

<strong>2. What is REST and how does it relate to the resources routes?</strong>
<p>
  Rest stands for representational state transfer is a software architecture style consisting of guidlines and best practices for creating scalable webservices. RESTful systems most of the times uses HTTP verbs (GET, POST, PUT/PATCH, DELETE, etc.) to retreive web pages and send data to remote servers. Data is treated as resourcess, meaning that RESTful applications can create, retrieve, update, and destroy (CRUD) data. When we use resources routes, we are mapping the HTTP verbs and URLs to the controller action of our app to manipulate data.
</p>

<strong>3. What's the major difference between model backed and non-model backed form helpers?</strong>
<p>
  The major difference is that model backed form helpers are binded to an object. There has to be a setter method, vitrual attribute, or a column in the database inorder to use model backed form helpers. The non-model backed form helpers has more felxibility in that it doesn't have this constraint. Most of the time, we want to use model backed form helpers when we create, edit, or update an object. 
</p>

<strong>4. How does <cdoe>form&lowbar;for</code> know how to build the &lt;form&gt; element?</strong>
<p>
  <code>form&lowbar;for</code> knows how to build the &lt;form&gt; element based on the object and what we spcify it to. For example:
</p>
<pre>
&lt;%= form&lowbar;for @post do |f| %&gt;
  &lt;%= f.label :title %&gt;
  &lt;%= f.text_field :title %&gt;
&lt;% end %&gt;
</pre>
<p>
  Will build a form for the post object with a title label and a input text field.
</p>

<strong>5. What's the general pattern we use in the actions that handle submission of model-backed forms (ie, the create and update actions)?</strong>
<pre>
  def create
      @post = Post.new(params.require(:post).permit(:url, :title, :description))
      if @post.save
          flash[:notice] = "Your post has been created"
          redirect&lowbar;to posts&lowbar;path
      else
          render :new
      end
  end
</pre>

<pre>
  def update
      @post = Post.find(params[:id])
      if @post.update(params.require(:post).permit(:url, :title, :description))
          flash[:notice] = "Your post has been updated"
          redirect&lowbar;to post&lowbar;path(@post)
      else
          render :edit
      end
  end
</pre>

<strong>6. How exactly do Rails validations get triggered? Where are the errors saved? How do we show the validation messages on the user interface?</strong>
<p>Rails validations get triggered when the data submissions try to hit the database. You can tell when there is a rollback after an attempt to save the data. The errors are saved on the object itself. We can view the errors by looking into <code>object.errors</code>. We can also display the errors by using object.errors.full_messages. A nice way to display the error is:
</p>
<pre>
&lt;% if obj.errors.any? %&gt;
    &lt;div class = 'row'&gt;
        &lt;div class='alert alert-error span8'&gt;
            &lt;h5&gt;There were some errors:&lt;/h5&gt;
            &lt;ul&gt;
                &lt;% obj.errors.full&lowbar;messages.each do |msg| %&gt;
                    &lt;li&gt;&lt;%= msg %&gt; &lt;/li&gt;
                &lt;% end %&gt;
            &lt;/ul&gt;
        &lt;/div&gt;
    &lt;/div&gt;
&lt;% end %&gt;
</pre>

<strong>7. What are Rails helpers?</strong>
</p>
  Rails helpers allows us to write our application's logic and formmating in one place so that we can then use to properly display information in our views. By putting our helper methods in the 'application_helper.rb' file, we can use it in our views without convoluting the view files with logic code.
</p>

<strong>8. What are Rails partials?</strong>
<p>
  Rails partials are a group of html code saved to file that can be used amongst all the views. The file name begins with an underscore, such as <code>&lowbar;errors.html.erb</code>. 
</p>

<strong>9. When do we use partials vs helpers?</strong>
<p>We put HTML code to be shared between other views into partials. We put logical codes that our views will use into helpers.</p>

<strong>10. When do we use non-model backed forms?</strong>
</p>We use non-model backed forms when we don't want our forms to be tied to a model object.<p>