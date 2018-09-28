<style>
.jab-post img {
    border: 2px solid #eeeeee;
    padding: 5px;
}
</style>


One question we often hear from people starting out with PythonAnywhere is "how do I turn this
script I've written into a website so that other people can run it?"

That's actually a bigger topic than you might imagine, and a complete answer would wind up
having to explain almost everything about web development.   So we won't do all of that in this
blog post :-)   The good news is that simple scripts can often be turned into simple websites
pretty easily, and in this blog post we'll work through a couple of examples.

Let's get started!


## The simplest case: a script that takes some inputs and returns an output

Let's say you have this Python 3.x script:

    number1 = float(input("Enter the first number: "))
    number2 = float(input("Enter the second number: "))
    solution = number1 + number2
    print("The sum of your numbers is {}".format(solution))

Obviously that's a super-simple example, but a lot of more complicated scripts
follow the same kind of form.   For example, a script for a financial analyst might
have these equivalent steps:

* Get data about a particular stock from the user.
* Run some kind of complicated analysis on the data.
* Print out a result saying how good the algorithm thinks the stock is as an investment.

The point is, we have three phases, input, processing and output.

*(Some scripts have more phases
-- they gather some data, do some processing, gather some more data, do more processing, and so on, and
eventually print out a result.  We'll come on to those later on.)*

Let's work through how we would
change our three-phase input-process-output script into a website.


### Step 1: extract the processing into a function

In a website's code, we don't have access to the Python `input` or `print` functions,
so the input and output phases will be different -- but
the processing phase will be the same as it was in the original script.
So the first step is to extract our processing code into a function so that it can be
re-used.   For our example, that leaves us with something like this:

    def do_calculation(number1, number2):
        return number1 + number2

    number1 = float(input("Enter the first number: "))
    number2 = float(input("Enter the second number: "))
    solution = do_calculation(number1, number2)
    print("The sum of your numbers is {}".format(solution))

Simple enough.  In real-world cases like the stock-analysis then of course there would
be more inputs, and the `do_calculation` function would be considerably more complicated, but
the theory is the same.


### Step 2: create a website

The easiest web framework to get started with when creating this kind of thing is
[Flask](http://flask.pocoo.org/); it's very simple and doesn't have a lot of the built-in
stuff that other web frameworks have, but for our purposes that's a good thing.

Assuming that you already have a PythonAnywhere account, log in and go to the "Web" page:

<img width="500" src="/static/images/flask-tutorial-web-tab-empty.png">

Click on the "Add a new web app" button to the left.  This will pop up a "Wizard" which allows you to configure your site.  If you have a free account, it will look like this:

<img width="500" src="/static/images/flask-tutorial-create-web-app-subdomain.png">

If you decided to go for a paid account (thanks :-), then it will be a bit different:

<img width="500" src="/static/images/flask-tutorial-create-web-app-custom-domain.png">

What we're doing on this page is specifying the host name in the URL that people will enter to see your website.  Free accounts can have one website, and it must be at *yourusername*`.pythonanywhere.com`.  Paid accounts have the option of using their own custom host names in their URLs.

For now, we'll stick to the free option.  If you have a free account, just click the "Next" button, and if you have a paid one, click the checkbox next to the *yourusername*`.pythonanywhere.com`, then click "Next".  This will take you on to the next page in the wizard.

<img width="500" src="/static/images/flask-tutorial-create-web-app-choose-framework.png">

This page is where we select the web framework we want to use.  We're using Flask, so click that one to go on to the next page.

<img width="500" src="/static/images/flask-tutorial-create-web-app-choose-python-version.png">

PythonAnywhere has various versions of Python installed, and each version has its associated version of Flask.  You can use different Flask versions to the ones we supply by default, but it's a little more tricky (you need to use a thing called a *virtualenv*), so for this tutorial we'll create a site using Python 3.6, with the default Flask version.  Click the option, and you'll be taken to the next page:

<img width="500" src="/static/images/flask-tutorial-create-web-app-choose-location.png">

This page is asking you where you want to put your code.  Code on PythonAnywhere is stored in your home directory, `/home/`*yourusername*, and in its subdirectories.  Now, Flask is a particularly lighweight framework, and you can write a simple Flask app in a single file.  PythonAnywhere is asking you where it should create a directory and put a single file with a really really simple website.  The default should be fine; it will create a subdirectory of your home directory called `mysite` and then will put the Flask code into a file called `flask_app.py` inside that directory.

*(It will overwrite any other file with the same name, so if you're _not_ using a new PythonAnywhere account, make sure that the file that it's got in the "Path" input box isn't one of your existing files.)*

Once you're sure you're OK with the filename, click "Next".  There will be a brief pause while PythonAnywhere sets up the website, and then you'll be taken to the configuration page for the site:

<img width="500" src="/static/images/flask-tutorial-web-app-page-initial-site.png">

You can see that the host name for the site is on the left-hand side, along with the "Add a new web app" button.  If you had multiple websites in your PythonAnywhere account, they would appear there too. But the one that's currently selected is the one you just created, and if you scroll down a bit you can see all of its settings.  We'll ignore most of these for the moment, but one that is worth noting is the "Best before date" section.

If you have a paid account, you won't see that -- it only applies to free accounts.  But if you have a free account, you'll see something saying that your site will be disabled on a date in three months' time.  Don't worry!  You can keep a free site up and running on PythonAnywhere for as long as you want, without having to pay us a penny.  But we do ask you to log in every now and then and click the "Run until 3 months from today" button, just so that we know you're still interested in keeping it running.

Before we do any coding, let's check out the site that PythonAnywhere has generated for us by default.  Right-click the host name, just after the words "Configuration for", and select the "Open in new tab" option; this will (of course) open your site in a new tab, which is useful when you're developing -- you can keep the site open in one tab and the code and other stuff in another, so it's easier to check out the effects of the changes you make.

Here's what it should look like.

<img width="500" src="/static/images/flask-tutorial-initial-site-content.png">

OK, it's pretty simple, but it's a start.  Let's take a look at the code!  Go back to the tab showing the website configuration (keeping the one showing your site open), and click on the "Go to directory" link next to the "Source code" bit in the "Code" section:

<img width="500" src="/static/images/flask-tutorial-source-code-link.png">

You'll be taken to a different page, showing the contents of the subdirectory of your home directory where your website's code lives:

<img width="500" src="/static/images/flask-tutorial-initial-source-dir.png">

Click on the `flask_app.py` file, and you'll see the (really really simple) code that defines your Flask app.  It looks like this:

<img width="500" src="/static/images/flask-tutorial-initial-source-code.png">

It's worth working through this line-by-line:

    from flask import Flask

As you'd expect, this loads the Flask framework so that you can use it.

    app = Flask(__name__)

This creates a Flask application to run your code.

    @app.route('/')

This decorator specifies that the following function defines what happens when someone goes to the location "/" on your site -- eg. if they go to `http://`*yourusername*`.pythonanywhere.com/`.  If you wanted to define what happens when they go to `http://`*yourusername*`.pythonanywhere.com/foo` then you'd use `@app.route('/foo')` instead.

    def hello_world():
        return 'Hello from Flask!'

This simple function just says that when someone goes to the location, they get back the (unformatted) text "Hello from Flask".

Try changing it -- for example, to "This is my new shiny Flask app".  Once you've made the change, click the "Save" button at the top to save the file to PythonAnywhere:

<img src="/static/images/flask-tutorial-save-button.png">

...then the reload button (to the far right, looking like two curved arrows making a circle), which stops your website and then starts it again with the fresh code.

<img src="/static/images/flask-tutorial-reld-button.png">

A "spinner" will appear next to the button to tell you that PythonAnywhere is working.  Once it has disappeared, go to the tab showing the website again, hit the page refresh button, and you'll see that it has changed as you'd expect.

<img width="500" src="/static/images/flask-tutorial-first-site-modification.png">


### Step 3: make the processing code available to the web app

Now, we want our Flask app to be able to run our code.   We've already extracted it into a
function of its own.   It's generally a good idea to keep the web app code -- the basic stuff to
display pages -- from the more complicated processing code (after all, if we were doing the stock analysis
example rather than this simple add-two-numbers script, the processing could be thousands of lines long).

So, we'll create a new file for our processing code.   Go back to the browser tab that's showing your editor page; you'll see
"breadcrumb" links showing you where the file is stored.  They'll be a series of directory names separated
by "/" characters, each one apart from the last being a link.
The last one, just before the name of the file containing your Flask code,
will probably be `mysite`.   Right-click on that, and open it in a new browser tab -- the new tab
will show the directory listing you had before:

<img width="500" src="/static/images/flask-tutorial-initial-source-dir.png">

In the input near the top right, where it says "Enter new file name, eg. hello.py", enter the name
of the file that will contain the processing code.  Let's (uninventively) call it `processing.py`.
Click the "New file" button, and you'll have another editor window open, showing an empty file.
Copy/paste your processing function into there; that means that the file should simply contain this
code:

    def do_calculation(number1, number2):
        return number1 + number2

Save that file, then go back to the tab you kept open that contains the Flask code.   At the
top, add a new line just after the line that imports Flask, to import your processing code:

    from processing import do_calculation

Save the file; you'll see that you get a warning icon next to the new line.  If you move your
mouse pointer over the icon, you'll see the details:

<img width="500" src="/static/images/script-to-webapp-import-warning.png">

It says that the function was imported but is not being used, which is completely true!  That
moves us on to the next step.

### Step 4: Accepting input

What we want our site to do is display a page that allows the user to enter two numbers.   To do
that, we'll change the existing function that is run to display the page.  Right now we have this:

    @app.route('/')
    def hello_world():
        return 'This is my new shiny Flask app'

We want to display more than text, we want to display some HTML.   Now, the *best* way to do HTML in
Flask apps is to use templates (which allow you to keep the Python code that Flask needs in separate files from
the HTML), but we have [other tutorials](/121/) that go into the details of that.  In this case
we'll just put the HTML right there inside our Flask code:

    @app.route('/')
    def hello_world():
        return '''
            <html>
                <body>
                    <p>Enter your numbers:
                    <form>
                        <p><input name="number1" /></p>
                        <p><input name="number2" /></p>
                        <p><input type="submit" value="Do calculation" /></p>
                    </form>
                </body>
            </html>
        '''

We won't go into the details of how HTML works here, there are lots of excellent tutorials online
and one that suits the way you learn is just a Google search away.   For now, all we need to know is that
where we were previously returning a single-line string, we're now returning a multi-line one
(that's what the three quotes in a line mean, in case you're not familiar with them -- one string split
over multiple lines).   The multi-line string contains HTML code, which just displays a page
that asks
the user to enter two numbers, and a button that says "Do calculation".   Click on the editor's
"reload website" button:

<img src="/static/images/flask-tutorial-reld-button.png">

...and then check out your website again in the tab that you (hopefully) kept open, and you'll
see something like this:

<img src="/static/images/script-to-webapp-initial-page.png">

However, as we haven't done anything to wire up the input to the processing, clicking the "Do calculation"
button won't do anything but reload the page.


### Step 5: validating input

We could at this stage go straight to adding on the code to do the calculations, and I was
originally planning to do that here.   But after thinking about it, I realised that doing that
would basically be teaching you to shoot yourself in the foot...  When you put a website up
on the Internet, you have to allow for the fact that the people using it will make mistakes.
If you created a site that allowed people to enter numbers and add them, sooner or later someone
will type in "wombat" for one of the numbers, or something like that, and it would be embarrassing
if your site responded with an internal server error.

So let's add on some basic validation -- that is, some code that makes sure that people aren't
providing us with random marsupials instead of numbers.

A good website will, when you enter an invalid input, display the page again with an error message
in it.   A bad website will display a page saying "Invalid input, please click the back button
and try again".   Let's write a good website.

The first step is to change our HTML so that the person viewing the page can click the "Do calculation"
button and get a response.   Just change the line that says

                    <form>

So that it says this:

                    <form method="post" action=".">

What that means is that previously we had a form, but now we have a form that has an "action"
telling it that when the button that has the type "submit" is clicked, it should request the same
page as it is already on, but this time it should use the "post" method.

*(HTTP methods are extra bits of information that are tacked on to requests that are made
by a browser to a server.  The "get" method, as you might expect, means "I just want to get a
page".  The "post" method means "I want to provide the server with some information to store or
process".  There are vast reams of details that I'm skipping over here, but that's the most
important stuff for now.)*

So now we have a way for data to be sent back to the server.  Reload the site using the button
in the editor, and refresh the page in the tab where you're viewing your site.   Try entering some
numbers, and click the "Do calculation" button, and you'll get... an incomprehensible error message:

<img width="500" src="/static/images/flask-tutorial-method-not-allowed.png">

Well, perhaps not entirely incomprehensible.  It says "method not allowed".  Previously we were
using the "get" method to get our page, but we told the form that it should use the post message
when the data was submitted.   So Flask is telling us that it's not going to allow that page to
be requested with the "post" method.

By default, Flask view functions only accept requests using the "get" method.  It's easy to change that.
Back in the code file, where we have this line:

    @app.route('/')

...replace it with this:

    @app.route("/", methods=["GET", "POST"])

Save the file, hit the reload button in the editor, then go to the tab showing your page; click back
to get away from the error page if it's still showing, then enter some numbers and click the
"Do calculation" button again.

You'll be taken back to the page with no error.   Success!  Kind of.

Now let's add the validation code.  The numbers that were entered will be made available to us
in our Flask code via the `form` attribute of a global variable called `request`.  So we can add
validation logic by using that.  The first step is to make the `request` variable available by
importing it; change the line that says

    from flask import Flask

to say

    from flask import Flask, request

Now, add this code to the view function, before the `return` statement:

        errors = ""
        if request.method == "POST":
            number1 = None
            number2 = None
            try:
                number1 = float(request.form["number1"])
            except:
                errors += "<p>{!r} is not a number.</p>\n".format(request.form["number1"])
            try:
                number2 = float(request.form["number2"])
            except:
                errors += "<p>{!r} is not a number.</p>\n".format(request.form["number2"])

Basically, we're saying that if the method is "post", we do the validation.

Finally, add some code to put those errors into the page's HTML; replace the bit that
returns the multi-line string with this:

        return '''
            <html>
                <body>
                    {errors}
                    <p>Enter your numbers:
                    <form>
                        <p><input name="number1" /></p>
                        <p><input name="number2" /></p>
                        <p><input type="submit" value="Do calculation" /></p>
                    </form>
                </body>
            </html>
        '''.format(errors=errors)

This is exactly the same page as before, we're just interpolating the string that contains
any errors into it just above the "Enter your numbers" header.

Save the file; you'll see more warnings for the lines where we define
variables called `number1` and `number2`, because we're not using those variables.  We know we're going
to fix that, so they can be ignored for now.

Reload the site, and head over to the
page where we're viewing it, and try to add a koala to a wallaby -- you'll get an
appropriate error:

<img src="/static/images/script-to-webapp-marsupial-not-allowed-error.png">

Try adding 23 to 19, however, and you won't get 42 -- you'll just
get the same input form again.  So now, the final step that brings it all together.


### Step 6: doing the calculation!

We're all set up to do the calculation.  What we want to do is:

* If the request used a "get" method, just display the input form
* If the request used a "post" method, but one or both of the numbers are not valid, then display the input form with error messages.
* If the request used a "post" method, and both numbers are valid, then display the result.

We can do that
by adding something inside the `if request.method == "POST":` block, just after we've checked
that `number2` is valid:

            if number1 is not None and number2 is not None:
                result = do_calculation(number1, number2)
                return '''
                    <html>
                        <body>
                            <p>The result is {result}</p>
                            <p><a href="/">Click here to calculate again</a>
                        </body>
                    </html>
                '''.format(result=result)

Adding that code should clear out all of the warnings in the editor page, and if you reload your
site and then try using it again, it should all work fine!

<img src="/static/images/script-to-webapp-life-the-universe-and-everything.png">


## Pause for breath...

So if all has gone well, you've now converted a simple script that could add two numbers
into a simple website that lets other people add numbers.   If you're getting error messages,
it's well worth trying to debug them yourself to find out where any typos came in.   An
excellent resource is the website's error log; there's a link on the "Web" page:

<img width="500" src="/static/images/script-to-webapp-error-log-link.png">

...and the most recent error will be at the bottom:

<img width="500" src="/static/images/script-to-webapp-error-log-with-typo.png">

That error message is telling me that I mistyped "flask" as "falsk", and the traceback
tells me exactly which line the typo is on.

However, if you get completely stuck, here's the code you should currently have:

    from flask import Flask, request

    app = Flask(__name__)
    from processing import do_calculation

    @app.route("/", methods=["GET", "POST"])
    def hello_world():
        errors = ""
        if request.method == "POST":
            number1 = None
            number2 = None
            try:
                number1 = float(request.form["number1"])
            except:
                errors += "<p>{!r} is not a number.</p>\n".format(request.form["number1"])
            try:
                number2 = float(request.form["number2"])
            except:
                errors += "<p>{!r} is not a number.</p>\n".format(request.form["number2"])
            if number1 is not None and number2 is not None:
                result = do_calculation(number1, number2)
                return '''
                    <html>
                        <body>
                            <p>The result is {result}</p>
                            <p><a href="/">Click here to calculate again</a>
                        </body>
                    </html>
                '''.format(result=result)

        return '''
            <html>
                <body>
                    {errors}
                    <p>Enter your numbers:
                    <form method="post" action=".">
                        <p><input name="number1" /></p>
                        <p><input name="number2" /></p>
                        <p><input type="submit" value="Do calculation" /></p>
                    </form>
                </body>
            </html>
        '''.format(errors=errors)
