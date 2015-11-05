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
It can be helpful to think of a directive as a self contained route, wherever you place that directive it will display the html template you have given it. Before moving on to adding functionality, lets add the `restrict` property to our directive's return object. Restrict determines how you can use the directive in your DOM and there are three options: `'E'`, `'A'`, and `'C'`. These stand for Element, Attribute, and Class. A directive that has `restrict: 'E'` (note that 'E' is a string) can only be used in your html as an element, like we have above. Think through Angular's built in directives such as ng-repeat or ng-options. These are used with the `'A'` restriction because they are passed as attributes to existing elements. Directives as classes are less commonly seen and should not be used as they are being phased out of angular. So for now, let us just restrict our lessonHider directive to only be an Element.

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
		restrict: 'E',
		link: function( scope, element, attributes ) {
			
		}
	}

});
```
Unlike dependency injection used elsewhere in Angular these paramater names don't carry specific meaning. The first paramater represents the `$scope` of your directive, the second is the html element that wraps your directive, and the third is an object containing all the properties and values of the attributes on your directive in the DOM.

Remember that data is passed to directives through attributes on the element. So in our html lets add an attribute named `my-test` and give it a value of `Hello there!`. Just like this : `<lesson-hider my-test="Hello there!"></lesson-hider>`

Now inside of your link method lets console log the three paramaters (scope, element, attributes) check out the console to see what we're getting. The scope will log out an object containing angular properties, the element will log out the actual DOM element itself, and you will be able to see that the attributes logs out your myTest attribute and value. You can also see that Angular has done it's magic in converting the snake-case in your html to camelCase.

Once you're done browsing through the results of your console log feel free to remove the console log and my-test attribute before we move on to the next step, we wont be using them anymore.


## Step 4: Isolating scope.
Ideally, directives can be re-used in multiple places throughout a project, one of the greatest benefits of a well written directive is its pluggability. Like a function, you can use the same directive in several different areas for slightly different values. Unfortunately this can lead to issues as you might want to use differently named data in different areas of your project. So saying `{{ lessons }}` might work fine on the current controller, but what if we wanted to use it elsewhere where we didn't have a lessons array?

Angular's work around to this problem lies in something called Isolate Scope, Isolate Scope cuts the directive off from the parent controller (lessonCtrl in this case) and only allows the directive to use data we explicitly pass to it. To isolate scope, all we need to do is add a `scope:` property to our directive's return object and give that `scope:` property the value of an object. Here is what the directive should look like after adding `scope:`:

```javascript
angular.module('directivePractice')
.directive('lessonHider', function() {
	
	return {
		templateUrl: 'lessonHider.html',
		restrict: 'E',
		scope: {

		},
		link: function( scope, element, attributes ) {

		}
	}

});
```
Once you have added the `scope:` property you will see that the directive is no longer displaying `{{ lessons }}`, this is because our directive no longer has access to lessonCtrl and therefore no longer has access to the `$scope.lessons` array. That is ok! We'll fix it soon!

A quick refresher on the different scope options here:
```javascript
scope: {
	twoWayDataBinding: '=',
	stringBinding: '@',
	functionBinding: '&'
}
```
A two way data binding means that our directive and it's parent controller (lessonCtrl in this case) are in constant communication when the value passed in changes. Any changes made will be reflected both in the controller and inside the directive.

A string binding simply tries to grab the string value that is passed to it.

A function binding allows you to pass a function from the controller, into your directive.

So lets give our new `scope:` object a key-value pair with the key being `lesson:` and the value being `'='`. What we are doing here is telling our directive that we plan on having an attribute named `lesson` and we want to create a two way data binding between our directive and the parent controller on whatever value is passed to that attribute.To demonstrate this two way binding we first need to pass our directive a `lesson` attribute and give that attribute a value.

In our lessonCtrl lets create a new property on the `$scope` and call it 'test', we will give this a value of 'Two way data binding!'. Now let's add `{{ test }}` to a new `<div>` inside our index.html, you should see your page update with 'Two way data binding!'. To get this to display inside of our directive as well, we need to add an attribute to our directive inside our index.html. So lets add the `lesson` attribute to our directive and pass it the value `"test"`. If you don't remember how to do this, look back to the previous section when you used the `my-test` attribute.

Now all we need to do to display our string is add it to the lessonHider template. So inside of lessonHider.html lets add `{{ lesson }}` and you should see a second 'Two way data binding!' show up on your page. The reason we use 'lesson' here instead of 'test' is because our directive can no longer talk to our controller, it only knows the values we give it, and we have passed `$scope.test` to our directives `lesson` attribute, so we can only access it by `lesson`.








