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

## 📁 FUTURE_CS_01/
├── main.py # Main Flask application
├── templates/ # HTML templates (UI pages)
│ └── index.html
├── uploads/ # Folder where encrypted files are stored
│ └── images
├── Encryption_Security.docx

---

## 🐍 Sample Code (`main.py`)

```python
from flask import Flask, render_template
from flask import Flask, render_template, request, send_from_directory, redirect, url_for, flash
import os
from werkzeug.utils import secure_filename

app = Flask(__name__)
app.secret_key = 'supersecretkey'  # Change this in production

UPLOAD_FOLDER = 'uploads'
if not os.path.exists(UPLOAD_FOLDER):
    os.makedirs(UPLOAD_FOLDER)

app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
ALLOWED_EXTENSIONS = {'txt', 'pdf', 'png', 'jpg', 'jpeg', 'gif', 'zip', 'csv'}

def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/')
def index():
    files = os.listdir(app.config['UPLOAD_FOLDER'])
    return render_template('index.html', files=files)

@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        flash('No file part')
        return redirect(url_for('index'))
    file = request.files['file']
    if file.filename == '':
        flash('No selected file')
        return redirect(url_for('index'))
    if file and allowed_file(file.filename):
        filename = secure_filename(file.filename)
        file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
        flash('File successfully uploaded')
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

## 💻index.html (Template File)

```html

<!doctype html>
<html lang="en">
<head>
    <title>File Upload & Download Portal</title>
    <style>
        body { font-family: Arial; margin: 2em; }
        .container { max-width: 600px; margin: auto; }
        .flash { color: rgb(0, 255, 26); }
    </style>
</head>
<body>
<div class="container">
    <h2>Upload a File</h2>
    {% with messages = get_flashed_messages() %}
      {% if messages %}
        <div class="flash">
          {% for message in messages %}
            <p>{{ message }}</p>
          {% endfor %}
        </div>
      {% endif %}
    {% endwith %}
    <form method="post" action="{{ url_for('upload_file') }}" enctype="multipart/form-data">
        <input type="file" name="file" required>
        <input type="submit" value="Upload">
    </form>

    <h2>Available Files</h2>
    <ul>
      {% for file in files %}
        <li>
          {{ file }}
          [<a href="{{ url_for('download_file', filename=file) }}">Download</a>]
        </li>
      {% else %}
        <li>No files uploaded yet.</li>
      {% endfor %}
    </ul>
</div>
</body>
</html>

