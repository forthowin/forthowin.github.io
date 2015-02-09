---
layout: post
title: First Course Completed!
---

I was just able to finish lesson 4 today, thus concluding the first course. Lesson 4 was pretty short compared to the other lessons. Although it was short, I struggled a little due to the concepts that were introduced being new to me. What we did in this lesson was to add javascript into our application, mainly for the player hit and stay button, and the dealer hit button. The reasoning behind why we would want javascript in an application is to create dynamic content for our browser to handle. So instead of sending a request to the server everytime we hit that like button on facebook and having to render the response(the entire page) which can take alot of resources to complete, we just let our client, the browser handle that request just for that like button. We don't need to re-render the entire page. We used a javascript library called JQuery in our application to simplify the way code, such as selecting our DOM elements (the buttons). An example is: 
<div style='white-space: pre-wrap'>
function player_hits(){
  $(document).on("click", "form#hit_form input", function(){
    $.ajax({
      type: "POST",
      url: "/game/player/hit"
    }).done(function(msg){
      $("#game").replaceWith(msg)
    });
    return false;
  });
};
</div>

Here, we easily select the player hit button the same way we do in css by traversing. We call ajax when the button is pushed and it passes 3 parameters, the <strong>type</strong>(or verb, which is the get or post request), <strong>url</strong> that tells it to go to where next, and <strong>data</strong>(not shown since we didn't need to pass anything along). The whole page don't need to be rendered again, since the only change that is being made is in our div tag with the id #game. Finally the return false is there to stop the form from being submitted when we click the hit button.

I said it before and I'll say it again, I loath front end development since I have no artistic sense, so I was glad we didn't go much deeper into this. The exposure was nice though and I learned quite a bit from it. If I were to run into javascript in the future, atleast I will have some experience in it. With this blog, I will be done with the first course, hurray! I will start the next course tomorrow, which will be about rails I think, so I am very excited about that. Also, the updated blackjack game can be found <a href='https://young-anchorage-8578.herokuapp.com/'>here</a> again.
