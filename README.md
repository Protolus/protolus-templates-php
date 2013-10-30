protolus-templates-php 
===========

Protolus templates are a relative of the Smarty syntax, focused on testing and modularity/reusability through template recursion and view-based rendering control to enable [MVVM](http://russelleast.wordpress.com/2008/08/09/overview-of-the-modelview-viewmodel-mvvm-pattern-and-data-binding/).

Template Recursion
------------------

The templates started as an experiment to validate recursive smarty rendering in php, and slowly extended from there incorporating multi-level controllers, wrappers, 2nd pass javascript injection and pluggable tags. The basic concept is that a unit of work is a 'panel' which is a chunk of html which adds a Smarty-like macro language to add render logic to the basic HTML, on top of this we add a number of macros which add awareness of our environment along with macros to control special features.

View Based Control
------------------

The key feature is render flow is controlled by the view rather than the 'controller' logic (which only serves to populate data for a given interface). So you can stub out an entire interface without touching any controllers.

![Screenshot](http://wiki.protol.us/images/9/9e/ProtolusRenderTree.png )

Macros
------

1. **page**
    This is where we set the main data for the page macro, and should be set on the entry panel of all requests (rather than us manually setting them in the controller, and thus obfuscating it from the GUI layer).
    1. title : this is the actual title tag content
    2. heading: page heading, used as a subsection label, but can be used for most anything.
    3. meta: text for the meta tag
    4. wrapper: this is the wrapper that we will render the page inside of
    
    **example:**
    
        {page title="My Homepage" wrapper="homepage_wrapper" meta="My Homepage Meta"}

2. **panel**
    This is where a subpanel is rendered as well as a test is defined. In order to render a panel:
        
        {panel name="path/relative/to/panel/root"}
        
3. **value**
    Any value can be be output in the form:
        
        {$var}
        
    or
        
        {$var.subvalue}
        
4. **if**
    the if construct allows you to conditionally execute logic, for example:
            
        {if $bobMckenzieIsDirecting}
            <!--Act!-->
        {/if}
            
    it also supports else clauses:
            
        {if $myList.length > 0}
            <!--iterate over list-->
        {else}
            <!--show 'no data' state-->
        {/if}
5. **foreach**
    You can iterate across a collection using the foreach macro which supports 3 attributes
    1. from : the object or array we are iterating over
    2. item : the variable name we store the current iteration's value in
    3. key : the variable name we store the current iteration's key in
    
    as an example:

        {foreach from="$thing" item="item" key="key"}
            <li>{$key}:{$item}</li>
        {/foreach}
5. **literal**
    An encapsulation to prevent '{}' from attempting to parse as macros, useful for wrapping inline js or css.

6. **and more!**
    Complete support of all built-in functions,modifiers and variables available in [Smarty 2](http://www.smarty.net/docsv2/en/language.builtin.functions.tpl). 

'Controllers'
-------------

A controller is just an arbitrary piece of php which allows you to register data with the view's renderer. This is exposed as '$renderer' in the scope of the controller where you do the assignments.
        
        $renderer->set('myValue', $aVariable);
        
You can use the data layer, interact with an API or manually manage these resources the controllers are straight php, so they are agnostic to the origin of the data.
    
Wrappers
--------

Wrappers are a special kind of template which renders upon completion of the inner content generation and serve as a container for global page structure which you want to persist across many pages. The wrapper, like a panel, also has an optional controller, and it attempts to write the rendered page into the 'content' variable.

Resources may be targeted to outputs (by default all output will be to 'HEAD' which is assumed to be at the bottom of your HEAD tag) which occur within the wrapper. Let's look at the simplest working example: sample.wrapper.tpl

    <html>
        <head>
            <title>WTFBBQ!</title>
            <!--[HEAD]-->
        </head>
        <body>
            {$content}
        </body>
    </html>
    
Usage
-----

If you find any rough edges, please submit a bug!

Enjoy,

-Abbey Hawk Sparrow