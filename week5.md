# Week 5

# Objectives

1. [Resources](#resources)
- [jQuery vs. Vanilla](#jquery-vs-vanilla)
- [Backbone](#Backbone)
- [Backbone Views](#backbone-views)
- [Backbone Models](#backbone-models)
- [Backbone Routers](#backbone-routers)

---

# Discussion Topics and Homework

### (1.) Monday

**Homework**

1. TBD

### (2.) Tuesday

**Homework**

1. TBD

### (2.) Wednesday

**Homework**

1. TBD

### (2.) Thursday

**Homework**

- TBD

---

# Resources

- http://onepagelove.com/
- http://sass-lang.com/guide
- https://www.npmjs.org/
- https://github.com/jonathanpath/SASS-SMACSS
- http://bonsaiden.github.io/JavaScript-Garden/
- http://devdocs.io/
- Mozilla Developer Network: https://developer.mozilla.org/en-US/
- http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/
- https://developer.mozilla.org/en-US/docs/Web/Reference/API
- https://developer.mozilla.org/en-US/docs/Web/JavaScript
- https://leanpub.com/understandinges6/read/
- http://blog.andyet.com/2014/08/13/opinionated-rundown-of-js-frameworks
- https://github.com/PROSPricing/js-assessment/tree/master/app
- https://github.com/enaqx/awesome-react
- http://backbonejs.org
- https://github.com/instanceofpro/awesome-backbone
- https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/README.md
- http://youmightnotneedjquery.com/
- https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills
- http://caniuse.com/

---

# jQuery vs. Vanilla

---

# Backbone 

**... an MVC (Model-View-Controller) JavaScript Framework**

We've just finished our stint in API's. We've learned that API's have a lot to offer, and that they make a service or ecosystem of apps more scalable and open, where we can integrate almost any services we choose into a nice "mashup".

We've already hit the nail on the head when it comes to MVC structure. The rest of the class is all about MVC frameworks, patterns, and problem-solving with code.

So let's have a gander again at MVC one last time. An MVC application is broken into three parts:

- `M` - Model: the data/JSON stored from an API, and the methods that pull the data from an API
- `V` - View: the templates created, and maybe the methods that draw them to the screen
- `C` - Controller: the mechanisms used to control application flow and state, such as with Path.js to handle hash-routes

Most of the time, the Model is the part of our code that pulls and pushes information back-and-forth to an API.

---

# Backbone Views

Imagine we wanted to write our own class to manage:

- the current container element on the DOM
- clicks, inputs, and other DOM events on this container (or inside it)
- any logic and state-management code for showing or hiding something
- any template loading and HTML generating code

This all falls under the Backbone View.

### A few things about Backbone

Backbone apps have three prerequisite libraries in order for them to work:

- jQuery
- Underscore/lodash
- Backbone.js :-)

Backbone's own constructors are a mashup of good ol' vanilla JS, and mixins for automatically using jQuery and lodash to provide some nicely integrated functionality.

The View, as mentioned before, provides a hidden Constructor enforces particular conventions on how we write code with Backbone. That is how frameworks "do", they simply give you an API, and you use it. Some will be more "opinionated" than others, but Backbone tends to err on the more flexible side of MVC frameworks.

Let's have a look at a Backbone View.

### Creating our first Backbone View

In Backbone-world, we always **create our own constructors by extending the default**.

```js
var myView = Backbone.View.extend({
    // ...
})
var v1 = new myView();
console.log(v1);
```

**Notice that we first just use `Backbone.View.extend()` (no `new`), which creates our myView Constructor.** Then we use `new myView()`.

`console.dir(v1)` to see what it looks like.

Notice a few properties on this 'feller:
- `$el` and `el` - `el` is the html element that is the outermost container of our view. This would be akin to the place your `document.querySelector()` code would find a container element in `index.html` to insert templates. The `$el` property is just the jQuery wrapper around this element, to provide us with some jQuery methods, if we need them.
- `cid` - there is a unique id given to every element created by a Backbone constructor (View, Model, etc). This is that unique id. For now, we'll just accept this exists and move on.

Notice some methods on our `prototype`, provided by Backbone.View, which are accessible on the instance `v1`:
- `v1.$` - jQuery function used to select child elements in our view. For example, we can get all links in our view with `v1.$('a')`.
- `v1.initialize` - whenever the constructor was called (`new myView()`), our `initialize` function is called. This is similar to our `init()` function created on the vanilla JavaScript stuff when we were learning about APIs.
- `v1.remove` - this is a function that will remove this View, container and all, from the DOM.
- `v1.render` - this is a function that we will need to write ourselves (by default, it does nothing). This function should add our container to the DOM (with either a write to an `innerHTML`, `$.append()`, etc)
- `v1.tagName` - by default, when we create a View, Backbone will create a `<div>` element as the container, which will need to be added to the page in `v1.initialize()` or `v1.render()`. We can change this in the options given to `Backbone.View.extend()`.

### Options

There are two cases we supply options to our Backbone instances:
1. as part of the configuration to `Backbone.View.extend({})`
2. as part of the options object to `new AnimalView({})`

**We define the code for our `AnimalView` constructor and prototype in #1, and we define the specific data for our instance in #2.**

### Options for `Backbone.View.extend()`

We can pass options in `Backbone.View.extend({})`. Notice that the argument to `extend` is an object. The `extend` method actually comes from `_` (lodash), and Backbone just has something in its code where `this.extend = _.extend;`. The `extend` function simply works like this:

```js
_.extend(
    { name: 'fred' },
    { employer: 'slate' },
    { years: 15 }
    // ...
);
// → { name: 'fred', employer: 'slate', years: 15 }
```

It takes all properties from the objects, and bundles them up into a single object, giving precedence to right-most arguments.

That last bit can be explained in code:

```js
_.extend(
    { name: 'fred', employer: 'slate' },
    { name: 'matt' }
    // ...
);
// → { name: 'matt', employer: 'slate' }
```

See how `name: 'fred'` was cancelled out in favor of `name: 'matt'`?

Now, let's continue with Backbone.View options:

```js
var AnimalView = Backbone.View.extend({
    tagName: 'li', // defaults to div if not specified
    className: 'animal', // optional, can also set multiple like 'animal dog'
    id: 'dogs', // also optional
    events: {
        'click':         'alertTest',
        'click .edit':   'editAnimal',
        'click .delete': 'deleteAnimal'
    },
    initialize: function(opts) {
        // 1. Sometimes it will be instantiated without options, so to guard against errors:
        var options = _.extend(
            {},
            {
                $container: $('body')
            },
            opts
        );

        // 2. Part of putting a view into its initial state is to put its element
        //    into the DOM. Its container should be configurable using an option
        //    so that a) it can be used anywhere in the app and b) it can be
        //    easily unit tested.
        options.$container.append(this.el);

        // 3. Render the content of the view
        this.render();
    },
    render: function() {
        this.$el.html('test');
    },
    alertTest: function(){
        alert('hi!');
    },
    editAnimal: function(){
        // ...
    },
    deleteAnimal: function(){
        // ...
    }
});
```

The `alertTest`, `editAnimal`, and `deleteAnimal` methods are not provided by Backbone -- these are custom methods added to the prototype chain we created.

**Again**, notice that there are two cases we supply options to our Backbone instances:
1. as part of the configuration to `Backbone.View.extend({})`
2. as part of the options object to `new AnimalView({})`

**We define the code for our `AnimalView` constructor and prototype in #1, and we define the specific data for our instance in #2.**

### Options for `new AnimalView({})`

Backbone will pass the options object from `new AnimalView({})` into `initialize({})`. **If you want to pass options to initialize, you need to write your `initialize()` to handle that. By default `initialize()` does nothing.**

We can pass instance data to the constructor:

```js
var a1 = new AnimalView({
    $container: document.querySelector('.someClass') //--> already on the page
})
```

In the above code, `$container` will overwrite the default `$container` in `initialize()`.

The `Backbone.View#initialize()` method (and all others that we create) have access to `this.$container`. The nice thing about Backbone is that we can add our own properties that should exist on an instance. For instance, we should soon be on `Backbone.Model`, and we can add an instance of a Model to an instance of a View by simply passing it as an option to the constructor:

```js
var Task = Backbone.Model.extend({});
var TaskView = Backbone.View.extend({
    tagName: 'li',
    initialize: function(options){
        options = _.extend({}, {
            $container: $('body')
        }, options);

        this.render();
    },
    render: function(){
        console.log( this.model.get('title') );
    }
})

var t1 = new Task({ title: "Some task name" });
var tv1 = new TaskView({ model: t1 });
```

We can give any Backbone Model or View instance instance properties (like the model) by following the constructor pattern above.

### Serializing a form

If you need to get data out of an HTML `<form>` in you view, you can use `this.$el.serializeArray()` to grab the data out of the form. However, the result will look like:

```js
[
  {
    name: "a",
    value: "1"
  },
  {
    name: "b",
    value: "2"
  },
  {
    name: "c",
    value: "3"
  },
  {
    name: "d",
    value: "4"
  },
  {
    name: "e",
    value: "5"
  }
]
```

To transform the data into a form that you can pass to a model or collection, you can use the following code:

```js
$.fn.serializeObject = function(){
  return this.serializeArray().reduce(function(acum, i){
    acum[i.name] = i.value;
    return acum;
  }, {});
};

$('.my-form').serializeObject();
```

Now the results look like:

```js
{
  a: "1",
  b: "2",
  c: "3",
  d: "4",
  e: "5"
}
```

---

# Backbone Models

Imagine we wanted to write our own class to manage:

- pulling and syncing data from a server
- enforcing defaults on domain data
- providing validation on some JSON data being pulled down from the server
- an event system for updates to domain data

This all falls under the Backbone Model.

> **Note:** our setup-script.sh has been updated to include Backbone!

### Creating our first Backbone Model

In Backbone-world, we always **create our own constructors by extending the default**.

```js
var Task = Backbone.Model.extend({
    // ...
})
var t1 = new Task({});
console.log(t1);
```

Notice a few properties on this 'feller:
- `attributes` - a collection of key/value pairs that are passed to the instance constructor (`new Task({ ... })`) or provided in the `defaults`.
- `changed` - an object that will hold any key/value pairs of the Model instance that were just changed
- `cid` - there is a unique id given to every element created by a Backbone constructor (View, Model, etc). This is that unique id for this Model instance. For now, we'll just accept this exists and move on.

Notice some methods on our `prototype`, provided by Backbone.Model, which are accessible on the instance `t1`:
- `changedAttributes()` - returns a copy of the `changed` object, used to get the diff between the current and previous state for the instance
- `clear()` - empties out a Model's data
- `clone()` - creates an exact replica in memory of the Model instance and its data
- `fetch()` - pulls JSON information from a defined URL
- `initialize()` - whenever the constructor was called (`new Task()`), our `initialize` function is called. This is similar to our `init()` function created on the vanilla JavaScript stuff when we were learning about APIs.
- `off()/on()` - used to handle add/update/delete events, and so forth
- `sync()` - attempts to push this instance data up to a server
- `toJSON()` - creates a POJO (plain old JS object literal) with all the data from the instance.
- `set()/get()` - these are the getter and setter methods to use when retrieving or updating an instance's data; **you must use these when manipulating or accessing (ahem: Shawn) the model's data.**

### Defining Model (constructor) properties

There are two cases we supply options to our Backbone instances:

1. as part of the configuration to `Backbone.Model.extend({})`
2. as part of the options object to `new Task({})`

**We define the code for our `Task()` constructor and prototype in #1, and we define the specific data for our instance in #2.**

### Options for `Backbone.Model.extend()`

We can pass options in `Backbone.Model.extend({})`:

```js
var Task = Backbone.Model.extend({
    defaults: {
        title: 'Task title',
        isDone: false,
        inProgress: true
    }
});

var t1 = new Task({});
t1; // { title: 'Task title', isDone: false, inProgress: true }
```

### Create Models with attributes

We can pass instance data to the constructor:

```js
var t1 = new Task({
    title: 'Some task name',
    dueDate: new Date(),
    isDone: false,
    inProgress: true
});

t1; // { title: 'Some task name', dueDate: DateObject, isDone: false, inProgress: true }
```

### Validating properties on Model instances

Whenever we set properties on a Model instance, the `Model#validate()` method will be automatically called. If `validate()` returns a string, an `Error()` is thrown with that message.

```js
var Task = Backbone.Model.extend({
    defaults: {
        title: 'Task title',
        isDone: false,
        inProgress: true
    },
    validate: function(attrs, options){
        if (!attrs.title){
            return 'Your task must have a name!';
        }
    }
});

var t1 = new Task({ title: false }); // will throw an error "Your task must have a name!"
```

### Getting attributes

Using the `Model#get()` method we can access Model properties at anytime.

```js
var t1 = new Task({});

t1.get('title'); // 'Task title'
t1.get('isDone'); // false
```

### Setting Model attributes

We can manipulate Model instances with `Model#set()` or with our own methods defined in the Constructor:

```js
var Task = Backbone.Model.extend({
    defaults: {
        title: 'Task title',
        isDone: false,
        inProgress: true
    },
    validate: function(attrs, options){
        if (!attrs.title){
            return 'Your task must have a name!';
        }
    },
    setDone: function(){
        this.set('isDone', true);
    }
});

var t1 = new Task({});
t1.setDone();
t1.set({ inProgress: false, title: '(done)' }); // we can also set multiple items in one call
```

### Listening for change events on the Model

Models by default trigger 'change' events whenever data is updated.

```js
var Task = Backbone.Model.extend({
    initialize: function(){
        this.on('change', function(model){
            alert('something was changed');
        });
        this.on('change:title', function(model){
            alert('title was changed');
        });
    },
    defaults: {
        title: 'Task title',
        isDone: false,
        inProgress: true
    },
    validate: function(attrs, options){
        if (!attrs.title){
            return 'Your task must have a name!';
        }
    },
    setDone: function(){
        this.set('isDone', true);
    }
});

var t1 = new Task({});

t1.setDone();
// ALERTED: "something was changed"

t1.set({ title: "(done)" });
// ALERTED: "something was changed"
// ALERTED: "title was changed"
```

### Models can validate, have defaults, and trigger events when something changes... they can also do your AJAX requests for you.

Models are used to represent data from your server and actions you perform on them will be translated to RESTful operations:
- GET translates to a `$.get` (`type: "GET"`)
- CREATE translates to a `$.post` (`type: "POST"`)
- UPDATE translates to a `$.ajax` with `type: "PUT"`
- DEL translates to a `$.ajax` with `type: "DEL"`

### Think back to our Etsy API requests.

- When we pulled all active listings, we got an array of data.
- When we setup routing with the Etsy project, we queried the Etsy API with a different URL that targeted a **specific id**.

**When we create Models and have them pull data from the server, we will give them an `id`, then tell them to `fetch()`.**

### Getting Model data from a server with `Model#fetch()`

We've mentioned something before:

> Nearly every record you pull from ANY API has a unique identifier -- an `id`. This is what we use to too a Model to pull that specific piece of information form the server.

Well, with Backbone Models, we need just two things:
- The `id` of an individual Etsy record
- The URL to put that `id` into when making the request for that individual Etsy item

Let's take a look at some example code for a fake API on the localhost. **Notice the use of Promises!!**

```js
var UserModel = Backbone.Model.extend({
    urlRoot: '/user',
    defaults: {
        name: '',
        email: ''
    }
});

var matt = new UserModel({ id: 1 });
matt.fetch().then(function(){
    // AJAX request finished!
})
```

### Saving Models to a server with `Model#save()`

```js
var UserModel = Backbone.Model.extend({
    urlRoot: '/user',
    defaults: {
        name: '',
        email: ''
    }
});

var matt = new UserModel({ name: "Matt", email: "matt@theironyard.com" });
matt.save().then(function(model){
    // AJAX request finished!
    alert(model.toJSON());
})
```

### Deleting Models from a server with `Model#destroy()`

```js
var UserModel = Backbone.Model.extend({
    urlRoot: '/user',
    defaults: {
        name: '',
        email: ''
    }
});

var matt = new UserModel({ id: 1 });
matt.destroy().then(function(model){
    // AJAX request finished!
})
```

### Backbone Collections

Collections are just lists of a particular Model constructor with events that fire when something is added, changed, removed, etc. A Collection constructor is created (surprise!) yet again with `Backbone.Collection.extend({})`:

```js
var TaskList = Backbone.Collection.extend({
    model: Task
});
```

Notice that we only add one property to the options, a Model constructor, **NOT a Model instance**. There is an **optional** `url` argument that Backbone will use (when `fetch()` is called) to fill a Collection with Models populated with data from that `url`.

Let's create a Collection instance, and pass it an array of JSON data:

```js
var tasks = new TaskList([
    {title: "some task", isDone: true},
    {title: "some task 2", inProgress: false},
    {title: "some task 3"}
]);
```

Observe the `tasks` Collection instance in the Chrome Dev Console.

Notice a few properties on this 'feller:
- `length` - the number of Models in this Collection.
- `models` - an array of Models created/added by the constructor or through `Collection#add()`

Notice some methods on our `prototype`, provided by Backbone.Collection, which are accessible on the instance `tasks`. There are a **lot**, because Collections deal with arrays of Models, and Backbone uses lodash behind the scenes. Thus, it makes sense why we would see a lot of methods here:
- `add()` - adds a Model to the Collection and triggers an event
- `fetch()` - pulls down an updated Collection from a specified URL, returns a Promise object
- `get()/set()`
- `forEach()`
- `on()/off()`
- and of course all the lodash methods

### Creating a list of Models automatically with `Collection#fetch()`

When querying the Etsy API, we had two things going on:

1. Request all active listings, which returned some object with an array of listings
2. We then would make a request to a separate URL with an `id` in it to make requests for an individual listing

** Number 1 above would be handled by a Collection, while Number 2 would be handled by a Model. **

```js
var EtsyListing = Backbone.Model.extend({});

var EtsyItems = Backbone.Collection.extend({
    model: EtsyListing,
    api_key: "aavnvygu0h5r52qes74x9zvo",
    url: function(){
        return [
            'https://openapi.etsy.com/v2/listings/active.js?',
            "api_key=",
            this.api_key,
            "&callback=?"
        ].join('')
    }
});

var items = new EtsyItems();
items.fetch().then(function(collection){
    // AJAX function has finished!
})
```

When we call `items.fetch()`, Backbone's Collection code will fire of a request and try to automatically create a bunch of `EtsyListing`'s.

There's one last thing we need to do. Backbone Collections assume that the data they are pulling down with `fetch()` is **just an array**. Instead, Etsy gives us an array nested inside an object:

```js
{
    count: 50100,
    results: {[ ... ]}
}
```

What we need to do is tell the `EtsyItems` Collection to look for the `results` property and start creating `EtsyListing` items from there:

```js
var EtsyListing = Backbone.Model.extend({});

var EtsyItems = Backbone.Collection.extend({
    model: EtsyListing,
    api_key: "aavnvygu0h5r52qes74x9zvo",
    url: function(){
        return [
            'https://openapi.etsy.com/v2/listings/active.js?',
            "api_key=",
            this.api_key,
            "&callback=?"
        ].join('')
    },
    parse: function(data){
        return data.results;
    }
});

var items = new EtsyItems();
items.fetch().then(function(collection){
    // AJAX function has finished!
})
```

The `parse` property given to `EtsyItems` tells Backbone Collection to look into the data returned and use the `results` array to create the Models.

---

# Backbone Routers

Backbone Routers work virtually the same as Path.js.

```js
var App = Backbone.Router.extend({
    initialize: function(){
        this.appView = new appView();
        this.collection = new EtsyCollection();

        Backbone.history.start();
    },
    routes: {
        "etsy": "showEtsyItems", // matches http://example.com/#etsy
        "etsy/:id": "showEtsyItem", // matches http://example.com/#etsy/1234
        "download/*path": "downloadFile", // matches http://example.com/#/download/user/images/hey.gif
        "*actions": "defaultRoute" // matches anything not matched before this, i.e. http://example.com/#anything-here
    },
    defaultRoute: function(){
        //...
    },
    downloadFile: function(path){
        //...
    },
    showEtsyItems: function(){
        //...
    },
    showEtsyItem: function(id){
        //...
    }
})

window.onload = app;

function app(){
    var app = new App();
}
```

---
