# PyCascades 2019

## Day 1 — Saturday, 23 February 2019


### Katie McLaughlin: Turning 'wat' into 'why'

Every language has a bunch of 'wat'? But usually there is a logical 'why'. Lots of laughs in this one

### Al Sweigart: The Amazing Mutable, Immutable Tuple

Every object has an [*idntity, type and value*](https://docs.python.org/3/reference/datamodel.html)

> An object’s identity never changes once it has been created; you may think of it as the object’s address in memory.

Where identity is comared with `is`, the values are compared (and deinfed by) the equality operator `==`.

The point of this talk is that tuples are not exactly immutable. For examlpe:

`a = (1, 3, ['cat'])`

can be changed to

`a = (1, 3, ['apple', 'dog'])`

The object types must stay the same however, and you can't change the integer values.

### Jigyasa Grover: Perceiving Python Programming Paradigms

Pretty boring talk. Had a good cringe when the speaker used `list` as a variable name.

### Iyanuoluwa Ajao: Type Hints: Adding Types to Legacy Code

This talk didn't happen... maybe later?

### Paul Watts: Given this, assert that: fluent testing using fixtures and properties

Tests should be formatted in 3 blocks: arrange objects, act on them, then assert something. Pytest fixtures take care of the "arrange" step. Keep in mind, if you don't need to reuse the test setup then fixtures are not really important. However, using fixtures when setup code must be reused enables you to write smaller and more focused tests without duplication.

Can use [Hypothesis](https://hypothesis.readthedocs.io/en/latest/) to automatically generate test inputs. These are "property based" tests.

When writing a test, start with the docstring

```python
test_get():
    """
    Given a widget and authorized user,
    we can use the API GET method.
    """
```

Speaker used some really neat type hinting. [For example](https://stackoverflow.com/questions/24853923/python-3-annotations-type-hinting-a-list-of-a-specified-type-pycharm):

```
from typing import List

def my_func(l: List[int]):
    pass
```

### Lightning Talks

```
#NEVERFORGET
#NEVERUNPICKLE
#CUCUMBERGONE
#ITSNOTCOMINGBACK
```

### Norah Klintberg Sakal: Guide to your own artificial intelligence application in 3 easy steps

This is my favorite talk so far, very good slides and cool project.

For image classification, you can pick up a pretrained CNN (e.g. VGG16) off the shelf (this knows smart convolutions to interpret the images) and train a model to classify your specific use case.

[Talk source on GitHub](https://github.com/norahsakal/pycascades-2019-shades/blob/master/train_your_model.ipynb)

[Helpful example of retraining model from VGG16](https://github.com/keras-team/keras/issues/4465)

[Related guide for classification with VGG16](https://machinelearningmastery.com/use-pre-trained-vgg-model-classify-objects-photographs/)

[Another interesting post with VGG16](https://medium.com/@franky07724_57962/using-keras-pre-trained-models-for-feature-extraction-in-image-clustering-a142c6cdf5b1)

### Ania Kapuścińska: Lint your code responsibly!

Lots of interesting things that you can do with code linting. The speaker went through a variety of options for linting with python and custom linting. If you're into that sort of thing then this would be a great talk to check out.

### Maria McKinley: Hunting the bugs

Strategies for hunting bugs:
 - Take a walk
 - Write stuff down - what do you expect? what are you seeing?

### Ben Berry: Who to blame for all your problems

A postmortem is a meeting (and doc) whose purpose is to review a system failure.

Do not include blame in a postmortem. This encourages open and honest feedback

Outline of a postmordem:
 - date
 - authors
 - status
 - summary
 - details
 - causes
 - trigger (how did we notice the bug?)
 - resolution

Before the postmordem, someone should fill in the outline of the doc: date, summary, trigger. The remainder can be figured out during the meeting. This should be done ASAP (within a few days) after the incident.

During the meeting, focus on working through the timeline (how the error happened from dev to trigger) in as much detail as possilbe.

### Hayley Denbraver: Recursion, Fractals, and the Python Turtle Module

Python [turtle module](https://docs.python.org/3.3/library/turtle.html?highlight=turtle) is pretty neat. You can make pretty things. But it is pretty useless though.


## Day 2 — Sunday, 24 February 2019

### Nina Zakharenko: Light Up Your Life - With Python and LEDs!

Using micropython for coding little adafruit LEDs. Cool live demo!

### Dustin Ingram: Data Protection for Developers: Past, Present, and Future

Companies must inform users that their data is being collected, and how it's being used.

Data breaches must be reported within 72 hours of the incident - or face fines.

A few laughs but overall pretty serious talk. I wish he presented the [walrus operator](https://www.youtube.com/watch?v=mHOgGuALfNc) again instead!

### Omayeli Arenyeka: Building a Gendered Dictionary

Very cool talk about the speakers journey of this project. 

Good subtle reminder in here to use system variables to store API keys (e.g. `os.environ`).

Neat API for doing reverse dictionary lookups, where you find words that have a given word in the definition (I missed the website :( but check out the talk video for this).

Neat use of BeautifulSuop API: you can do `soup.find('td > a')` to find all links that are children of `td`! ([another example here](https://stackoverflow.com/a/49968385/3511819))


### Andy Fundinger: A Taxonomy of Decorators: A-E

Decorators can be applied to classes as well as functions.

[Talk notes](https://github.com/bloomberg/decorator-taxonomy) are very good to reference for decorator basics e.g. decorators that can take arguments.

You can use classes to write decorators!!!

```
import random

class trace_it:
    def __init__(self, func):
        self.func = func
    def __call__(self, *args, **kwargs):
        print(args, kwargs)
        return self.func(*args, **kwargs)
        
@trace_it
def rand(min_val, max_val):
    return random.randint(min_val, max_val)
rand(10,30)

>>> (10, 30) {}
```

A possilbe use case is for retrying functions (a "control flow" decorator) e.g.

```
def infinite_retry(func):
    def wrapper(*args, **kwargs):
        while True:
            try:
                return func(*args, **kwargs)
            except RuntimeError as e:
                print(e)
    return wrapper

@infinite_retry
def random_fail(max_value):
    ret = random.randint(-100, max_value)
    if ret<0:
        raise RuntimeError("Invalid negative number {ret}".format(ret=ret))
    return ret

random_fail(10)
```

Descriptive decorators could be used to label / bin functions somehow. E.g.

```
import warnings 

to_qa=[]
def qa(func):
    to_qa.append(func.__name__)
    return func

@qa
def new_code(): pass

@qa
def refactored_code(): pass

def well_trusted_code(): pass
```

To quickly set up caching on a function use [functools.lru_cache](https://docs.python.org/3/library/functools.html#functools.lru_cache)!!!


### Chirag Shah: Understanding Multithreading

In python, the GIL makes *multithreading* really tough

Notes on the GIL:
 - Everything in python is an object in C (PyObject)
 - Variables are references in memory
 - Without the GIL these references may overlap (???)
 - Using threads holds the GIL while running and releases it when doing i/o, number crunching, etc...

These complications are seen as an issue by the community and they are looking to either optimize or remove the GIL.

### Kyungyun Lee: So tell me, what is your musical taste?


### Chris Waigl: Abstraction for students of all the things


### Abhishek Kapatkar: Nim for Python Programmers




### Philip James and Asheesh Laroia: Account Security for the Fashionable App Developer


### Trey Hunner: Meaningful Mentoring Moments


