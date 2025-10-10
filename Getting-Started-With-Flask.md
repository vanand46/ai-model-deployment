# Getting Started with Flask

**Flask** is a lightweight and flexible Python **microframework** used for building web applications and APIs.  
In this tutorial, we’ll use Flask to construct an API that provides two main endpoints:

- **`/train`** — to train a model  
- **`/predict`** — to make predictions  

We will deploy our Flask app as part of a **containerized environment** to follow **production best practices** for model deployment.

## Why Flask?

Flask is widely used in data science and machine learning projects because it’s:
- Simple to set up and extend.
- Perfect for building APIs around trained models.
- Easy to containerize for production.

**Alternatives to Flask** include:
- **Django**
- **web2py**
- **FastAPI**

Each of these frameworks can also be containerized using the same general approach.

## Prerequisite: Setting Up a Virtual Environment

Before installing Flask, it’s good practice to create an **isolated Python virtual environment (venv)**.  
This helps avoid dependency conflicts and keeps your project clean.

### Steps to set up a virtual environment
1. **Create a project folder** (for example `flask-lab`):
   
   ```bash
   mkdir flask-lab
   cd flask-lab
   ```
2. **Create a virtual environment** using the built-in venv module:
```bash
python3 -m venv venv
```  
3. **Activate the virtual environment**:
```bash
source venv/bin/activate
```


## Installing Flask

If Flask is not already installed, you can add it using:

```bash
pip3 install flask
```

Verify the installation,
```bash
python -m flask --version
```

## "Hello World!" Example
A minimal Flask app to verify the setup:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(debug=True)
```

### Steps to Run:
1. **Save the code** in a file named `programs/flask_example.py`
2. **Run the file**
```bash
python3 programs/flask_example.py
```
3. **Open** your browser and navigate to:
```cpp
http://127.0.0.1:5000/
```
You can see the **Hello World!**

To Stop the server, press `Ctrl + C`

## **Note** Understanding **WSGI** (Web Server Gateway Interface)
**WSGI** stands for **Web Server Gateway Interface**.
It is the standard interface between web servers and Python web applications or frameworks (like Flask, Django, and FastAPI).

### Why WSGI Matters
When you run app.run(), Flask uses its built-in development server,great for testing, but not suitable for production.
In production, Flask apps are typically run using a WSGI server such as:
- Gunicorn
- Waitress
- uWSGI

These WSGI servers provide better performance, scalability, and security.

[Previous Page](Getting-Started-With-Docker.md) -  [Next Page](Getting-Started-With-Docker.md)