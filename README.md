# Python Style Guide

This style guide aims to document my preferred style for writing Python code.


It is based on Python [PEP 8][pep8]. Portions of this guide borrow heavily from:

* Google: [C++][google-c++-comments] and [Python][google-python-comments] style guides.
* Airbnb: [Ruby][airbnb-ruby] style guide.
* [Trey Hunner][trey-hunner]: [Python][trey-hunner-python-guide] style guide.

## Goals

The ultimate goal of this guide is having code that is clean, consistent, and efficient. Some parts
of the guide are opinionated and meant to be strictly followed to preserve consistency when writing
new code.

## Table of Contents

<!-- TOC -->

- [Layout](#layout)
    - [Indentation](#indentation)
    - [Inline](#inline)
    - [Line Length](#line-length)
    - [Line Breaks](#line-breaks)
    - [Trailing Commas](#trailing-commas)
    - [Blank Lines](#blank-lines)
    - [Comprehensions](#comprehensions)
    - [Source File Encoding](#source-file-encoding)
    - [Imports](#imports)
    - [String Quotes](#string-quotes)
- [Commenting](#commenting)
    - [Inline Comments](#inline-comments)
    - [Documentation Strings](#documentation-strings)
    - [TODO comments](#todo-comments)
    - [TODO vs FIXME](#todo-vs-fixme)
    - [Commented-out code](#commented-out-code)
- [Naming Conventions](#naming-conventions)
- [Strings](#strings)
- [Regular Expressions](#regular-expressions)
- [Conditionals](#conditionals)
    - [Truthiness](#truthiness)
    - [Long if-elif chains](#long-if-elif-chains)
    - [Conversion to bool](#conversion-to-bool)
- [Recommendations](#recommendations)
    - [Compacting Assignments](#compacting-assignments)
    - [Naming Indexes](#naming-indexes)
    - [Named Tuples](#named-tuples)
    - [Exceptions](#exceptions)
    - [Return statements](#return-statements)
    - [Loops](#loops)
- [Be Consistent](#be-consistent)
- [A Foolish Consistency is the Hobgoblin of Little Minds](#a-foolish-consistency-is-the-hobgoblin-of-little-minds)

<!-- /TOC -->

## Layout

### Indentation

* <a name="spaces-indentation"></a>Use 4 spaces per indentation level.
<sup>[[link](#spaces-indentation)]</sup>

* <a name="continuation-lines"></a>Continuation lines should align wrapped elements either
vertically using Python's implicit line joining inside parentheses, brackets and braces, or using a
hanging indent.<sup>[[link](#continuation-lines)]</sup>
    ```python
    # Yes

    # Aligned with opening delimiter.
    def long_function_name(var_one,
                           var_two,
                           var_three,
                           var_four):

    # Hanging indents should add a level.
    foo = long_function_name(
        var_one,
        var_two,
        var_three,
        var_four,
    )
    ```

    ```python
    # No

    # Arguments on first line are forbidden when not using vertical alignment.
    foo = long_function_name(var_one, var_two,
        var_three, var_four)
    ```

    ```python
    # Good when it fits the line length limit
    def create_translation(phrase_id, phrase_key, target_locale):
        ...

    translation = create_translation(phrase_id, phrase_key, target_locale)

    # Good, but use it only for function definitions
    def create_translation(phrase_id,
                           phrase_key,
                           target_locale,
                           value,
                           user_id,
                           do_xss_check):
        ...

    # Good, stick to one argument or element per line
    # This applies to lists, tuples, sets, dictionaries, function calls
    translation =  create_translation(
        phrase_id,
        phrase_key,
        target_locale,
        value,
        user_id,
        do_xss_check,
    )
    ```

### Inline

* <a name="trailing-whitespace"></a>Never leave trailing whitespace.
<sup>[[link](#trailing-whitespace)]</sup>

* <a name="extraneous-whitespace"></a>Avoid extraneous whitespace in the
[following situations][pep8-whitespace].<sup>[[link](#extraneous-whitespace)]</sup>

* <a name="space-around-equal-sign"></a>Don't use spaces around the = sign when used to indicate a
keyword argument or a default parameter value.<sup>[[link](#space-around-equal-sign)]</sup>

    ```python
    # No
    def func(arg1, arg2 = 2):

    # Yes
    def func(arg1, arg2=2):
    ```

* <a name="compound-statements"></a>Compound statements (multiple statements on the same line) are
generally discouraged.<sup>[[link](#compound-statements)]</sup>
    ```python
    # Yes

    if foo == 'blah':
        do_blah_thing()
    do_one()
    do_two()
    do_three()

    # Rather not

    if foo == 'blah': do_blah_thing()
    do_one(); do_two(); do_three()

    # Definitely not

    if foo == 'blah': do_blah_thing()
    else: do_non_blah_thing()

    try: something()
    finally: cleanup()

    do_one(); do_two(); do_three(long, argument,
                                 list, like, this)

    if foo == 'blah': one(); two(); three()
    ```

### Line Length

* <a name="line-length"></a>Keep each line of code to a readable length. Unless you have a reason
to, keep lines to fewer than 120 characters.<sup>[[link](#line-length)]</sup>

    Long lines mean you are doing too much on a single line.

### Line Breaks

* <a name="closing-brackets"></a>The closing brace/bracket/parenthesis on multiline constructs
should be lined up under the first character of the line that starts the multiline construct.
<sup>[[link](#closing-brackets)]</sup>
    ```python
    # Yes
    my_list = [
        'item 1',
        'item 2',
        'item 3',
        'itme 4',
        'item 5',
        'item 6',
    ]
    result = some_function_that_takes_arguments(
        'arg1',
        'arg2',
        'arg3',
        'arg4',
    )

    # No
    my_list = [
        'item 1',
        'item 2',
        'item 3']

    my_list = [
        'item 1',
        'item 2',
        'item 3'
        ]
    ```

* <a name="binary-ops-line-break"></a>Add line break before binary operators.
<sup>[[link](#binary-ops-line-break)]</sup>

    This applies to `and` and `or` as well
    ```python
    # No: operators sit far away from their operands
    income = (gross_wages +
              taxable_interest +
              (dividends - qualified_dividends) -
              ira_deduction -
              student_loan_interest)

    # Yes: easy to match operators with operands
    income = (
        gross_wages
        + taxable_interest
        + (dividends - qualified_dividends)
        - ira_deduction
        - student_loan_interest
    )
    ```

* <a name="readability-line-breaks"></a>Add line breaks when it improves readability. For instance,
add line breaks between the mapping, looping, and (optional) conditional parts of a comprehension.
<sup>[[link](#readability-line-breaks)]</sup>

    ```python
    # No
    employee_hours = [schedule.earliest_hour for employee in self.employess for schedule in employee.schedules]
    return min(h for h in employee_hours if h is not None)

    # Yes
    employee_hours = [
        schedule.earliest_hour
        for employee in self.employess
        for schedule in employee.schedules
    ]
    return min(
        hour
        for hour in employee_hours
        if hour is not None
    )
    ```

* <a name="short-comprehension"></a>For a very short comprehension, it is acceptable to use just one
line of code.<sup>[[link](#short-comprehension)]</sup>
    ```python
    sum_of_squares = sum(n**2 for n in numbers)
    ```

### Trailing Commas

* <a name="mandatory-trailing-commas"></a>Trailing commas are usually optional, except they are
mandatory when making a tuple of one element.<sup>[[link](#mandatory-trailing-commas)]</sup>

    ```python
    # Yes
    FILES = ('setup.cfg',)

    # OK, but confusing
    FILES = 'setup.cfg',
    ```

* <a name="redundant-trailing-commas"></a>When trailing commas are redundant, they are often helpful
 when a version control system is used, when a list of values, arguments or imported items is
 expected to be extended over time. The pattern is to put each value (etc.) on a line by itself,
always adding a trailing comma, and add the close parenthesis/bracket/brace on the next line.
However it does not make sense to have a trailing comma on the same line as the closing delimiter
(except in the above case of singleton tuples).<sup>[[link](#redundant-trailing-commas)]</sup>

    ```python
    # Yes
    FILES = [
        'setup.cfg',
        'tox.ini',
    ]
    initialize(
        FILES,
        error=True,
    )

    # No
    FILES = ['setup.cfg', 'tox.ini',]
    initialize(FILES, error=True,)
    ```

### Blank Lines

* <a name="blank-lines-top-level"></a>Surround top-level function and class definitions with
**two blank** lines.<sup>[[link](#blank-lines-top-level)]</sup>

* <a name="blank-lines-class-methods"></a>Method definitions inside a class are surrounded by a
**single blank** line.<sup>[[link](#blank-lines-class-methods)]</sup>

* <a name="blank-lines-internal"></a>Use blank lines in functions, sparingly, to indicate logical
sections.<sup>[[link](#blank-lines-internal)]</sup>

### Comprehensions

* <a name="sequences-comprehension"></a>Use comprehensions when possible over writing multiline for
loops.<sup>[[link](#sequences-comprehension)]</sup>
    ```python
    measurements = [1, 1, 2, 3, 4, 5]
    high_measurements = []

    # No
    for m in measurements:
        if m > 3:
            high_measurements.append(m)

    # Yes
    high_measurements2 = [
        m
        for m in measurements
        if m > 3
    ]

    # Yes, returns generator
    high_measurements_gen = (
        m
        for m in measurements
        if m > 3
    )

    # Yes, returns distinct elements (set)
    high_measurements_set_gen = {
        m
        for m in measurements
        if m > 3
    }

    # Yes, for dictionaries
    employees_count = {
        key: value
        for key, value in [('Berlin', 350), ('Zurich', 50)]
    }
    ```

### Source File Encoding

* <a name="source-file-encoding"></a>Files using ASCII (in Python 2) or UTF-8 (in Python 3) should
not have an encoding declaration.<sup>[[link](#source-file-encoding)]</sup>

### Imports

* <a name="imports-lines"></a>Imports should usually be on separate lines and in alphabetic order if
they are too many.<sup>[[link](#imports-lines)]</sup>

* <a name="imports-position"></a>Imports are always put at the top of the file, just after any
module comments and docstrings, and before module globals and constants.
<sup>[[link](#imports-position)]</sup>

* <a name="imports-grouping"></a>Imports should be grouped in the following order with a blank line
between each group of imports: <sup>[[link](#imports-grouping)]</sup>
  1. standard library imports
  1. related third party imports
  1. local application/library specific imports

* <a name="wildcard-imports"></a>Wildcard imports (`from <module> import *`) should be avoided.
<sup>[[link](#wildcard-imports)]</sup>

### String Quotes

* <a name="single-quotes"></a>Use single-quoted strings whenever possible.
<sup>[[link](#single-quotes)]</sup>

* <a name="double-quotes"></a>When a string contains single quote characters, use the double quote
ones to avoid backslashes in the string. It improves readability.<sup>[[link](#double-quotes)]</sup>

* <a name="triple-quotes"></a>For triple-quoted strings, always use double quote characters to be
consistent with the docstring convention in PEP 257.<sup>[[link](#triple-quotes)]</sup>

## Commenting

> Though a pain to write, comments are absolutely vital to keeping our code
> readable. The following rules describe what you should comment and where. But
> remember: while comments are very important, the best code is
> self-documenting. Giving sensible names to types and variables is much better
> than using obscure names that you must then explain through comments.
>
> When writing your comments, write for your audience: the next contributor who
> will need to understand your code. Be generous — the next one may be you!

&mdash;[Google C++ Style Guide][google-c++]

* <a name="up-to-date-comments"></a>Comments that contradict the code are worse than no comments.
Always make a priority of keeping the comments up-to-date when the code changes!
<sup>[[link](#up-to-date-comments)]</sup>

* <a name="complete-sentence-comments"></a>Comments should be complete sentences. The first word
should be capitalized, unless it is an identifier that begins with a lower case letter (never alter
the case of identifiers!).<sup>[[link](#complete-sentence-comments)]</sup>

### Inline Comments

An inline comment is a comment on the same line as a statement. Inline comments should be separated
by at least two spaces from the statement. They should start with a # and a single space.

* <a name="inline-comments"></a>Use inline comments sparingly.<sup>[[link](#inline-comments)]</sup>

* <a name="unnecessary-inline-comments"></a>Inline comments are unnecessary and in fact distracting
if they state the obvious.<sup>[[link](#unnecessary-inline-comments)]</sup>
    ```python
    # Don't do this:
    x = x + 1                 # Increment x

    # But sometimes, this is useful:
    x = x + 1                 # Compensate for border
    ```

### Documentation Strings

* <a name="public-docstrings"></a>Write docstrings for all public modules, functions, classes, and
methods.<sup>[[link](#public-docstrings)]</sup>

* <a name="private-docstrings"></a>Docstrings are not necessary for non-public methods, but you
should have a comment that describes what the method does. This comment should appear after the
`def` line.<sup>[[link](#private-docstrings)]</sup>

* <a name="docstrings-conventions"></a>Conventions for writing good documentation strings (a.k.a.
"docstrings") are immortalized in [PEP 257][pep257].<sup>[[link](#docstrings-conventions)]</sup>

### TODO comments

* <a name="temporary-todo"></a>Use TODO comments for code that is temporary, a short-term solution,
or good-enough but not perfect.<sup>[[link](#temporary-todo)]</sup>

* <a name="todo-convention"></a>TODOs should include the string TODO in all caps, and optionally
followed by the full name of the person who can best provide context about the problem referenced
by the TODO, in parentheses.<sup>[[link](#todo-convention)]</sup>

* <a name="todo-optional-column"></a>A colon is optional.<sup>[[link](#todo-optional-column)]</sup>

* <a name="todo-comment"></a>A comment explaining what there is to do is required. The main purpose
is to have a consistent TODO format that can be searched to find the person who can provide more
details upon request.<sup>[[link](#todo-comment)]</sup>

* <a name="todo-mention"></a>A TODO is not a commitment that the person referenced will fix the
problem. Thus when you create a TODO, it is almost always your name that is given.
<sup>[[link](#todo-mention)]</sup>

    ```python
      # No
      # TODO: check.

      # Bad
      # TODO(RS): Use proper namespacing for this constant.

      # Bad
      # TODO(drumm3rz4lyfe): Use proper namespacing for this constant.

      # Ok, but rather mention the full name
      # TODO: Use proper namespacing for this constant.

      # Good
      # TODO(Ringo Starr): Use proper namespacing for this constant.
    ```

### TODO vs FIXME

* <a name="todo-usage"></a>Use `# FIXME:` to annotate problems.<sup>[[link](#todo-usage)]</sup>
    ```python
    def truncate(sentence):
        # FIXME: shouldn't use a global here.
        return sentence[:CUT]
    ```

* <a name="fixme-usage"></a>Use `# TODO:` to annotate solutions to problems.
<sup>[[link](#fixme-usage)]</sup>
    ```python
    def truncate(sentence):
        # TODO: replace CUT with a function param.
        return sentence[:CUT]
    ```

### Commented-out code

* <a name="commented-out-code"></a>Never leave commented-out code in our codebase.
<sup>[[link](#commented-out-code)]</sup>

## Naming Conventions

* <a name="single-character-variables"></a>Never use the characters 'l' (lowercase letter el), 'O'
(uppercase letter oh), or 'I' (uppercase letter eye) as single character variable names.
<sup>[[link](#single-character-variables)]</sup>

* <a name="naming-files"></a>Modules (filenames) should have short, all-lowercase names. Underscores
can be used in the module name if it improves readability.<sup>[[link](#naming-files)]</sup>

* <a name="naming-directories"></a>Python packages (directories) should also have short,
all-lowercase names, although the use of underscores is discouraged.
<sup>[[link](#naming-directories)]</sup>

* <a name="naming-classes"></a>Class names should normally use the CapWords convention.
<sup>[[link](#naming-classes)]</sup>
  * The naming convention for functions may be used instead in cases where the interface is
  documented and used primarily as a callable.

* <a name="naming-exceptions"></a>Because exceptions should be classes, the class naming convention
applies here. However, you should use the suffix "Error" on your exception names (if the exception
actually is an error).<sup>[[link](#naming-exceptions)]</sup>

* <a name="naming-functions"></a>Function names should be lowercase, with words separated by
underscores as necessary to improve readability.<sup>[[link](#naming-functions)]</sup>

* <a name="naming-instance-method-arguments"></a>Always use `self` for the first argument to
instance methods.<sup>[[link](#naming-instance-method-arguments)]</sup>

* <a name="naming-class-method-arguments"></a>Always use `cls` for the first argument to class
methods.<sup>[[link](#naming-class-method-arguments)]</sup>

* <a name="naming-clashes"></a>If a function argument's name clashes with a reserved keyword, it is
generally better to append a single trailing underscore rather than use an abbreviation or spelling
corruption. Thus `class_` is better than `clss`. (Perhaps better is to avoid such clashes by using a
synonym.) <sup>[[link](#naming-clashes)]</sup>

* <a name="naming-non-public-methods"></a>Use one leading underscore only for non-public methods and
instance variables.<sup>[[link](#naming-non-public-methods)]</sup>

* <a name="naming-constants"></a>Constants are usually defined on a module level and written in all
capital letters with underscores separating words. Examples include MAX_OVERFLOW and TOTAL.
<sup>[[link](#naming-constants)]</sup>

* <a name="naming-public-attributes"></a>Public attributes should have no leading underscores.
<sup>[[link](#naming-public-attributes)]</sup>

* <a name="shortening-names"></a>Do not shorten words or smash them together without a separating
underscore.<sup>[[link](#shortening-names)]</sup>

* <a name="naming-functions"></a>It is preferred to name functions with a verb. (Even if it means
putting `get_` or `find_` in front of the function name) <sup>[[link](#naming-functions)]</sup>

* <a name="long-variable-names"></a>Use long variable names, whole words and multiple words.
<sup>[[link](#long-variable-names)]</sup>

## Strings

* <a name="format-strings"></a>Use .format() in Python 2 and string literals in Python 3.
<sup>[[link](#format-strings)]</sup>
    ```python
    currency = 'USD'
    rank = 2
    rate = 0.3510

    # No
    message = 'Currency: %s, rank: %d, rate: %.2f' % (currency, rank, rate)

    # Ok
    message = 'Currency: {}, rank: {}, rate: {:.2f}'.format(currency, rank, rate)

    # Good in 2.x (Too verbose but allows easier migration to Python 3.6+)
    message = 'Currency: {currency}, rank: {rank}, rate: {rate:.2f}'.format(
        currency=currency,
        rank=rank,
        rate=rate,
    )

    # Yes in 3.x
    message = f'Currency: {currency}, rank: {rank}, rate: {rate:.2f}'
    ```

* <a name="join-strings"></a>Join a list of values together using the ``join`` method.
<sup>[[link](#join-strings)]</sup>
    ```python
    animals = ['cat', 'dog', 'mouse']
    output = ', '.join(animals)
    ```

## Regular Expressions

* <a name="avoid-regular-expressions"></a>Avoid using regular expressions if there's a simpler and
equally accurate way of expressing your target search/transformation.
<sup>[[link](#avoid-regular-expressions)]</sup>
* <a name="verbose-regular-expressions"></a>Unless your regular expression is extremely simple,
always use a multi-line string and ``VERBOSE`` mode when representing your regular expression.
<sup>[[link](#verbose-regular-expressions)]</sup>

## Conditionals

* <a name="comparing-singletons"></a>Comparisons to singletons like `None` should always be done
with is or is not, never the equality operators.<sup>[[link](#comparing-singletons)]</sup>

### Truthiness

* <a name="check-emptiness"></a>Do not check emptiness (strings, lists, tuples) through length or
other means. Use the fact that empty sequences are false.<sup>[[link](#check-emptiness)]</sup>

    ```python
    # No
    if len(results) == 0:
        print('No results found.')

    if len(failures) > 0:
        print('There were failures during processing.')

    # Yes
    if not results:
        print('No results found.')

    if failures:
        print('There were failures during processing.')
    ```

* <a name="check-zeroness"></a>Do not rely on truthiness for checking zeroness or non-zeroness
though.<sup>[[link](#check-zeroness)]</sup>
    ```python
    # No:
    if n % 2:
        print('The given number is odd')

    if not step_count:
        print('No steps taken.')
    ```

    ```python
    # Yes:
    if n % 2 == 1:
        print('The given number is odd')

    if step_count == 0:
        print('No steps taken.')
    ```

* <a name="compare-booleans"></a>Don't compare boolean values to True or False using `==`.
<sup>[[link](#compare-booleans)]</sup>
    ```python
    # Yes
    if greeting:

    # No
    if greeting == True:

    # Worse
    if greeting is True:
    ```

### Long if-elif chains

* <a name="if-else-chains"></a>Python does not have switch statements.  Instead, you'll often see
Python developers use an `if` statement with many `elif` statements. Instead of using many `elif`
statements, consider using a dictionary. This alternative is often (but not always) possible.
<sup>[[link](#if-else-chains)]</sup>
    ```python
    # No
    if word == 'zero':
        numbers.append(0)
    elif word == 'one':
        numbers.append(1)
    elif word == 'two':
        numbers.append(2)
    elif word == 'three':
        numbers.append(3)
    elif word == 'four':
        numbers.append(4)
    elif word == 'five':
        numbers.append(5)
    elif word == 'six':
        numbers.append(6)
    elif word == 'seven':
        numbers.append(7)
    elif word == 'eight':
        numbers.append(8)
    elif word == 'nine':
        numbers.append(9)
    else:
        numbers.append(' ')

    # Yes
    word_to_digit = {
        'zero': 0,
        'one': 1,
        'two': 2,
        'three': 3,
        'four': 4,
        'five': 5,
        'six': 6,
        'seven': 7,
        'eight': 8,
        'nine': 9,
    }
    numbers.append(word_to_digit.get(word, ' '))
    ```

### Conversion to bool

* If you ever see code that sets a variable to `True` or `False` based on a condition. Rely on
truthiness by converting the condition to a `bool` instead, either explicitly for the truthy case or
implicitly using `not` for the falsey case.

    ```python
    # No
    if results:
        found_results = True
    else:
        found_results = False

    if not failures:
        success = True
    else:
        success = False

    # Yes
    found_results = bool(results)

    success = not failures
    ```

* Keep in mind that sometimes no conversion is necessary. The condition here is already a boolean
value. So type-casting to a ``bool`` would be redundant.  Instead simply set the variable equal to
the expression.

    ```python
    # No
    if n % 2 == 1:
        is_odd = True
    else:
        is_odd = False

    # Yes
    is_odd = (n % 2 == 1)
    ```

## Recommendations

### Compacting Assignments

* It is fine to use iterable unpacking to compact multiple assignment statements onto one line. Do
this only when the assignments are very tightly related.

    ```python
    word1, word2 = word1.upper(), word2.upper()
    x, y, z = (a1 - a2), (b1 - b2), (c1 - c2)
    ```

### Naming Indexes

* Whenever you see something like `some_variable[0]` or `some_variable[2]`, treat this as an
indication that you should be relying on iterable unpacking.
    ```python
    # No
    do_something(things[0], things[1])

    do_something(things[0], things[1:-1], things[-1])

    # Yes
    first, second = things
    do_something(first, second)

    head, *middle, tail = things
    do_something(head, middle, tail)
    ```

### Named Tuples

* Whenever you see yourself using tuples and indexes to access them, define a namedtuple.
    ```python
    # No

    employee = ('Guido', 'Berlin', 61)
    if employee[2] > 65:
        print(f'Sorry {employee[1]}, it is time to rest')

    # Yes
    from collections import namedtuple

    Employee = namedtuple('Employee', 'name city age')
    employee = Employee(name='Guido', city='Berlin', age=61)
    if employee.age > 65:
        print(f'Keep going {employee.name}!')
    ```

### Exceptions

* <a name="specific-exceptions"></a>When catching exceptions, mention specific exceptions whenever
possible instead of using a bare `except:` clause.<sup>[[link](#specific-exceptions)]</sup>

* <a name="limit-try-clauses"></a>For all try/except clauses, limit the try clause to the absolute
minimum amount of code necessary. Again, this avoids masking bugs.
<sup>[[link](#limit-try-clauses)]</sup>
    ```python
    # Yes

    try:
        value = collection[key]
    except KeyError:
        return key_not_found(key)
    else:
        return handle_value(value)

    # No

    try:
        # Too broad!
        return handle_value(collection[key])
    except KeyError:
        # Will also catch KeyError raised by handle_value()
        return key_not_found(key)

    ```
* <a name="custom-exceptions"></a>Define your custom exceptions and explicitly catch them.
<sup>[[link](#custom-exceptions)]</sup>

### Return statements

* Be consistent in return statements. Either all return statements in a function should return an
expression, or none of them should. If any return statement returns an expression, any return
statements where no value is returned should explicitly state this as return None, and an explicit
return statement should be present at the end of the function (if reachable).
    ```python
    # No

    def foo(x):
        if x >= 0:
            return math.sqrt(x)

    def bar(x):
        if x < 0:
            return
        return math.sqrt(x)

    # Yes

    def foo(x):
        if x >= 0:
            return math.sqrt(x)
        else:
            return None

    def bar(x):
        if x < 0:
            return None
        return math.sqrt(x)
    ```

### Loops

* <a name="no-range-len"></a>If you ever see `range(len(colors))`, consider whether you actually
need an index. Never do this.<sup>[[link](#no-range-len)]</sup>

    ```python
    # No
    for i in range(len(colors)):
        print(colors[i])
    ```

* <a name="use-zip"></a>Use `zip` instead of an index to loop over multiple lists at the same time.
<sup>[[link](#use-zip)]</sup>

    ```python
    # Yes
    for color, ratio in zip(colors, ratios):
        print('{}% {}'.format(ratio * 100, color))
    ```

* <a name="enumerate"></a>If you do really need an index, use `enumerate`.
<sup>[[link](#enumerate)]</sup>
    ```python
    # Yes
    for number, name in enumerate(presidents, start=1):
        print('President {}: {}'.format(number, name))
    ```

## Be Consistent

> If you're editing code, take a few minutes to look at the code around you and
> determine its style. If they use spaces around all their arithmetic
> operators, you should too. If their comments have little boxes of hash marks
> around them, make your comments have little boxes of hash marks around them
> too.
>
> The point of having style guidelines is to have a common vocabulary of coding
> so people can concentrate on what you're saying rather than on how you're
> saying it. We present global style rules here so people know the vocabulary,
> but local style is also important. If code you add to a file looks
> drastically different from the existing code around it, it throws readers out
> of their rhythm when they go to read it. Avoid this.

&mdash;[Google C++ Style Guide][google-c++]

## A Foolish Consistency is the Hobgoblin of Little Minds

> One of Guido's key insights is that code is read much more often than it is written. The
> guidelines provided here are intended to improve the readability of code and make it consistent
> across the wide spectrum of Python code. As PEP 20 says, "Readability counts".
>
> A style guide is about consistency. Consistency with this style guide is important. Consistency
> within a project is more important. Consistency within one module or function is the most
> important.
>
> However, know when to be inconsistent -- sometimes style guide recommendations just aren't
> applicable. When in doubt, use your best judgment. Look at other examples and decide what looks
> best. And don't hesitate to ask!

&mdash;[PEP 8 -- Style Guide for Python Code][pep8]

In particular:

* Do not break backwards compatibility just to comply with this guide!
* Do not pep8-ify code for the sake of pep8-ify-ing it. This is meta-work and often brakes
`git blame` usage.

[airbnb-ruby]: https://github.com/airbnb/ruby/blob
[default-indentation]: https://www.python.org/dev/peps/pep-0008/#indentation
[google-c++-comments]: https://google.github.io/styleguide/cppguide.html#Comments
[google-c++]: https://google.github.io/styleguide/cppguide.html
[google-python-comments]: https://google.github.io/styleguide/pyguide.html#Comments
[gyg-php]: https://github.com/getyourguide/gyg-style-guides/blob/master/PHP.md
[trey-hunner]: https://github.com/treyhunner
[trey-hunner-python-guide]: https://github.com/TruthfulTechnology/style-guide
[pep8-whitespace]: https://www.python.org/dev/peps/pep-0008/#whitespace-in-expressions-and-statements
[pep8]: https://www.python.org/dev/peps/pep-0008
[pep257]: https://www.python.org/dev/peps/pep-0257
