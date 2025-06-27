# 🔐 Secure File Sharing System

A Flask-based secure file sharing system for encrypted file upload & download.

---

## 🧠 Project Overview

This project allows secure file sharing using:

- 🔒 AES-256 encryption  
- ⚙️ Python Flask framework  
- 📁 Secure file upload/download  
- 🖥️ HTML templates for UI  
- 📦 pip install flask in vs terminal
---

## 📂 FUTURE_CS_03/ Directory Structure

├── main.py # Main Flask application
├── templates/ # HTML templates (UI pages)
│ └── index.html
├── uploads/ # Folder where uploaded/encrypted files are stored
└── images/sample.png
├── Encryption_Security.docx


---

## 🧪 Sample Code (`main.py')

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


```

## 🌐 HTML Template (`index.html`)

<h2>🌐 HTML Template: index.html</h2>

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
