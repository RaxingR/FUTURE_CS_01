# 🔐 Secure File Sharing System

A Flask-based secure file sharing system for encrypted file upload & download.

---

## 🧠 Project Overview

This project allows secure file sharing using:

- 🔒 AES-256 encryption  
- ⚙️ Python Flask framework  
- 📁 Secure file upload/download  
- 🖥️ HTML templates for UI  

---

## 📂 FUTURE_CS_03/ Directory Structure

├── main.py # Main Flask application
├── templates/ # HTML templates (UI pages)
│ └── index.html
├── uploads/ # Folder where uploaded/encrypted files are stored
└── images/sample.png
├── Encryption_Security.docx


---

## 🧪 Sample Code (`main.py`)

```python
from flask import Flask, render_template, request, redirect, url_for, send_from_directory, flash
import os

app = Flask(__name__)
app.secret_key = 'your-secret-key'
UPLOAD_FOLDER = 'uploads'
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/upload', methods=['POST'])
def upload_file():
    file = request.files['file']
    if file:
        filename = file.filename
        file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
        flash('File uploaded successfully!')
        return redirect(url_for('index'))
    else:
        flash('Invalid file type')
        return redirect(url_for('index'))

@app.route('/download/<filename>')
def download_file(filename):
    return send_from_directory(app.config['UPLOAD_FOLDER'], filename, as_attachment=True)

if __name__ == '__main__':
    app.run(debug=True)

---

## 🧪 Sample Code (`main.py`)
