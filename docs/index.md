# Flask Workshop

Welcome to the Flask Workshop! This guide will introduce you to Flask, a powerful web framework for Python, and guide you through building web applications from scratch.

---

## Table of Contents
1. [Introduction To Flask](#1-introduction-to-flask)
2. [Workshop Prerequisites](#2-workshop-prerequisites)
3. [Setting Up Your Environment](#3-setting-up-your-environment)
4. [Getting Started with Flask](#4-getting-started-with-flask) 
5. [Working with Templates](#5-working-with-templates) 
6. [Template Inheritance](#6-template-inheritance)
7. [Static Content (CSS)](#7-static-content-css)
8. [About the Project](#8-about-the-project)
9. [Databases](#9-databases)
10. [Updating the UI](#10-updating-the-ui)
11. [POST and GET methods](#11-post-and-get-methods)
12. [Sending request for POST method](#12-sending-request-for-post-method)
13. [Conclusion](#13-conclusion)
---

## 1. Introduction to Flask

Flask is a lightweight framework for web development in Python, designed to make creating web applications easy and flexible. It’s known for its simplicity and extensive customization options, making it ideal for beginners and experts alike.

### Key Features of Flask
- Minimal setup and easy to learn
- Supports extensions for additional functionality
- Works well with other Python libraries

---

## 2. Workshop Prerequisites

Before the workshop, ensure you have:
- **Python** (version 3.x) installed
- A code editor, like **Visual Studio Code**
- **pip**, the Python package manager (usually included with Python)  
For a step-by-step setup guide, [Click here](assets/pre_req.pdf).  
> If you want to acess some of the useful VS Code shortcuts, that you could use in this workshop, [Click here](https://youtu.be/xvFZjo5PgG0)  
(But try to use them minimally, as there are no shortcuts in life :) )


## 3. Setting Up Your Environment

1. Create a folder for the workshop project.

2. Open the folder using VS Code, and install virtual environment by typing the following code in the VS Code terminal:-  
    `pip3 install virtualenv`

3. Type the following code to create the environment in you folder:-  
    `python -m venv venv`

4. Activate the environment:-  
    - **Windows**:
      `.\venv\Scripts\activate`  

      > ***Note:*** You may get an error called **PowerShell Execution Policy Restriction** or a **PSSecurityException**.  
      It occurs because PowerShell’s execution policy restricts script execution to prevent potentially harmful scripts from running on the system.  
      For that you can run the code `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` and then try activating it again.  
       This temporarily changes PowerShell’s execution policy for the current session only, allowing scripts to run without restrictions.  

    - **macOS/Linux**:
      `source venv/bin/activate`

5. Installing Flask:-  
    Run `pip3 install flask flask-sqlalchemy` on the VS Code terminal.

## 4. Getting Started with Flask  

***Creating Your First Flask App***

1. Create a file named `app.py` in your project folder.

2. Add the following code:

    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def home():
        return "Welcome to the Flask Workshop!"

    if __name__ == '__main__':
        app.run(debug=True)
    ```  
    ***Explanation of Key Parts:***  
    - `app = Flask(__name__)` initializes the app, using the module's name to configure Flask.  
    - `@app.route('/')` sets the route for the root URL, directing requests to the index() function.
    - `app.run(debug=True)` starts the app in debug mode, which automatically reloads the server on code changes and provides detailed error messages.  

3. Run the code:-  
    Run `python3 app.py` on the terminal after saving your app.py file.

4. Opening the Webpage:-  
    Go to your browser and type in `localhost:5000`

5. View the Output

## 5. Working with Templates

1. Create folders `static` and `template` in your project.

2. Add `index.html` in `templates`  

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        Welcome to Flask Workshop 2!
    </body>
    </html>
    ```

3. Modify `app.py`:  

    ```python
    from flask import Flask, render_template

    app = Flask(__name__)

    @app.route('/')
    def index():
        return render_template('index.html')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

## 6. Template Inheritance

Template inheritance in Flask allows you to define a base template with common elements (like headers, footers) and extend it in other templates, promoting code reuse and consistent design.  

1. Add `base.html` in `templates`:  
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        {% block head %}{% endblock %} <!--Jinja code template-->
    </head>
    <body>
        {% block body %}{% endblock %}
    </body>
    </html>
    ```

2. Modify `index.html` in `templates`:  

    ```html
    {% extends 'base.html' %}

    {% block head %}

    {% endblock %}

    {% block body %}
    <h1>Template</h1>
    {% endblock %}
    ```

3. Try searching `localhost:5000` on your browser and view the output.

## 7. Static Content (CSS)

1. In the `static` folder create `css` folder and then create `main.css` file inside `css` folder.  
    ```css
    body{
        margin: 0;
        font-family: sans-serif;
    }
    ```
2. Linking this with `base.html`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}"
        {% block head %}{% endblock %} <!--Jinja code template-->
    </head>
    <body>
        {% block body %}{% endblock %}
    </body>
    </html>
    ```

3. Updating `app.py` (To import url_for)
    ```python
    from flask import Flask, render_template, url_for

    app = Flask(__name__)

    @app.route('/')
    def index():
        return render_template('index.html')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

4. Try searching `localhost:5000` on your browser and view the output. You will file the text font is "sans-serif"

## 8. About the Project

### ACM Task Manager Web Application
We are going to make a Task Manager Web Application where the users are allowed to:  
1. Add tasks with descriptions.
2. Display tasks in a table format, showing the task description and date added.
3. Update or delete tasks using action links.


## 9. Databases

We will be using `SQLAlchemy` for databases

1. Import SQLAlchemy in `app.py` and make some more changes in it.
    ```python
    from flask_sqlalchemy import SQLAlchemy
    ```

2. Make the following changes in `app.py` to add the ToDo class:  
    ```python
    from flask import Flask, render_template, url_for
    from flask_sqlalchemy import SQLAlchemy
    from datetime import datetime

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
    db = SQLAlchemy(app)

    class Todo(db.Model):
        id = db.Column(db.Integer, primary_key = True)
        content = db.Column(db.String(200), nullable=False)
        date_created = db.Column(db.DateTime, default=datetime.utcnow)

        def __repr__(self):
            return '<Task %r>' % self.id

    @app.route('/')
    def index():
        return render_template('index.html')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

3. Setting up the Database
    - Make sure that your virtual environment is activated in the terminal
    - Start an interactive `python3` shell, for that just simple type `python3` in the terminal.
    - This will open a python interpreter which allows you to execute Python code line-by-line. Type the following code in the terminal  
        ```python
        >>> from app import db, app
        >>> with app.app_context():
        ...     db.create_all()
        ```
        This will now create a file called `test.db` in your working directory.  

        > Note: This will either be inside the main folder that you have created, or inside a folder called `instance`.

## 10. Updating the UI

1. Making changes in `index.html`  (Yes!!, just copy paste)
    ```html
    {% extends 'base.html' %}
    
    {% block head %}
    
    {% endblock %}
    
    {% block body %}
    <div class="content">
        <h1>ACM Task Manager</h1>
    
        <table>
            <tr>
                <th>Task</th>
                <th>Added</th>
                <th>Actions</th>
            </tr>
            <tr>
                <td></td>
                <td></td>
                <td>
                    <a href="">Delete</a>
                    <br>
                    <a href="">Update</a>
                </td>
            </tr>
        </table>
    </div>
    {% endblock %}
    ```

2. Making changes in `main.css` (Again, copy paste :) )
    ```css
    body{
        margin: 0;
        font-family: sans-serif;
    }
    
    /* Center content in the container */
    .content {
        text-align: center;
    }
    
    /* Center the table horizontally */
    table {
        margin: auto;
        border-collapse: collapse;
    }
    
    /* Basic table borders */
    table, th, td {
        border: 1px solid black;
        padding: 4px;
    }
    ```

Try searching `localhost:5000` on your browser and view the output.

## 11. POST and GET methods

- In Flask:

    - GET: Retrieves data from the server. It’s used for reading or displaying data. Data is sent via the URL, visible to the user. Commonly used for loading pages.

    - POST: Sends data to the server to create or update resources. It’s hidden from the URL, typically used for form submissions where data (e.g., login credentials) is sent securely  

1. In `app.py` we update `@app.route('/')` to `@app.route('/', methods=['POST', 'GET'])`
    ```python
    from flask import Flask, render_template, url_for
    from flask_sqlalchemy import SQLAlchemy
    from datetime import datetime

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
    db = SQLAlchemy(app)

    class Todo(db.Model):
        id = db.Column(db.Integer, primary_key = True)
        content = db.Column(db.String(200), nullable=False)
        date_created = db.Column(db.DateTime, default=datetime.utcnow)

        def __repr__(self):
            return '<Task %r>' % self.id

    @app.route('/', methods = ['POST', 'GET']) #Changes made here
    def index():
        return render_template('index.html')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

2. Make slight changes in `index.html` in order to add a form section to the UI
    ```html
    {% extends 'base.html' %}

    {% block head %}

    {% endblock %}

    {% block body %}
    <div class="content">
        <h1>ACM Task Manager</h1>

        <table>
            <tr>
                <th>Task</th>
                <th>Added</th>
                <th>Actions</th>
            </tr>
            <tr>
                <td></td>
                <td></td>
                <td>
                    <a href="">Delete</a>
                    <br>
                    <a href="">Update</a>
                </td>
            </tr>
        </table>

        <form action="/" method="POST">
            <input type="text" name="content" id="content">
            <input type="submit" value="Add Task">
        </form>
    </div>
    {% endblock %}
    ```

3. Try searching `localhost:5000` on your browser and view the output.

## 12. Sending request for POST method

1. First, import `request` and `redirect` into `app.py`
```python
from flask import Flask, render_template, url_for, request, redirect
```

2. Update `app.py`

    ```python
    from flask import Flask, render_template, url_for, request, redirect
    from flask_sqlalchemy import SQLAlchemy
    from datetime import datetime

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
    db = SQLAlchemy(app)

    class Todo(db.Model):
        id = db.Column(db.Integer, primary_key = True)
        content = db.Column(db.String(200), nullable=False)
        date_created = db.Column(db.DateTime, default=datetime.utcnow)

        def __repr__(self):
            return '<Task %r>' % self.id

    @app.route('/', methods = ['POST', 'GET']) #Changes made here
    def index():
        if request.method == 'POST':
            task_content = request.form['content']
            new_task = Todo(content=task_content)

            try:
                db.session.add(new_task)
                db.session.commit()
                return redirect('/')
            except:
                return 'There was an issue adding your task'
        else:
            tasks = Todo.query.order_by(Todo.date_created).all()
            return render_template('index.html', tasks=tasks)

    if __name__ == '__main__':
        app.run(debug=True)
    ```

3. Update `index.html`

    ```html
    {% extends 'base.html' %}

    {% block head %}

    {% endblock %}

    {% block body %}
    <div class="content">
        <h1>ACM Task Manager</h1>

        <table>
            <tr>
                <th>Task</th>
                <th>Added</th>
                <th>Actions</th>
            </tr>
            {%  for task in tasks %}
                <tr>
                    <td>{{ task.content }}</td>
                    <td>{{ task.date_created.date() }}</td>
                    <td>
                        <a href="">Delete</a>
                        <br>
                        <a href="">Update</a>
                    </td>
                </tr>
            {% endfor %}
        </table>

        <form action="/" method="POST">
            <input type="text" name="content" id="content">
            <input type="submit" value="Add Task">
        </form>
    </div>
    {% endblock %}
    ```
4. Try searching `localhost:5000` on your browser and view the output.  
We have basically, finished making the "Add Task" feature by sending request for POST method.

5. Now, we will work on the "Delete" and "Update" feature in our ACM Task Manager.  
    - For that we can add "delete" and "update" function in `app.py` and accordingly update the code:

        ```python
        from flask import Flask, render_template, url_for, request, redirect
        from flask_sqlalchemy import SQLAlchemy
        from datetime import datetime

        app = Flask(__name__)
        app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
        db = SQLAlchemy(app)

        class Todo(db.Model):
            id = db.Column(db.Integer, primary_key = True)
            content = db.Column(db.String(200), nullable=False)
            date_created = db.Column(db.DateTime, default=datetime.utcnow)

            def __repr__(self):
                return '<Task %r>' % self.id

        @app.route('/', methods = ['POST', 'GET']) #Changes made here
        def index():
            if request.method == 'POST':
                task_content = request.form['content']
                new_task = Todo(content=task_content)

                try:
                    db.session.add(new_task)
                    db.session.commit()
                    return redirect('/')
                except:
                    return 'There was an issue adding your task'
            else:
                tasks = Todo.query.order_by(Todo.date_created).all()
                return render_template('index.html', tasks=tasks)

        @app.route('/delete/<int:id>')
        def delete(id):
            task_to_delete = Todo.query.get_or_404(id)

            try:
                db.session.delete(task_to_delete)
                db.session.commit()
                return redirect('/')
            except:
                return 'There was a problem deleting that task'

        @app.route('/update/<int:id>', methods=['GET','POST'])
        def update(id):
            task = Todo.query.get_or_404(id)

            if request.method == 'POST':
                task.content = request.form['content']
                
                try:
                    db.session.commit()
                    return redirect('/')
                except:
                    return 'There was an issue updating your task'
            else:
                return render_template('update.html', task=task)

        if __name__ == '__main__':
            app.run(debug=True)        
        ```
    
    - Update `index.html`:
        ```html
        {% extends 'base.html' %}

        {% block head %}

        {% endblock %}

        {% block body %}
        <div class="content">
            <h1>ACM Task Manager</h1>

            <table>
                <tr>
                    <th>Task</th>
                    <th>Added</th>
                    <th>Actions</th>
                </tr>
                {%  for task in tasks %}
                    <tr>
                        <td>{{ task.content }}</td>
                        <td>{{ task.date_created.date() }}</td>
                        <td>
                            <a href="/delete/{{task.id}}">Delete</a>
                            <br>
                            <a href="/update/{{task.id}}">Update</a>
                        </td>
                    </tr>
                {% endfor %}
            </table>

            <form action="/" method="POST">
                <input type="text" name="content" id="content">
                <input type="submit" value="Add Task">
            </form>
        </div>
        {% endblock %}
        ```

    - Now, we will also have to add the file `update.html` inside templates because, on clicking 'update' we will be redirected to another webpage, where we will update the task and submit it.

        ```html
        {% extends 'base.html' %}

        {% block head %}

        {% endblock %}

        {% block body %}
        <div class="content">
            <h1>Update Task</h1>


            <form action="/update/{{task.id}}" method="POST">
                <input type="text" name="content" id="content" value="{{task.content}}">
                <input type="submit" value="Update">
            </form>
        </div>
        {% endblock %}
        ```

## 13. Conclusion
Congratulations on completing the Flask Workshop! You've built a fully functional task management application with Flask, utilizing various features like routing, templates, static content, and a SQLite database.

***Next Steps***
To further your Flask knowledge, consider exploring:
- **Flask extensions** like Flask-Login for user authentication or Flask-WTF for form handling.
- **Deployment** of your Flask app to platforms like Heroku or DigitalOcean (which is going to be taught in this ACM-Dev Workshop Series in the further sessions).  

***Additional Resources***
- For more information, check out the Flask Documentation and explore additional projects to enhance your skills.

Happy Coding! 🚀
