# FUTURE_CS_01

This is a secure file sharing system using Flask and AES encryption.

## 🐍 main.py Example

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Future Interns!"

