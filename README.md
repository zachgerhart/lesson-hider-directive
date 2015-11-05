# lesson-hider-directive

Today we will be building a custom directive that checks whether a lesson is already in the provided schedule and striking through that lesson if it is in the schedule. We will cover the fundamentals of directives and demonstrate a real-world use case that will allow you to better understand the power of directives.

## Step 1: Initializing the file structure.
All that is provided is the schedule.json and app.js, so lets start by laying out the skeleton of our app. We will need an 'index.html', 'lessonCtrl.js', 'lessonService.js', and last but not least a 'lessonHider.js'. So lets start out by creating those files and setting up the necessary basics such as initializing the app and controller.

## Step 2: Starting your directive.
Start by creating your directive. You will use the directive method on your angular module (just like you do when creating controllers and services). This method takes two parameters, a string that will be the name of your directive, and a callback function which will return an object. It should look something like this:

```javascript
angular.module('directivePractice')
.directive('lessonHider', function() {
	return {
	
	}
});
```

Starting out looking pretty familiar right? So let's test it and make sure it works! Start out by creating a new file named 'lessonHider.html' and adding some text. Once you have that done we can add a property called ```templateUrl: ``` to our directive's return object. Point the templateUrl to your newly created 'lessonHider.html'. Now all we need to do is add the directive to our 'index.html' to see if it works. Remember that directives are converted to snake-case in html!

If you are not seeing the text in your 'lessonHider.html' it is time to double check your steps. Did you remember to add your 'lessonHider.js' to your 'index.html'? Did you convert your directive to snake-case? Is the file path in your templateUrl property correct?

Your directive in your html should look like this:
```html
<lesson-hider></lesson-hider>
```
It can be helpful to think of a directive as a self contained route, wherever you place that directive it will display the html template you have given it.

## Step 3: Adding functionality
So far our directive is displaying some text, but it isn't doing much else, so lets bring it up a level! Inside your lessonCtrl let's add an array of lessons to your $scope. Give the lessons array these values:

```javascript
['Services', 'Routing', 'Directives', 'Review', 'Firebase', 'No server project', 'Node', 'Express', 'Mongo'];
```

Now inside of our directive's template we can display the array of lessons the same way we would in any of our other html. So lets add the lessons array to the template and reload the page to make sure it prints out what we want it to.

Cool! But still not much that is new, so lets dive a little deeper. Inside of your 'lessonHider.js' add another property to your return object called 'link' and give it the value of a function. The link method will (usually) take three paramaters: 'scope', 'element', and 'attributes'. At this point your directive should look like this:

```javascript
angular.module('directivePractice')
.directive('lessonHider', function() {
	
	return {
		templateUrl: 'lessonHider.html',
		link: function( scope, element, attributes ) {
			
		}
	}

});
```
Unlike dependency injection used elsewhere in Angular these paramater names don't carry specific meaning. The first paramater represents the `$scope` of your directive, the second is the html element that wraps your directive, and the third is an object containing all the properties and values of the attributes on your directive in the DOM.

Remember that data is passed to directives through attributes on the element. So in our html lets add an attribute named `my-test` and give it a value of `Hello there!`. Just like this : `<lesson-hider my-test="Hello there!"></lesson-hider>`

Now inside of your link method lets console log the three paramaters (scope, element, attributes) check out the console to see what we're getting. The scope will log out an object containing angular properties, the element will log out the actual DOM element itself, and you will be able to see that the attributes logs out your myTest attribute and value. You can also see that Angular has done it's magic in converting the snake-case in your html to camelCase.











