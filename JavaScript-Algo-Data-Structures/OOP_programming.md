# Object Oriented Programming
  ## Create a Basic JavaScript Object
     These qualities, or properties, define what makes up an object. Note that       similar objects share the same properties, but may have different values for those properties. For example, all cars have wheels, but not all cars have the same number of wheels.

     Objects in JavaScript are used to model real-world objects, giving them properties and behavior just like their real-world counterparts. Here's an example using these concepts to create a duck object:
   ### Example
     let duck = {
       name: "Aflac",
       numLegs: 2
     };
     This duck object has two property/value pairs: a name of "Aflac" and a numLegs of 2.
  ## Create a Method on an Object
     Objects can have a special type of property, called a method.

     Methods are properties that are functions. This adds different behavior to an object. Here is the duck example with a method:
   ### Example 
     let duck = {
       name: "Aflac",
       numLegs: 2,
       sayName: function() {return "The name of this duck is " + duck.name + ".";}
     };
     duck.sayName();
     // Returns "The name of this duck is Aflac."
     The example adds the sayName method, which is a function that returns a sentence giving the name of the duck. Notice that the method accessed the name property in the return statement using duck.name. The next challenge will cover another way to do this.
  ## Verify an Object's Constructor with instanceof
     Anytime a constructor function creates a new object, that object is said to be an instance of its constructor. JavaScript gives a convenient way to verify this with the instanceof operator. instanceof allows you to compare an object to a constructor, returning true or false based on whether or not that object was created with the constructor. Here's an example:
   ### Example
     let Bird = function(name, color) {
       this.name = name;
       this.color = color;
       this.numLegs = 2;
     }

     let crow = new Bird("Alexis", "black");

     crow instanceof Bird; // => true
     If an object is created without using a constructor, instanceof will verify that it is not an instance of that constructor:

     let canary = {
       name: "Mildred",
       color: "Yellow",
       numLegs: 2
     };

     canary instanceof Bird; // => false
  ## Understand Own Properties
     In the following example, the Bird constructor defines two properties: name and numLegs:

     function Bird(name) {
       this.name  = name;
       this.numLegs = 2;
     }

     let duck = new Bird("Donald");
     let canary = new Bird("Tweety");
     name and numLegs are called own properties, because they are defined directly on the instance object. That means that duck and canary each has its own separate copy of these properties. In fact every instance of Bird will have its own copy of these properties. The following code adds all of the own properties of duck to the array ownProps:

     let ownProps = [];

     for (let property in duck) {
       if(duck.hasOwnProperty(property)) {
         ownProps.push(property);
       }
     }

     console.log(ownProps); // prints [ "name", "numLegs" ]
  ## Use Prototype Properties to Reduce Duplicate Code
     Since numLegs will probably have the same value for all instances of Bird, you essentially have a duplicated variable numLegs inside each Bird instance.

     This may not be an issue when there are only two instances, but imagine if there are millions of instances. That would be a lot of duplicated variables.

     A better way is to use Bird’s prototype. Properties in the prototype are shared among ALL instances of Bird. Here's how to add numLegs to the Bird prototype:
   ### Example
     Bird.prototype.numLegs = 2;
     Now all instances of Bird have the numLegs property.

     console.log(duck.numLegs);  // prints 2
     console.log(canary.numLegs);  // prints 2
     Since all instances automatically have the properties on the prototype, think of a prototype as a "recipe" for creating objects. Note that the prototype for duck and canary is part of the Bird constructor as Bird.prototype. Nearly every object in JavaScript has a prototype property which is part of the constructor function that created it.
  ## Iterate Over All Properties
     You have now seen two kinds of properties: own properties and prototype properties. Own properties are defined directly on the object instance itself. And prototype properties are defined on the prototype.
   ### Example1
     function Bird(name) {
       this.name = name;  //own property
     }
     
     Bird.prototype.numLegs = 2; // prototype property

     let duck = new Bird("Donald");

     Here is how you add duck's own properties to the array ownProps and prototype properties to the array prototypeProps:
   ### Example2
     let ownProps = [];
     let prototypeProps = [];
     
     for (let property in duck) {
       if(duck.hasOwnProperty(property)) {
         ownProps.push(property);
       } else {
         prototypeProps.push(property);
       }
     }

     console.log(ownProps); // prints ["name"]
     console.log(prototypeProps); // prints ["numLegs"]
  ## Change the Prototype to a New Object
     Up until now you have been adding properties to the prototype individually:
   ### Example1
     Bird.prototype.numLegs = 2;
     This becomes tedious after more than a few properties.
     
     Bird.prototype.eat = function() {
       console.log("nom nom nom");
     }
     
     Bird.prototype.describe = function() {
       console.log("My name is " + this.name);
     }
   ### Example2
     A more efficient way is to set the prototype to a new object that already contains the properties. This way, the properties are added all at once:

     Bird.prototype = {
       numLegs: 2, 
       eat: function() {
         console.log("nom nom nom");
       },
       describe: function() {
         console.log("My name is " + this.name);
       }
     };
  ## Understand the Prototype Chain
     All objects in JavaScript (with a few exceptions) have a prototype. Also, an object’s prototype itself is an object.

     function Bird(name) {
     this.name = name;
     }

     typeof Bird.prototype; // yields 'object'
     Because a prototype is an object, a prototype can have its own prototype! In this case, the prototype of Bird.prototype is Object.prototype:

     Object.prototype.isPrototypeOf(Bird.prototype); // returns true
     How is this useful? You may recall the hasOwnProperty method from a previous challenge:

     let duck = new Bird("Donald");
     duck.hasOwnProperty("name"); // yields true
     The hasOwnProperty method is defined in Object.prototype, which can be accessed by Bird.prototype, which can then be accessed by duck. This is an example of the prototype chain. In this prototype chain, Bird is the supertype for duck, while duck is the subtype. Object is a supertype for both Bird and duck. Object is a supertype for all objects in JavaScript. Therefore, any object can use the hasOwnProperty method.
  ## Understand the Immediately Invoked Function Expression (IIFE)
     A common pattern in JavaScript is to execute a function as soon as it is declared:

     (function () {
       console.log("Chirp, chirp!");
     })(); // this is an anonymous function expression that executes right away
     // Outputs "Chirp, chirp!" immediately
     Note that the function has no name and is not stored in a variable. The two parentheses () at the end of the function expression cause it to be immediately executed or invoked. This pattern is known as an immediately invoked function expression or IIFE.
