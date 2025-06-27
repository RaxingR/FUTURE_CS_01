# 🔐 FUTURE_CS_01 – Secure File Sharing System

A Flask-based secure file sharing system for encrypted file upload & download.

---

## 🧠 Project Overview

This project allows secure file sharing using:
- AES-256 encryption
- Python Flask framework
- Secure file upload/download
- HTML templates for UI

---

## 📁 Folder Structure


---

## 🐍 Sample Code (`main.py`)

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template("index.html")

if __name__ == '__main__':
    app.run(debug=True)
<!DOCTYPE html>
<html>
<head>
    <title>Secure File Sharing</title>
</head>
<body>
    <h1>Welcome to Secure File Upload</h1>
</body>
</html>
