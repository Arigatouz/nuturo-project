1) The getElementsByClassName code:

row.getElementsByClassName('delete')[0].addEventListener(...

row is the element that has just been added to the book-list, I am 
saying "Make an array of all the elements inside the row element that have the 
class 'delete'". We know that each row will have only one element that 
actually has a 'delete' class, so it will be the first (and only) element 
inside of this array. I get this element by using [0], and then add the event-
listener to that element.
@Arigatou 
2) why I use .bind(this) for our eventHandler:

When a function is executed, for example:

clearshowAlert();

, the value of 'this' inside the function is set to the window. 

When a method is executed on an object, for example:

UI.showAlert();

, the value of 'this' inside the function is set to the object (UI).


when an eventHandler is fired based on an event, for example:

...addEventListener('click', function(e) {
    console.log(this);
});

, the value of 'this' will be the ELEMENT that has the event-listener!

In our code:

ui.addBookList(); 

is being executed. So, the value of 'this' inside the method is set to ui,
and inside this, we are setting an event-listener. Inside this event-listener, 
we need to use showAlert('book removed' ,'sucsess') which is a method that 
will be found on the ui object.

so we should have: 

...addEventListener('click', function() {
      list.removeChild(row);
      this.showAlert('book removed' ,'sucsess');
      clearshowAlert();
    });

BUT as said above, 'this' inside an eventHandler will be set to the ELEMENT (the 
<a class="delete">X</a> element in this case)

But we need a reference to what 'this' was outside of the eventHandler.

Function.bind() allows to manually set the 'this' value for the code inside 
the function when it gets executed.

I am saying "the current code being executed is based on a method call, so the 
value of 'this' inside the method call is set to ui. Take the value of 'this' 
and manually set it to be the value of 'this' for our eventHandler, so that we 
can use ui.showAlert inside the eventHandler, by using this.showAlert."
@Arigatou 
3.1) It sounds to me that you are a bit uncertain how 
getElementsByClassName and perhaps also querySelector work, this explains how 
they work:

This is a method found on Element objects, and on the document object (same as 
querySelector).

I used the getElementsByClassName method, but I could have easily just used 
the querySelector method instead. They do very similar things, but 
getElementByClassName can only find elements that match a class / set of 
classes, whereas querySelector can search against any css selector. 

querySelecor returns the Element
querySelectorAll returns a NodeList where each node is a matched element
getElementsByClassName returns an HTMLCollection where each item is an HTMLElement

an HTMLCollection is array-like: it is quite similar to an array, but many of 
the methods found on Array.prototype are missing from this HTMLCollection.

for example:

<div id="1" class="btn blue bold">
    <div id="2" class="text bold">
        Hello
    </div>
</div>

var el1 = document.getElementById('1');

document.getElementsByClassName('btn blue') // will get HTMLCollection containing #1
document.getElementsByClassName('bold') // will get HTMLCollection containing #1 & #2
el1.getElementsByClassName('bold') // will get HTMLCollection containing #2

document.querySelector('.bold') // will get Element #1
document.querySelector('.blue .text') // will get Element #2
document.querySelectorAll('.bold') // will get NodeList containing #1 & #2
el1.querySelector('.bold') // will get Element #2
@Arigatou 
3.2) you cannot use any other type of selector within getElementByClassName:
document.getElementsByClassName('btn>text') will fail and probably error out.

So if you want to select an element with a more sophisticated selector, you 
will use querySelector.

There is also the fact that getElementsByClassName has LIVE HTMLElements (if 
the element is changed on in the DOM, its HTMLElement in the HTMLCollection 
will also update).
querySelector methods do NOT return live elements.