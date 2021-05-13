---
title: Asking good questions
---

# Asking Good Questions

This document is intended to provide you with the information you need to get help as quickly and effectively as possible. If you're stuck on a problem or don't understand something, you should always feel welcome to ask for help.

## Before You Ask

Before you ask your question, there are a few things you can do to find an answer on your own:

- Read the official documentation for whatever you're working with
- Examine the traceback when your code raises an exception
- Do some research online - for example, on Stack Overflow

!!! info
    Essentially, doing your research is the first step towards a solution to any problem.

If none of the above steps helps you, or you're not sure how to do some of the above steps, feel free to ask any of the mentors for help.

## A Good Question

When you're ready to ask a question, there are a few things that will help you formulate great quality questions:

- A code example that illustrates your problem (you can copy and paste or save it as a gist in GitHub and share the URL)
- Details on how you attempted to solve the problem on your own
- Full version information — for example, `Python 3.6.4 with NumPy 1.21.`
- The full traceback if your code raises an exception
- Do not curate the traceback as you may inadvertently exclude information crucial to solving your issue
- A screenshot of the output or rendered output

!!! tip
    Your question should be informative but to the point. More importantly, how you phrase your question and how you address those that may help you is crucial. Courtesy never hurts.

## What Not To Ask

> Can I ask a question?

Yes. Always yes. Just ask it.

> Is anyone here good at Flask / VSCode?

There are two problems with this question:

This kind of question does not manage to pique anyone's interest, so you're less likely to get an answer overall. On the other hand, a question like "Is it possible to get VSCode to compile SCSS into CSS files automatically" is much more likely to be interesting to someone. Sometimes, the best answers come from someone who does not already know the answer but finds the question interesting enough to search for the answer on your behalf.
When you qualify your question by first asking if someone is good at something, you are filtering out potential answerers. Not only are people bad at judging their skill at something, but the truth is that even someone who has zero experience with the framework you're having trouble with might still be of excellent help to you.
So instead of asking if someone is good at something, ask your question right away.

> My code doesn't work

This isn't a question, and it provides no context or information. Depending on the moods of the people that are around, you may even find yourself ignored. Don't be offended by this - try again with a better question.

## Examining Tracebacks

Usually, the first sign of trouble is that it raises an exception when you run your code. For beginning programmers, the traceback generated for the exception may feel overwhelming and discouraging at first. However, in time, most developers start to appreciate the extensive information in the traceback as it helps them track down the error in their code. So, don't panic and take a moment to review the information provided to you carefully.

### Reading the traceback

```python
Traceback (most recent call last):
  File "my_python_file.py", line 6, in <module>
    spam = division(a=10, b=0)
  File "my_python_file.py", line 2, in division
    answer = a / b
ZeroDivisionError: division by zero
```

In general, the best strategy is to read the traceback from bottom to top. As you can see in the example above, the last line of the traceback contains the actual exception raised by your code. In this case, `ZeroDivisionError: division by zero` indicates the problem: We're trying to divide by zero somewhere in our code, which can't be right. However, while we now know which exception was raised, we still need to trace the exception to the error in our code.

To do so, we turn to the lines above the exception. Reading from bottom to top again, we first encounter the line where the exception was raised: `answer = a / b`. Directly above it, we can see that this line of code was `line 2 `of the file `my_python_file.py` and that it's in the scope of the function division. At this point, it's a good idea to inspect the code referenced here to see if we can spot a mistake:

```python
def division(a, b):
    answer = a / b
    return answer
```

Continue reading the traceback from bottom to top. The next thing we encounter is `spam = division(a=10, b=0)` from `line 6` of the `file my_python_file.py`. In this case, `<module>` tells us that the code is in the global scope of that file. While it's already clear from the traceback what's going wrong here, we're passing `b=0` to the function division, inspecting the code shows us the same:

```python linenums="5"
spam = division(a=10, b=0)
print(spam)
```

We have now traced the exception to a line of code calling the division function with a divisor of 0.
This is a simplified example, but the same steps apply to more complex situations as well.

### The error is sometimes in the line **before** the line in the traceback

Sometimes, the actual error is in the line right before the one referenced in the traceback. This usually happens when we've inadvertently omitted a character meant to close an expression, like a brace, bracket, or parenthesis. For instance, the following snippet of code will generate a traceback pointing at the line after the one in which we've missed the closing parenthesis:

```python
print("Hello, world!"
print("This is my first Python program!")
File "my_python_file.py", line 2
    print("This is my first Python program!")
        ^
SyntaxError: invalid syntax
```

This may happen because Python allows for implicit line continuation and will only notice the error when the expression does not continue as expected on the following line. So, it's always a good idea to check the line before the one mentioned in the traceback!

### More resources on exceptions

- Real Python has a [great article on tracebacks and errors](https://realpython.com/python-traceback/)

Further information on exceptions can be found in the official Python documentation:

- [The built-in exceptions page](https://docs.python.org/3/library/exceptions.html) lists all the built-in exceptions along with a short description of the exception. If you're unsure of the meaning of an exception in your traceback, this is an excellent place to start.
- [The errors and exceptions chapter in the official tutorial](https://docs.python.org/3/tutorial/errors.html) gives an overview of errors and exceptions in Python. Besides explaining what exceptions are, it also describes how to handle expected exceptions graciously to keep your application from crashing when an expected exception is raised and how to define custom exceptions specific to your application.

!!! tip
    If you encounter an exception specific to an external module or package, it's usually a good idea to check the documentation of that package to see if the exception is documented. Another option is to paste a part of the traceback, usually the last line, into your favourite search engine to see if anyone else has encountered a similar problem. More often than not, you will be able to find a solution to your problem this way.
