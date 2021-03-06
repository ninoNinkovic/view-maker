# ViewMaker For Laravel 5.2

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Total Downloads][ico-downloads]][link-downloads]

**ViewMaker** is for use with the Laravel PHP framework (5.2 and up) Artisan command line tool.

ViewMaker adds 12 new artisan commands, providing ready-made templates for CRUD generation, Views and Datagrids, with ajax-powered search, column sorts and pagination, and chart.js charts.   You can create, migrate and test a foundation of code with crud and views in under a minute.

Help **[Support ViewMaker](#support-viewmaker)**.  

## Install ##

Via Composer

```
composer require evercode1/view-maker
```

In your config/app.php file, add the following to the providers array:

```
Evercode1\ViewMaker\ViewMakerServiceProvider::class,
```

## Usage

## Summary

With the **[make:master](#makemaster)** command, instantly create a layouts folder and master page, with all Bootstrap and jquery dependencies abstracted to partials for easy customization and extensibility.

![](layouts-folder.png)

With the **[make:foundation](#makefoundation)** command, create a code foundation instantly and get a working datagrid, searchable, sortable, and paginated, written with vue.js:

![](vue-index.png)

You also get the create, edit, and show views, instantly.  Here is what the create view looks like:

![](basic-create.png)

Minimal bootstrap is used, so you can easily modify and extend as you wish.

With the **[make:chart](#makechart)** command, you can easily add a chart.js chart, written with vue.js,  to the index page:

![](chart-line.png)

## ViewMaker Commands

ViewMaker will install 12 artisan commands.

7 make commands:

* **[make:master](#makemaster)**
* **[make:foundation](#makefoundation)**
* **[make:crud](#makecrud)**
* **[make:views](#makeviews)**
* **[make:parent-child](#makeparent-child)**
* **[make:child-of](#makechild-of)**
* **[make:chart](#makechart)**


5 remove commands:

* **[remove:foundation](#removefoundation)**
* **[remove:crud](#removecrud)**
* **[remove:views](#removeviews)**
* **[remove:child-of](#removechild-of)**
* **[remove:chart](#removechart)**

Use **[make:master](#makemaster)** to create a master page, providing dependencies, which includes:

* layouts folder
* master (you give it your name)
* meta partial
* css partial
* scripts partial
* bottom partial
* nav partial
* shim partial
* jquery
* bootstrap
* font-awesome

Use **[make:foundation](#makefoundation)** to create all files for crud and views, including:

* model
* controller
* api controller (if it does not yet exist)
* migration
* test
* appropriately-named view folder
* index view
* create view
* edit view
* show view

The **[make:foundation](#makefoundation)** command also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists)

Use **[make:crud](#makecrud)** to create the files necessary to display a view:

* model
* controller
* api controller (if it does not yet exist)
* migration
* test

The **[make:crud](#makecrud)** command also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists)

Use **[make:views](#makeviews)** to create views, including:

* appropriately-named view folder
* index
* create
* edit
* show

The **[make:parent-child](#makeparent-child)** will create all crud and view files for both a parent and a child, including. 

* model
* controller
* api controller (if it does not yet exist)
* migration
* test
* appropriately-named view folder
* index view
* create view
* edit view
* show view

The  **[make:parent-child](#makeparent-child)** command also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists) 

This command operates the same way as the **[make:foundation](#makefoundation)** command, but it builds a foundation for both the parent and child.  

In the views, it will display the relationship and in the create and edit views of the child, you will get the related parent, so when you create a child record, you can associate it to a parent record.  Use the optional slug parameter if you want to have slugs on the show pages.

The **[make:child-of](#makechild-of)** command is similar to the **[make:parent-child](#makeparent-child)** command, but only creates the child.

The **[make:child-of](#makechild-of)** will create all crud and view files for both a parent and a child, including. 

* model
* controller
* api controller (if it does not yet exist)
* migration
* test
* appropriately-named view folder
* index view
* create view
* edit view
* show view

The  **[make:child-of](#makechild-of)** command also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists) 

Instead it modifies the parent model to include the relationship.  The slug option is available for this command as well.

Use **[make:chart](#makechart)** to create a chart of your data, including:

* chart.js chart on index
* chart api route
* chart api method
* toggles line or bar type graph
* Select period of time for graph display
* vue.js implementation of chart.js

## Master Page Required For All Views

Please note:

ViewMaker templates assume you use and have a master page. If you don't already have a
master page, we recommend using our **[make:master](#makemaster)** command, it will include the things you need for working ajax calls.

If you don't use our **[make:master](#makemaster)** to create your master page, then you need to make sure you have the following:

* a master page in a folder named layouts in your views folder
* To use the DataTables template, jquery is a dependency
* jquery must be called first by you (most likely in your masterpage)
* To use the DataTables, you need an @yield('css') tag on your master page
* To use ajax for grids, you need a meta tag for the csrf token in your master page

example for csrf token:

```
<meta name="csrf-token" content="{!! csrf_token() !!}">
```

Our **[make:foundation](#makefoundation)** and **[make:crud](#makecrud)** creates everything you need to display views, but if you just
want to use ViewMaker to create views, you will need to write your model, route, migration, and
controllers in order to be able to see the views created by ViewMaker in your application.

All of these [requirements](#requirements-for-views) are listed in detail below, but since they are common sources of bugs, I have listed them up here.  You can use it as a checklist to make sure you have what you need to use ViewMaker successfully.

## How to Learn ViewMaker Commands

To play around with ViewMaker, and to learn quickly, we recommend installing a fresh build of Laravel with a working database connection.  Then run the **[make:master](#makemaster)**, which will provide your layouts folder, master page and asset dependencies.

After you have your master page and dependencies, follow the **[make:foundation Workflow Example](#makefoundation-workflow-example)** in the next section.

## make:foundation Workflow Example

To fully understand the power of the **[make:foundation](#makefoundation)** command, let's walk through a typical use case.  For this, we will assume that you have a master page named master.blade.php in your layouts folder, which is in your views folder.

To create that we recommend using our **[make:master](#makemaster)** command, it will supply you with everything you need to create a foundation.  Just give your master page a name and supply an optional name for your app, like so:

```
php artisan make:master master Demo
```

That would create a layouts folder in your views directory and create a master page named master.blade.php.  This would include the dependencies you need for ajax calls and a working data grid.

You can use your own master page, but if you do that, let's double check to make sure we have what we need:

In your masterpage or related meta partial, you should have your csfr token:

```
<meta name="csrf-token" content="{!! csrf_token() !!}">
```

Your masterpage or related css partial should also have your css tag;

```
@yield('css')
```

and in the scripts section of your masterpage or related scripts partial, you should have your call to jquery, for example:

```
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
```

You need to make sure that is all there before running the command, otherwise
the views containing ajax and grids will not function properly.

Now we're ready to try the **[make:foundation](#makefoundation)** command.  We’ll use an example model named Widget.  So let's create a Widget foundation for our Widget model with the following command:

```
php artisan make:foundation Widget master vue
```

Obviously, Widget is the name of the model we want to create. This is followed by master, which is the name of our master page.  And that is followed by vue, which is the type of template that we want, which will get us a fully paginated, searchable, sortable datagrid, written in vue.js.

After that runs, we're ready to migrate up to our db.  Before you migrate, this is where you can add additional columns for the DB, if you like.  To keep it simple, let's just migrate what we already have:

```
php artisan migrate
```

Everything should work at this point if you test the /widget route, but there is no data.  So let's run a unit test to add a single record by running from the command line:

```
vendor/bin/phpunit
```

You should get green and a record in the db. It's a very basic test and it should pass.

If your test fails:

Some versions of Laravel 5.2 require your routes to be in a web group.  Since
the new routes we made are just appended to the end of the file, you may need
to move them inside a route group, depending on your version of Laravel.

The test can also fail if another test, like ExampleTest, is having a problem and crashes
the program.  In that case fix or remove the broken tests, and make sure your route group is correct, in some versions you need one, in some versions you don't, and that should solve the problem.

Also note, if you are using the **[make:foundation](#makefoundation)** command and input 'plain' or 'basic' for template type, the test will fail because it is expecting to see the name of the record on the index page, and those templates intentionally do not include a way to do that, since the purpose is keep the code minimal.

Similarly, if you choose an index only option, there will be no create view and the test will fail. In those cases modify the test as you see fit.

Next you can use the factory to seed the db.  We start by calling tinker:

```
php artisan tinker
```

Then the following command:

```
factory('App\Widget', 30)->create();
```

Then control D from the command line to quit tinker.

If you don't want to use tinker, manually add some records via the create form.

With that you should be able to go to your /widget route and see the following:

![](vue-index.png)

Please note that the header and footer pictured above are called in by the master page, so
if you did not use our **[make:master](#makemaster)** command, you will see the output of your masterpage instead or an error if you have no master page.

As you can see the workflow with the **[make:foundation](#makefoundation)** command is optimal, in under a minute you are able to stand up a working crud application.  You can then easily modify it to add the fields you want, and you have everything in place to support what you need, including all the basics like the model, migration, route and controller, as well as a unit test, api controller, and factory method for seeding.  The **[make:foundation](#makefoundation)** command provides you with a complete foundation to start from.

Also see the [tip for use with make:auth](#tip-for-use-with-makeauth) to see how you can use artisan's native make:auth command to set up all your auth views to extend the master page you have created with [make:master](#makemaster).

Once you have made a foundation, you can also easily add a chart.js chart to your index view by using our **[make:chart](#makechart)** command.  The result will add a chart that looks like:

![](chart-line.png)

## make:master

ViewMaker's make:master command creates a layouts folder and places a master page and related files in it.

```
php artisan make:master {MasterName} {AppName=Demo}
```

The second parameter is optional and will default to Demo.

You supply the command with two arguments, the name you want for your master page and the name of your application.  For example, if we wanted our master page to be called master and our app name was MyProject:

```
php artisan make:master master MyProject
```

This will create the following:

* layouts folder within the views folder
* master page named by whatever you inputted
* bottom partial
* nav partial
* css partial
* meta partial
* scripts partial
* shim partial

The master page includes the partials and this makes the code very easy to work with.

ViewMaker includes a minimal bootstrap implementation, which you can easily change to suit
your tastes.  Note that inside the css partial, you get the following:

~~~~

<!-- Move style to a permanent home in your main .css file -->
<style>

   body {
       padding-top: 65px; }

</style>

~~~~

This is so the breadcrumbs will show below the nav, which is a bootstrap top pin.  You should move that to a permanent home as the comment suggests.  If your main .css already accounts for this padding, you can simply remove this style from your css.blade.php file.

If you have a .css file that your application requires, you need to call it from your css.blade.php file.  Obviously, you can and should modify these files in any way that suits your application's needs.

Using ViewMaker's make:master also makes it easier to work with the other commands, such as **[make:foundation](#makefoundation)**, since it is setup for the dependencies that you need.

Note that the second argument in the make:master command is optional.  If you leave it off, for example:

```
php artisan make:master master
```

It will default to naming your app "Demo" in the bootstrap navbar-brand class, which will appear on your top nav.

## Tip for use with make:auth

Here's a tip for using make:master with artisan's native make:auth command.  As you probably already know, the make:auth command will create all your auth views, extending a master page named app.blade.php.  You can easily use both commands.  

Run [make:master](#makemaster) first, but make sure you do not name your master page 'app,' so there is no conflict with the page that the make:auth command will make.  After running [make:master](#makemaster), run the make:auth command.  Then all you have to do is go to the views/auth folder and change the @extends('layouts.app') directive in those view files to @extends('layouts.whatever-your-master-page-is-named').

Please note that the make:auth command also creates a controller that returns the user to a specific page for logging in and registering, so you will have to modify that view as well if you want your generated master page extended there as well.

## make:crud

```
php artisan make:crud Widget slug
```

The make:crud command takes two arguments, the name of the model you wish to build your crud on and the optional slug parameter.  As typed above, the command would create the following file types:

* model
* controller
* api controller (if it does not yet exist)
* migration
* test

It also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists)

Since we specified ‘slug’, it will include the code necessary to have slugs on the show view.

You could then run the **[make:views](#makeviews)** command and have it functional, once you've migrated and seeded data or created a few records.  Note that the unit test included with make:crud will fail if you select 'plain' or 'basic' as your template type because those templates don't output the record name to the index page.  In that case modify the test as you see fit.

## make:views

The make views lets you quickly scaffold views for create, show, edit, and index, based on your input.

The make:views command has  the following arguments:

```
php artisan make:views {ModelName} {MasterPageName} {TemplateType} {Slug=false} {IndexOnly=false}
```
The last argument is optional and indicates that you only want the index view in the view folder. 
By default it is false, which means it's an optional argument, so if you leave it off
entirely, you get all the views.  If you do wish to use that option, you must enter
the word 'index' as your last argument, no quotes.

Before running make:views, at a minimum, you should already have your model, route and controller created.

As an alternative to doing that manually, you can use **ViewMaker's** **[make:crud](#makecrud)** to do it for you.  Or you could use **[make:foundation](#makefoundation)** to create everything all at once.  If you use **[make:foundation](#makefoundation)**, you do not need to run make:views, since the views will be included in the foundation.

We recommend using our **[make:master](#makemaster)** command to make your master page.  In any event before you run make:views, you need to have your master page ready. Also see [requirements for views](#requirements-for-views).

So for example, if you had a model named Widget, and you  had a master page
named master.blade.php, you may do one of the following:

```
php artisan make:views Widget master plain
```

```
php artisan make:views Widget master basic
```

```
php artisan make:views Widget master dt
```

```
php artisan make:views Widget master vue
```

In the examples above, we tell it the model name, 'Widget', the master page name 'master', and the template type.  Since we didn’t want the slug or index only options, we can simply leave those off the command, they will default to false.

The plain template creates simple stubs, the basic template gives you a
couple of working forms and the dt and vue templates give you a working data
grid implementation with search and column sorts. 

The templates are described in detail in **[Template Types](#template-types)** section.  Also see the **[Requirements For Views](#requirements-for-views)** section to make sure you have what you need before running this.  And finally, check out the conventions section for naming tips on models and instance variables, so you know what to expect there.

## make:foundation

The make:foundation command has the following arguments:

```
php artisan make:foundation {ModelName} {MasterPageName} {TemplateType} {Slug=false} {IndexOnly=false}
```

The last argument is optional and indicates that you only want the index view in the view folder. 
By default it is false, so unless you indicate otherwise, you will get all the views.  If you do wish
to use that option, you must enter the word 'index' as your last argument, no quotes.

The make foundation command also supports the slug option to show slugs on the show view.  If you want to use that, include the string ‘slug’ as the 4th argument.

Let's look at some typical examples.  If you wanted to create a model named Widget without slugs, and you had a master page named master.blade.php, you may do one of the following:

```
php artisan make:foundation Widget master plain
```

```
php artisan make:foundation Widget master basic
```

```
php artisan make:foundation Widget master dt
```

```
php artisan make:foundation Widget master vue
```

You can also use the optional arguments of ‘slug’ and ‘index’.  Adding slug will create slugs for your show view.  Indicating index will tell ViewMaker that you only want to create the index view.

make:foundation will create the following:

* model
* controller
* api controller (if it does not yet exist)
* migration
* test
* appropriately-named view folder
* index view
* create view
* edit view
* show view

make:foundation also appends to the following files:

* routes.php
* ModelFactory.php
* ApiController (if it already exists)

Note that the test included with make:foundation will fail if you select 'plain' or 'basic' as
your template type or if you have because those templates don't output the record name to the index page. 

Similarly, if you choose an index only option, there will be no create view and the test will fail. In those cases modify the test as you see fit.

## make:chart

The make:chart command is dependent on an index view that was build with the make:views or make: foundation command, so don’t run it if you are not using the other command to build your view.

When you run the make:chart command, you get the following:

* chart.js chart on index
* chart api route
* chart api method
* toggles line or bar type graph
* Select period of time for graph display
* vue.js implementation of chart.js

The signature of the command is as follows:

```
php artisan make:chart {ModelName}
```

Assuming you had a foundation built for a Widget model and you ran the following:

```
php artisan make:chart Widget
```

You would get the following on your index view:

![](chart-line.png)

You can use the select for type to switch to a bar graph:

![](chart-bar.png)

If you have run the factory method to seed data, you should change some of the create dates on the records if you want to try the different periods that the chart comes with.




## make:parent-child

Sometimes we need to associate two models, the most common example being Category and
Subcategory.  ViewMaker's make:parent-child let's you create this relationship with a single
command.

The command has the following arugments:

```
php artisan make:parent-child {ParentName} {ChildName} {MasterPageName} {TemplateType} {Slug=false} {IndexOnly=false}
```

So for example, you might want to run it as follows:

```
php artisan make:parent-child Category Subcategory master vue
```

That would be the equivalent to running the **[make:foundation](#makefoundation)** command on both Category and Subcategory, with a few important differences.

The datagrid it will build for Subcategory for example, will show the category that the subcategory belongs to.

Both the parent model and the child model will be built with the relationship made for you.  The parent will have a has many relationship and the child will have a belongsTo relationship in the model.

You also get a dropdown list of parent models, for example, categories, on the child create form, or in this case the subcategory create form, which allows you to associate the child to the parent.

This command, like the **[make:foundation](#makefoundation)** command, will create all the crud and views for you, you only need to run your migration, unit tests, and factory methods to populate them.  You can do this in under a minute.

## make:child-of

In cases where you have an existing model, and you want to create a foundation for a child model, you should use the make:child-of command.  The signature of the command is as follows:

```
php artisan make:child-of {ParentName} {ChildName} {MasterPageName} {TemplateType} {Slug=false} {IndexOnly=false}
```

So for example, if you had created a foundation for AutoMaker and you wanted a child model named AutoPart, you could run the following command:

```
php artisan make:child-of AutoMaker AutoPart master vue
```

This would update the parent model, in this case AutoMaker, with the has many relationship and also create a foundation for AutoPart, which will include the belongs to relationship to AutoMaker.

## Remove Commands

## remove:foundation

```
php artisan remove:foundation {ModelName}
```

This command will remove all of the foundation files for the given model:

remove:foundation will remove the following:

* model
* controller
* api controller methods
* migration
* test
* appropriately-named view folder
* index view
* create view
* edit view
* show view
* factory method
* routes


## remove:crud

This command will remove all of the crud files for the given model:

```
php artisan remove:crud {ModelName}
```

remove:crud will remove the following:

* model
* controller
* api controller methods
* migration
* test
* factory method
* routes

## remove:views

This command will remove all of the view files for the given model:

```
php artisan remove:views {ModelName}
```

Remove:views  will remove the following:

* appropriately-named view folder
* index view
* create view
* edit view
* show view

## remove:child-of

This command will remove all of the foundation files for the given parent and child:

```
php artisan remove:child-of {ParentName} {ChildName}
```

remove:child-of will remove the following:

* child model
* child controller
* child api controller methods
* child migration
* child test
* child view folder
* child index view
* child create view
* child edit view
* child show view
* child factory method
* child routes
* parent model relationship




## Requirements For Views

To use the **[make:views](#makeviews)** or **[make:foundation](#makefoundation)** command successfully, you need to have a master page. We recommend using our **[make:master](#makemaster)** command, it will give you a nice starting point for your project.

If you decide to make the master page yourself, you will need the following:

* a master page in a folder named layouts in your views folder
* To use the DataTables template, jquery is a dependency
* jquery must be called first by you (most likely in your masterpage)
* To use the DataTables, you need an @yield('css') tag on your master page
* To use ajax for grids, you need a meta tag for the csrf token in your master page

example for csrf token:

```
<meta name="csrf-token" content="{!! csrf_token() !!}">
```
If you need to reference what a master page is, here is the info:

[Master Page Docs in Laravel](https://laravel.com/docs/5.2/blade#template-inheritance)

To call in jquery, you may do so by CDN in your scripts section of your master page:

```
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
```

Please refer to the actual CDN for the newest link.

In the css section of your master page or related partial, make sure you have the following:

```
@yield('css')
```

That should come after bootstrap or whatever your main css is.

## Template Types

In the following sections, we cover the template types that are available in the **[make:views](#makeviews)** and **[make:foundation](#makefoundation)** commands.  The options are as follows:

* plain
* basic
* dt
* vue


## Plain Templates

Using plain for template type will create a view folder, named according to your input, in your views directory.  For example, if you input Widget as the model, you would get a widget view folder. Then within that folder, the following files:

* create
* show
* edit
* index

All blade files will extend whatever value for master page that you inputted
on the command.  In the example above, the master page is named master.blade.php,
so we used 'master'. 

If for whatever reason you are not using a master page for your application,
then you will have to go to each file and remove the @extends directive.

Since most applications will use a masterpage for common html markup, extending
the master page is included in the plain templates.

When using plain as the template type, you get the view folder and and the 4 view
files.  In each file, you get the extends directive and a single \<h1> tag, 
for example in create.blade.php, if you input Widget as your model, you would see:

"This is your Widget Create page"

And that’s it.  So use plain if you just want to stub out the folder and the files,
this can still be a time-saver.

## Basic Templates

Many times, we need to do rapid prototyping, so we have a very simple and minimal
bootstrap implementation named ‘basic’.   The artisan command is for that is as
example:

```
php artisan make:views widget master basic
```

Using ‘basic’ for template type will create a widget folder in your views directory,
and then within that folder, the following files:

* create
* show
* edit
* index

This will give you an input forms for create and edit and a show page and a simple
index page.

![basic create view](basic-create.png)

Above you can see that you are provided with a single input for widget name, which
in the form will be labelled widget_name.  The convention for fieldname is
$modelName . ‘_name’.  If you follow a different convention, then you will have to
change the value of the field name in all the appropriate spots.  Please see the
[conventions](#conventions) section for tips on how the templates are formatted.

Note that that the header and footer in the sample above are brought in by extending
the master page, they will not be generated by the **ViewMaker**. 

The action on the above form is set to POST  to /widget, with the appropriate token set
to avoid csrf.  That actual posting of the form depends on you having a model, route,
controller and db setup.  None of that is created for you by **ViewMaker**.  That said,
these templates are designed to work with a route resource, so if you had the following
in your routes file:

```
Route::resource('widget', 'WidgetController'):
```

That would obviously point to a WidgetController, which you have to make on your own. 
If you want the form to actually function, you need to write your update method on your
controller. 

You can see a basic implementation of the model, migration, route, and controller that
support the basic views at:

[demo](https://github.com/evercode1/package-for-views)

Please note that is the code on github, not a live demo.

Since the templates only provide for a single field, it is easy to add fields, modify
the html markup and css to suit your own tastes and needs.  Using the **[make:views](#makeviews)** command
is a starting point that will get you up and running quickly.

## Datatables Templates

Sometimes we need to do quick prototyping of data grids.  With our datatables implementation,
you get all of the views that come with basic, but you get also get a working data grid
on the index page.

Our dt template is a simple implementation of jquery datatables, which can get you up
and running quickly with a sortable, searchable grid.  The command for that looks like
this:

```
php artisan make:views widget master dt
```

You can use our **[make:crud](#makecrud)** or **[make:foundation](#makefoundation)** command to create the route, model, migration, api route, controller, api controller, factory method and test to set up the data you want.

In the event you don't want to use our commands, then you have to do it on your own.  If you do that, make sure to follow our conventions.  See the conventions section for more details.

Assuming you have set up your route, model, migration, api route, and controller, and have some records, the dt template will get you the following:

![](dt-index.png)

Again note the header and footer are brought in by master page, which you create
separately.  We recommend our make:master command for creating master page because it will
set the dependencies for the views.

When you run the **[make:views](#makeviews)** command with ‘dt’, you get two additional view pages.   One is datatable.blade.php, which holds the table partial.  The other is datatable-script.blade.php, which holds the datatable script.

The datatable-script.blade.php is a temporary home for your script.  You should move  your
datatable-script.blade js code to a permanent home, such as in public/js folder or assets/js or some other location for your js assets.  It’s up to you how you want to organize that.

As mentioned numerous times throughout the documentation, that you need the following meta tag:

```
<meta name="csrf-token" content="{!! csrf_token() !!}">
```

The easiest way to make sure you have all the configuration correct is to use our **[make:master](#makemaster)** command for your master page, it will include the above meta tag.

You can see how I did all this in the demo app:

[demo](https://github.com/evercode1/package-for-views)

Please note that is the code, not a live demo.  But you can see how I structured the
master page and the cdn calls.

In addition to having the route resource and matching controller, you also need a route
for your api call, which again, using widget as an example, would be:

```
Route::any('api/widget', 'ApiController@widgetData');
```

Please note that this is provided with our **[make:crud](#makecrud)** and **[make:foundation](#makefoundation)** commands.

This route assumes you have a controller named ApiController, which are also provided by
**[make:crud](#makecrud)** and **[make:foundation](#makefoundation)**. 

I’m using any as the verb here so I can do a get request to debug.  You also need to format
the json response a specific way,  so for example, you api controller could look like this:

~~~~

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use DB;

class ApiController extends Controller
{
  public function widgetData(){

      $result['data'] = DB::table('widgets')
                      ->select('id as Id',
                               'Widget_name as Name',
                               'Created_at as Created')
                      ->get();

      return json_encode($result);

  }
}

~~~~

Obviously, you can add new columns to the select statement as you require them, as long
as you have added them in your db.  You would also have to have a corresponding table
row on your datatable.blade.php partial.  And of course you would have to modify your
script to account for additional columns.

Datatables is a popular jquery plugin, the docs are here:

[Datatables](https://datatables.net/)

## Vue.js Templates

Sometimes we need to do quick prototyping of data grids and we want to use vue.js.
With our vue.js implementation, you get all of the views that come with basic, but
you get also get a working data grid on the index page.

Our vue template is a simple implementation of a vue.js grid component, which can get you up
and running quickly with a sortable, searchable grid.  The command for that can look like
this:

```
php artisan make:views widget master vue
```

Assuming you have some records, and have set up your route, model, migration, api route,
and controller, that will get you the following on your index page:

![](vue-index.png)

Again note the header and footer are brought in by master page, which you create
separately on your own. 

You also need a meta tag, which will create the tokens for your ajax calls,
so put it in the appropriate place in your head section:

```
<meta name="csrf-token" content="{!! csrf_token() !!}">
```
You can see how I did all this in the demo app:

[demo](https://github.com/evercode1/package-for-views)

I break out my meta section as a view partial, which gets called into master.blade.php,
but you can do it any way you want as long as you have it in there correctly.

Please note that is the code, not a live demo.  But you can see how I structured the
master page and the cdn calls.  This is exactly what our **[make:master](#makemaster)** command creates.

When you run the **[make:views](#makeviews)** command with ‘vue’, you get your script, template, and css all included on the same index page.  You also get create.blade.php, edit.blade.php, and show.blade.php, but those are the same as the basic template, so refer to that for what those will look like.

The ViewMaker will get you up and running quickly, but you should move your
vue js code to a permanent home, such as in public/js folder or assets/js or some other location for your js assets.  It’s up to you how you want to organize that.  The same is true for the css that I provided for it.

In addition to having the route resource and matching controller, you also need a route
for your api call, which again, using widget as an example, would be:

```
Route::any('api/widget-vue', 'ApiController@widgetVueData');
```

Note: this is a different convention than the datatables version.

The route assumes you have a controller named ApiController with a widgetVueData method.  Obviously, if your model is something other than Widget, you would substitute the model name for widget.

Our **[make:crud](#makecrud)** and **[make:foundation](#makefoundation)** commands will build the controller for you or you have to do it on
your own.

I’m using any as the verb here so I can do a get request to debug.  As a basic example, your api controller could look
like this:

~~~~

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use DB;

class ApiController extends Controller
{
  public function widgetVueData()
      {
 
          $widgets = DB::table('widgets')
                   ->select('id as Id',
                            'widget_name as Name',
                            'created_at as Created')
                   ->paginate(10);
 
          return response()->json($widgets);
 
      }
}

~~~~

Obviously, you can add new columns to the select statement as you require them, as long
as you have added them in your db.  You would also have to have a corresponding table
row in your grid on the index view and add the column name or names in the vue component.

Vue.js is a popular javascript library, the docs are here:

[Vue.js](https://vuejs.org/)

What you get is a working, ajax-powered vue.js grid, but it's just a starting point.  If you
are just starting with Vue, it will give you some idea of how it works.

## Conventions

If you are using our make:foundation or make:crud command to create your routes, models, controllers, etc, these are the conventions they follow, but all the work is done for you, so you don't have to worry about missing something.

If you are making your own models, routes, controllers, etc., it's important to reference things correctly or the views will not work.

### Models

When inputting model names, you have some options.  Ultimately ViewMaker will convert it to the proper format as long as it close.

The best way to input the model name is to stick with Laravel’s conventions.  Here are a couple of examples:

~~~~

php artisan make:views Widget master dt

~~~~

For compound words:

~~~~

php artisan make:views BigWidget master dt

~~~~

To make things easier, and to not have to delete a lot of files over a typo, I built in some flexibility into how you can supply the model name to the commands.

For models with a single word, you can also use the lowercase version of the word as the first argument of the command:

~~~~

php artisan make:foundation widget master dt

~~~~


In this case, widget represents the Widget model. ViewMaker will convert it automatically to uppercase for use in the appropriate places in the templates.

If you have model with compound words, for example, AlphaWidget, then you can type it as the lowercase, separated by a dash:

~~~~

php artisan make:foundation alpha-widget master dt

~~~~


Note, whether you use AlphaWidget or alpha-widget, your route would be:

~~~~

Route::resource('alpha-widget', 'AlphaWidgetController');

~~~~

ViewMaker will convert plural in model names to singular, which is a handy protector against making mistakes.

### Routes

Routes for models with a single word take on the lowercase value of the model. For example, for the Widget model, you would have the following route:

~~~~

Route::resource('widget', 'WidgetController');

~~~~


Routes for models made up of compound words, will have a dash separating them. For example, for the AlphaWidget model, you would have the following route:

~~~~

Route::resource('alpha-widget', 'AlphaWidgetController');

~~~~

### Api Routes

If you wish the ajax calls to work out of the box for datatables and vue.js, then you need to follow the conventions when naming the api routes, assuming you are not using make:foundation to build them for you.

The following routes are an example for the datatables api routes.

Single word model:

~~~~

Route::any('api/widget', 'ApiController@widgetData');

~~~~


Multiple world model:

~~~~

Route::any('api/alpha-widget', 'ApiController@alphaWidgetData');

~~~~


For vue.js, use the following api route convention:

~~~~

Route::any('api/widget-vue', 'ApiController@widgetVueData');

~~~~


For multiple word models and using vue.js:

~~~~

Route::any('api/beta-widget-vue', 'ApiController@betaWidgetVueData');

~~~~


You are free to deviate from this as you wish, however you will need to overwrite the parts of the index.blade.php file that make the api call with your own values.

### View Folder Conventions

View folders will be created with the lowercase string value of the model name. If you have a multi-word model, for example, AlphaWidget, the name of your view folder would also be alpha-widget, so in your controller methods, you do the following for create view for example:

____________________________________________________________________________

~~~~

public function create()
{
  return view('alpha-widget.create');
}

~~~~

____________________________________________________________________________


### Field Names

ViewMaker templates are built with a single field, with the following convention:

~~~~

$modelName . '_name'

~~~~


So that would mean that you have to have a widget_name column in your db, if you are following the Widget model example.

A quick note on why I do it this way. You could have several models with a name attribute. By making the model part of the name of the attribute, there is never confusion between widget_name and product_name, for example, and that makes working with queries easier in the long run.

This convention will work great in most cases, but obviously there are some cases where it will have to be changed, for example if want to use it on a user model, you would end up with user_name, which in the migration in Laravel is actually just ‘name’. Stick with Laravel defaults if you can.

So if you have situation like that or if you use a different convention, it’s ok, just change it after the files are made. You will have to work on these files anyway, since it’s unlikely you will build models with a single field name. At any rate, it’s meant to be a starting point, a fast way to get up and running.

Also note that this convention is only for the initial field supplied with the template. Any fields that you add to your models and tables are completely at your discretion, since you will have to add those yourself anyway.


## Support ViewMaker

I hope you enjoy this plugin and find it useful.  I don’t have a donate button, but If you would like
to support my work and learn more about Laravel, you can do so by buying one of
my books, **[Laraboot: laravel 5.2 For Beginners](https://leanpub.com/laravel-5-for-beginners-laraboot)**,
I really appreciate it.

## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.


## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CONDUCT](CONDUCT.md) for details.

## Security

If you discover any security related issues, please email ikon321@yahoo.com instead of using the issue tracker.

## Credits

- [Bill Keck](https://github.com/evercode1)


## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/evercode1/view-maker.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/evercode1/view-maker/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/evercode1/view-maker.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/evercode1/view-maker.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/evercode1/view-maker.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/evercode1/view-maker
[link-downloads]: https://packagist.org/packages/evercode1/view-maker/stats
[link-author]: https://github.com/evercode1