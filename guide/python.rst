
.. _python_primer:

Introduction to Python
======================

.. note:: This is intended to be a *very* brief overview/reference of the various
          topics you need to master in order to program Python. This is not an
          exhaustive guide to programming with python. We recommend other
          resources to really learn that in-depth:

          * `List of various guides to learn Python <http://docs.python-guide.org/en/latest/intro/learning/>`_
          * `CodeAcademy <http://www.codecademy.com/tracks/python>`_
          * `Python 3.5 Tutorial <https://docs.python.org/3.5/tutorial/>`_
          
          If you want to practice some of these concepts, try out
          `pybasictraining <https://github.com/virtuald/pybasictraining>`_!

Language elements
-----------------

Comments
~~~~~~~~

Comments are not functional and do not do anything. They are intended to
be human readable things that help others understand what the code is
doing. Comments should be indented at the same level as surrounding code

::

    # This is a comment. It starts with a '#' character

Indentation
~~~~~~~~~~~

All code should be indented in multiples of 4 spaces. Tab characters
should be avoided. Anytime you see a ``:`` character at the end of a
line, the next line(s) should have another level of indentation. This is
a visual indicator to show that the following lines are part of a block
of code associated with the previous line. For example:

::

    # this is good
    if x == True:
        do_something()

    # this is bad and will not work
    if x == True:
    do_something()

Pass
~~~~

pass is a null operation — when it is executed, nothing happens. It is
useful as a placeholder when a statement is required syntactically, but
no code needs to be executed. **Most finished code will not use pass for
anything.**

::

    if False:
        pass

Numbers
~~~~~~~

You can use numbers in Python, and perform computations on them.

::
   
    # A number
    1
   
    # Multiplying two numbers
    2*2

Strings
~~~~~~~

Strings are groups of words that you can use as variables, and the program can
manipulate them and pass them around.

::

    "This is a string"
    'This is also a string'
    '''This is a string that can 
       be extended to multiple lines'''
    """This is also
       a multiline string"""

Booleans
~~~~~~~~

Boolean values are those that are True or False. In python, True and False always
have the first letter capitalized.

Variables
---------

Variables are used to store some information (strings, numbers, objects, etc).
They can be assigned values, and referred to later on. 

::

    x = 1
    x = 'some value'

Control Flow
------------

If
~~

If statements allow you to control the flow of the program and make decisions
about what the program should do next, based on information retrieved
previously. Note that the body of the if statement is indented::

    if statement is True:
        # do_something here
    elif other_statement is True:
        # do something lese here
    else:
        # otherwise do this

Also see the `python
documentation <http://docs.python.org/dev/tutorial/controlflow.html#if-statements>`_.

Operations
----------

Python supports various types of operations on variables and constants:

::

    # Addition
    1 + 2
    x + 1
    
    # Multiplication
    1 * 2
    x * 2
    
    # Equality operator (evaluated to True or False)
    1 == 1
    x == 1
    
    # Less than
    x < 2
    
    # Lots more!

Functions
---------

Functions are blocks of code that can be reused and are useful for grouping
code. They can return a value to the caller. The code that is contained inside
of a function is not executed until you call the function.

Defintion
~~~~~~~~~

To define a function, you start with the word ``def``, followed by the name 
of the function, and a set of parentheses::
    
    def function_name():
        '''String that describes what the function does'''
        pass

Functions can accept input from their callers, allowing them to be reused for
many purposes. You place the names of the parameters inside the parentheses::

    def function_name(parameter1, parameter2):
        pass

After computing a result, you can return it to the caller. You can also return
constants or variables::

    def function_returns_computation(parameter1, parameter2):
        return parameter1 + parameter2

    def function_returns_a_variable():
        x = 1
        return x
        
    def function_returns_a_value():
        return True

Calling a function
~~~~~~~~~~~~~~~~~~

The code that is contained inside of a function is not executed until
you call the function. You call it by specifying the name of the function,
followed by parentheses::

    # Calling a function that takes no parameters
    function_name()

If you wish to pass data to the function, you place the variable names (or constants)
inside of the parentheses::

    # Calling a function with two constant parameters
    return_value = function_name(1, 2)
    
    # Calling a function with two variables
    return_value = function_name(x, y)

Classes
-------

A collection of functions (also called methods) and variables can be put into a
logical group called a 'class'.

Definition
~~~~~~~~~~

A class named ``Foo``::

    class Foo(object):
        '''String that describes the class'''
     
        def __init__(self):
            '''Constructor -- this function gets called when an instance is created'''
             
            # store a variable in the class for use later
            self.variable = 1
     
        def method(self, parameter1, optional_parameter=None):
            '''A function that belongs to the Foo class. It takes
               two arguments, but you can specify only one if you desire'''
            pass

A class named ``Bar``

::

        class Bar(Foo):
            '''This class inherits from the Foo class, so anything in
               Foo is transfered (and accessible) here'''
               
            def __init__(self, parameter1):
                pass

Creating an instance
~~~~~~~~~~~~~~~~~~~~

To actually use a class, you must create an instance of the class. Each instance
of a class is unique, and usually operations on the class instances do not
affect other instances of the same class.

::

    foo = Foo()

    # These are two separate instances of the Bar class, and operations on one
    # do not affect the other
    bar1 = Bar(1)
    bar2 = Bar(1)

Accessing variables stored in a class instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    x = Foo()          # creates an instance of Foo
    y = x.variable     # get the value from the instance
    x.variable = 1     # set the value in the instance

Calling functions (methods) on a class instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    x = Foo()     # this creates an instance of Foo
    x.method()    # this calls the function

    
Loops
-----

for
~~~

::

    for i in a_list_of_things:
        print(i)

while
~~~~~

::

    while statement is True:
        do_this_until_statement_is_not_true()

Exceptions
----------

::

    try:
        do_something_that_might_cause_an_exception()
        if bad_thing is True:
            raise SomeException()
    except ExceptionType as e:
        # this code is only executed if an ExceptionType exception is raised
        print("Error: " + e)
    finally:
        # This is always executed
        clean_up()

    try:
        import wpilib
    except ImportError:
        import fake_wpilib as wpilib

Future topics
-------------

-  Lists, dictionaries, tuples
-  Scope

Next Steps
----------

Learn about the basic structure of Robot code at :ref:`anatomy`.
