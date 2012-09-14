Genshi
======

Genshi is a Python template engine: <http://genshi.edgewall.org/>.

These are quick reference notes on the Genshi XML template language, mostly
summarised from the Genshi website, with some of my own tips added.

Genshi and XInclude XML Namespaces
----------------------------------

Genshi directives (see below) come from the genshi.edgewall.org namespace, so
you need this at the top of your file:

    <html xmlns="http://www.w3.org/1999/xhtml"
          xmlns:py="http://genshi.edgewall.org/"
          lang="en">
          xmlns:xi="http://www.w3.org/2001/XInclude">
      ...
    </html>

The `xmlns:xi` line is for the XInclude namespace that Genshi uses for template
includes (see below).

Substituting Python Expressions
-------------------------------

**Substitute** a simple Python expression into the template:

    $some_expression

The expression must begin with a letter and contain only letters, digits, dots
and underscores. If you need to use some other character e.g. a space in your
expression you need curly brackets:

    ${...}

You can substitute any type of Python variable or expression into the template
like this.  
The expression will be evaluated against the template data, and the result
converted into a string.

Genshi Directives
-----------------

**If** directives look like this:

    <ol py:if="links">
      ...
    </ol>

This `<ol>` will only appear if `"links"` evaluates to `True`.  
`"links"` can be any Python expression.  

All `py:` directives can be used as an attribute on an XML element (as above)
or as XML elements themselves. When `py:if` is used as an element you have to
put the Python test expression as the value of an attribute called test:

    <py:if test="foo">
      ...
    </py:if>

I think the attribute form is nicer and the element form should only be used
when the attribute form won't work for some reason.

**Choose** directives work like if/elif/else. In this example the value of the
`py:choose` attribute is an empty string, so each `py:when` directive will be
tested for truth, the first true `py:when` will be rendered or if they're all
false the `py:otherwise` will be rendered:

    <div py:choose="">
      <span py:when="0 == 1">0</span>
      <span py:when="1 == 1">1</span>
      <span py:otherwise="">2</span>
    </div>

If the value of the `py:choose` attribute is a Python expression, the result of
each `py:when` will be tested against the result of the `py:choose` instead of
tested for truth:

    <div py:choose="1">
      <span py:when="0">0</span>
      <span py:when="1">1</span>
      <span py:otherwise="">2</span>
    </div>

As with all `py:` directives, choose and when can be used as elements:

    <py:choose test="1">
      <py:when test="0">0</py:when>
      <py:when test="1">1</py:when>
      <py:otherwise>2</py:otherwise>
    </py:choose>

**For** directives look like this:

    <li py:for="link in reversed(links)">
      ...
    </li>

This `<li>` will be repeated for every link in links.  
Again, `"link in reversed(links)"` can be arbitrarily complex Python.

For as an element:

    <ul>
      <py:for each="item in items">
        <li>${item}</li>
      </py:for>
    </ul>

`py:with` lets you assign expressions to variables, like Python's `with`
statement:

    <span py:with="y=7; z=x+10">$x $y $z</span>

or as an element:

    <py:with vars="y=7; z=x+10">$x $y $z</py:with>

But you cannot use with to override names that already exist, the name will
have the same value inside the with as outside, the attempt to override it will
be ignored. This seems like an easy way to introduce confusing errors.

`py:attrs` takes a dictionary and applies it as attributes to an element, e.g:

    <li py:attrs="foo">Bar</li>

given `foo={'class': 'collapse'}` in the template context would produce:

    <li class="collapse">Bar</li>

Dictionary keys with value `None` are omitted, so if you had `foo={'class':
None}` you would get:

    <li>Bar</li>

`py:content` replaces the content of the XML element with the result of
evaluating the Python expression.  
`py:replace` replaces the XML element itself with the result of evaluating the
Python expression.  
`py:strip` conditionally strips the top-level element from the output.  
These three don't seem particularly useful to me.

Accessing Dictionary Items and Object Attributes
------------------------------------------------

You can **access dictionary items as if they were object attributes**:

    ${dict.foo}

but I don't see the point of this, it's just inconsistent with Python (and with
Python code blocks in Genshi, see below).  
(You can also access dictionary items as if they were object attributes!)

Testing whether a Name, Attribute or Key Exists
-----------------------------------------------

Use **defined** or **value_of** to check whether a variable exists in the
template context:

`defined("name")` returns `True` if the name exists in the template
context, `False` otherwise.

`value_of("name", default=None)` returns the value of the name or if the name
doesn't exist returns the default.

Use `hasattr()` and `getattr()` to check whether an object has an attribute.

Use `get()` or `in` to check whether a dictionary has a key.

Code Blocks
-----------

You can put whole **blocks of multiline Python code**:

    <?python
      ...
    ?>

You can use this to import modules, define classes and functions, etc. (almost
anything you could do in Python).  
But you cannot emit content into the generated output from code blocks.  
If you use a `print` statement in a code block it will print to stdout, i.e.
the terminal or log file, not into the template.  
If Genshi's `allow_exec` option is `False` code blocks won't work.  
In code blocks you cannot use dot-notation to access dictionary items, or
dictionary notation to access object attributes!

### Tip: Using Code Blocks for Debugging

You can use a code block to drop into pdb or ipdb from a Genshi template:

    <?python
      import ipdb; ipdb.set_trace()
    ?>

then from the pdb or ipdb shell do `pp dir()` to list all the names in the
current context.

Genshi Macros
-------------

The **`py:def` directive** defines macros (named Genshi functions with
arguments) that can be called from other parts of the template. Define a macro
like this:

    <p py:def="greeting(name)" class="greeting">
        Hello, ${name}!
    </p>

And then use it like a Python function:

    ${greeting('world')}

Here's a macro with no arguments:

    <p py:def="greeting" class="greeting">
      Hello, world!
    </p>

You still need the `()` when calling it:

    ${greeting()}

Like other directives, `py:def` can also be used as an XML element:

    <py:def function="greeting(name)">
      <p class="greeting">Hello, ${name}!</p>
    </py:def>

### Tip: Define Macros in Child Templates

If you have a base template that is included by many child templates, you might
want the child templates to effect part of the base template, e.g. the title of
the page.

Define a macro in the child template:

    <py:def function="page_heading">Edit: ${c.group.display_name}</py:def>

and then use the macro in the base template, if it's defined:

      <h1 py:if="defined('page_heading')" class="page_heading">
        <img py:if="defined('page_logo')" id="page-logo" src="${page_logo()}"
             alt="Page Logo" />
        ${page_heading()}
      </h1>

Match Templates
---------------

The `py:match` directive defines a **match template** which, given an XPath
expression, replaces every element in the template that matches the expression
with its own content.  
For example, you could find all the `<greeting>` elements
and replace them.  
You can incorporate part of the original element into the replacement element
using `select(path)`.  
Match templates are applied in a pipeline, a match template defined after
another match template is applied to the output from the first match template.

I think you should avoiding using match templates, unless you really have to.

Including other Templates
-------------------------

**Include** other template files with `<xi:include>`. You first need to declare
the XInclude namespace:

    <html xmlns="http://www.w3.org/1999/xhtml"
          xmlns:py="http://genshi.edgewall.org/"
          xmlns:xi="http://www.w3.org/2001/XInclude">

Then use `<xi:include>` at the point in your file where you want the external
template to be pulled in:

    <xi:include href="base.html" />

The path is relative to the current file.

An error is raised if the file is not found. You can silence the error by
specifying fallback content to be used when the file is not found, and this
fallback content can be empty:

    <xi:include href="base.html"><xi:fallback /></xi:include>

The `href` attribute is a Python expression, and directive attributes like
`py:if` and `py:for` can be used on `<xi:include>` elements:

    <xi:include href="${name}.html" py:if="not in_popup"
                py:for="name in ('foo', 'bar', 'baz')" />

Comments
--------

You can use normal HTML/XML comments:

    <!-- this is a comment -->

If you want Genshi to strip the comment from the generated output,
put an extra `!`:

    <!-- !this is a comment too, but one that will be stripped from the output -->
    <!--! this is a comment too, but one that will be stripped from the output -->