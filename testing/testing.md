**Based on materials by Katy Huff and Rachel Slaybaugh**

What is testing?
----------------
 
Software testing is a process by which one or more expected behaviors and results from a piece of software are exercised and confirmed. Well chosen tests will confirm expected code behavior for the extreme boundaries of the input domains, output ranges, parametric combinations, and other behavioral edge cases.
 

Why test?
---------

Unless you write flawless, bug-free, perfectly accurate, fully precise, and predictable code every time, you must test your code in order to trust it enough to answer in the affirmative to at least a few of the following questions:

    Does your code work?
    Always?

Does it do what you think it does?

Does it continue to work after changes are made?

Does it continue to work after system configurations or libraries are upgraded?

Does it respond properly for a full range of input parameters?

What about edge or corner cases?

What’s the limit on that input parameter?


**Verification**

Verification is the process of asking, “Have we built the software correctly?” That is, is the code bug free, precise, accurate, and repeatable?

**Validation**
 
Validation is the process of asking, “Have we built the right software?” That is, is the code designed in such a way as to produce the answers we’re interested in, data we want, etc.

Where are tests ?
Say we have an averaging function:

::

  def mean(numlist):
     total = sum(numlist)
     length = len(numlist)
     return total/length

The test could be runtime exceptions in the function.

::

  def mean(numlist):
     try :
         total = sum(numlist)
         length = len(numlist)
     except ValueError :
         print "The number list was not a list of numbers."
     except :
         print "There was a problem evaluating the number list."
     return total/length


Sometimes they’re alongside the function definitions they’re testing.

::

 def mean(numlist):
    try :
        total = sum(numlist)
        length = len(numlist)
    except ValueError :
        print "The number list was not a list of numbers."
    except :
        print "There was a problem evaluating the number list."
    return total/length
 
 class TestClass:
    def test_mean(self):
        assert(mean([0,0,0,0])==0)
        assert(mean([0,200])==100)
        assert(mean([0,-200]) == -100)
        assert(mean([0]) == 0)
    def test_floating_mean(self):
        assert(mean([1,2])==1.5)

Sometimes they’re in an executable independent of the main executable.

 
::

 def mean(numlist):
    try :
        total = sum(numlist)
        length = len(numlist)
    except ValueError :
        print "The number list was not a list of numbers."
    except :
        print "There was a problem evaluating the number list."
    return total/length

Where, in a different file exists a test module:


::

  import mean
  class TestClass:
      def test_mean(self):
          assert(mean([0,0,0,0])==0)
          assert(mean([0,200])==100)
          assert(mean([0,-200]) == -100)
          assert(mean([0]) == 0)
      def test_floating_mean(self):
          assert(mean([1,2])==1.5)


**When should we test?**

The short answer is all the time. The long answer is that testing either before or after your software is written will improve your code, but testing after your program is used for something important is too late.

If we have a robust set of tests, we can run them before adding something new and after adding something new. If the tests give the same results (as appropriate), we can have some assurance that we didn’t break anything. The same idea applies to making changes in your system configuration, updating support codes, etc.

Another important feature of testing is that it helps you remember what all the parts of your code do. If you are working on a large project over three years and you end up with 200 classes, it may be hard to remember what the widget class does in detail. If you have a test that checks all of the widget’s functionality, you can look at the test to remember what it’s supposed to do.

**Who tests?**
In a collaborative coding environment, where many developers contribute to the same code, developers should be responsible individually for testing the functions they create and collectively for testing the code as a whole.

Professionals invariably test their code, and take pride in test coverage, the percent of their functions that they feel confident are comprehensively tested.

**How does one test?**

The type of tests you’ll write is determined by the testing framework you adopt.

**Types of Tests:**
*Exceptions*
Exceptions can be thought of as type of runttime test. They alert the user to exceptional behavior in the code. Often, exceptions are related to functions that depend on input that is unknown at compile time. Checks that occur within the code to handle exceptional behavior that results from this type of input are called Exceptions.

*Unit Tests*

Unit tests are a type of test which test the fundametal units a program’s functionality. Often, this is on the class or function level of detail.

To test functions and classes, we want to test the interfaces, rather than the implmentation. Treating the implementation as a ‘black box’, we can probe the expected behavior with boundary cases for the inputs.

In the case of our fix_missing function, we need to test the expected behavior by providing lines and files that do and do not have missing entries. We should also test the behavior for empty lines and files as well. These are boundary cases.

*System Tests*

System level tests are intended to test the code as a whole. As opposed to unit tests, system tests ask for the behavior as a whole. This sort of testing involves comparison with other validated codes, analytical solutions, etc.

*Regression Tests*

A regression test ensures that new code does change anything. If you change the default answer, for example, or add a new question, you’ll need to make sure that missing entries are still found and fixed.

*Integration Tests*

Integration tests query the ability of the code to integrate well with the system configuration and third party libraries and modules. This type of test is essential for codes that depend on libraries which might be updated independently of your code or when your code might be used by a number of users who may have various versions of libraries.

**Test Suites**
Putting a series of unit tests into a suite creates, as you might imagine, a test suite.

**Elements of a Test**

**Behavior**

The behavior you want to test. For example, you might want to test the fun() function.

**Expected Result**

This might be a single number, a range of numbers, a new, fully defined object, a system state, an exception, etc.

When we run the fun function, we expect to generate some fun. If we don’t generate any fun, the fun() function should fail its test. Alternatively, if it does create some fun, the fun() function should pass this test.

**Assertions**

Require that some conditional be true. If the conditional is false, the test fails.

**Fixtures**

Sometimes you have to do some legwork to create the objects that are necessary to run one or many tests. These objects are called fixtures.

For example, since fun varies a lot between people, the fun() function is a member function of the Person class. In order to check the fun function, then, we need to create an appropriate Person object on which to run fun().

**Setup and teardown**

Creating fixtures is often done in a call to a setup function. Deleting them and other cleanup is done in a teardown function.

**The Big Picture**
Putting all this together, the testing algorithm is often:

::

  setUp
  test
  tearDown


But, sometimes it’s the case that your tests change the fixtures. If so, it’s better for the setup and teardown functions to occur on either side of each test. In that case, the testing algorithm should be:

::

  setUp
  test1
  tearDown
  setUp
  test2
  tearDown
  setUp
  test3
  tearDown

----------------------------------------------------------
Python Nose
---------------------------------------------------------- 

The testing framework we’ll discuss today is called nose, and comes packaged with the enthought python distribution that you’ve installed.

**Where is a nose test?**

Nose tests are files that begin with Test-, Test_, test-, or test_. Specifically, these satisfy the testMatch regular expression [Tt]est[-_]. (You can also teach nose to find tests by declaring them in the unittest.TestCase subclasses chat you create in your code. You can also create test functions which are not unittest.TestCase subclasses if they are named with the configured testMatch regular expression.)

Nose Test Syntax
To write a nose test, we make assertions.

::

    assert (ShouldBeTrue())
    assert (not ShouldNotBeTrue())


In addition to assertions, in many test frameworks, there are expectations, etc.

**Add a test to our work**

There are a few tests for the mean function that we listed in this lesson. What are some tests that should fail? Add at least three test cases to this set.

*Hint: think about what form your input could take and what you should do to handle it. Also, think about the type of the elements in the list. What should be done if you pass a list of integers? What if you pass a list of strings?*

**Test Driven Development**

Some people develop code by writing the tests first.

If you write your tests comprehensively enough, the expected behaviors that you define in your tests will be the necessary and sufficient set of behaviors your code must perform. Thus, if you write the tests first and program until the tests pass, you will have written exactly enough code to perform the behavior your want and no more. Furthermore, you will have been forced to write your code in a modular enough way to make testing easy now. This will translate into easier testing well into the future.

--------------------------------------------------------------------
An example
--------------------------------------------------------------------
The overlap method takes two rectangles (red and blue) and computes the degree of overlap between them. Save it in overlap.py. A rectangle is defined as a tuple of tuples: ((x_lo,y_lo),(x_hi),(y_hi))

::

 def overlap(red, blue):
    '''Return overlap between two rectangles, or None.'''

    ((red_lo_x, red_lo_y), (red_hi_x, red_hi_y)) = red
    ((blue_lo_x, blue_lo_y), (blue_hi_x, blue_hi_y)) = blue

    if (red_lo_x >= blue_hi_x) or \
       (red_hi_x <= blue_lo_x) or \
       (red_lo_y >= blue_hi_x) or \
       (red_hi_y <= blue_lo_y):
        return None

    lo_x = max(red_lo_x, blue_lo_x)
    lo_y = max(red_lo_y, blue_lo_y)
    hi_x = min(red_hi_x, blue_hi_x)
    hi_y = min(red_hi_y, blue_hi_y)
    return ((lo_x, lo_y), (hi_x, hi_y))


Now let's create a set of tests for this class. Before we do this, let's think about *how* we might test this method. How should it work?


::

 from overlap import overlap

 def test_empty_with_empty():
    rect = ((0, 0), (0, 0))
    assert overlap(rect, rect) == None

 def test_empty_with_unit():
    empty = ((0, 0), (0, 0))
    unit = ((0, 0), (1, 1))
    assert overlap(empty, unit) == None

 def test_unit_with_unit():
    unit = ((0, 0), (1, 1))
    assert overlap(unit, unit) == unit

 def test_partial_overlap():
    red = ((0, 3), (2, 5))
    blue = ((1, 0), (2, 4))
    assert overlap(red, blue) == ((1, 3), (2, 4))


Run your tests.

::

 [rguy@infolab-33 ~/TestExample]$ nosetests
 ...F
 ======================================================================
 FAIL: test_overlap.test_partial_overlap
 ----------------------------------------------------------------------
 Traceback (most recent call last):
   File "/usr/lib/python2.6/site-packages/nose/case.py", line 183, in runTest
     self.test(*self.arg)
   File "/afs/ictp.it/home/r/rguy/TestExample/test_overlap.py", line 19, in test_partial_overlap
     assert overlap(red, blue) == ((1, 3), (2, 4))
 AssertionError

 ----------------------------------------------------------------------
 Ran 4 tests in 0.005s

 FAILED (failures=1)


Oh no! Something failed. The failure was on line in this test:

::

 def test_partial_overlap():
   red = ((0, 3), (2, 5))
   blue = ((1, 0), (2, 4))
   assert overlap(red, blue) == ((1, 3), (2, 4))


Can you spot why it failed? Try to fix the method so all tests pass.
