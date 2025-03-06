# Flask Cheat Sheet

---

## **1. Installation & Setup**  
```bash
pip install flask
```
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Flask!"

if __name__ == '__main__':
    app.run(debug=True)  # Start the server
```

---

## **2. Routing & Views**  
```python
@app.route('/hello/<name>')
def hello(name):
    return f"Hello, {name}!"

@app.route('/user/<int:user_id>')  # Type conversion
def user_profile(user_id):
    return f"User ID: {user_id}"
```

---

## **3. HTTP Methods**  
```python
@app.route('/submit', methods=['GET', 'POST'])
def submit():
    if request.method == 'POST':
        return "Form Submitted!"
    return "Send a POST request to submit."
```

---

## **4. Handling Request Data**  
```python
from flask import request

@app.route('/data', methods=['POST'])
def get_data():
    json_data = request.get_json()  # JSON data
    form_data = request.form['name']  # Form data
    return f"Received: {json_data or form_data}"
```

---

## **5. Rendering Templates**  
```bash
mkdir templates  # Create templates folder
```
`templates/index.html`
```html
<!DOCTYPE html>
<html>
<head><title>Flask App</title></head>
<body>
    <h1>Welcome, {{ name }}</h1>
</body>
</html>
```
```python
from flask import render_template

@app.route('/welcome/<name>')
def welcome(name):
    return render_template('index.html', name=name)
```

---

## **6. Redirects & URL Handling**  
```python
from flask import redirect, url_for

@app.route('/old-page')
def old_page():
    return redirect(url_for('new_page'))

@app.route('/new-page')
def new_page():
    return "This is the new page!"
```

---

## **7. Session & Cookies**  
```python
from flask import session

app.secret_key = 'supersecretkey'  # Required for sessions

@app.route('/set/')
def set_session():
    session['user'] = 'FlaskUser'
    return "Session set!"

@app.route('/get/')
def get_session():
    return f"User: {session.get('user')}"
```

---

## **8. REST API with Flask**  
```python
from flask import jsonify

@app.route('/api/data')
def api_data():
    return jsonify({'name': 'Flask', 'version': '2.0'})
```

---

## **9. Flask with SQLite Database**  
```python
from flask_sqlalchemy import SQLAlchemy

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)

@app.route('/add_user/<name>')
def add_user(name):
    user = User(username=name)
    db.session.add(user)
    db.session.commit()
    return f"User {name} added!"
```

---

## **10. Running the Flask App**  
```bash
flask run  # Run the app (set FLASK_APP=app.py first)
```
