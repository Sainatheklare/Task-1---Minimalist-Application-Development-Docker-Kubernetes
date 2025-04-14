# Task-1---Minimalist-Application-Development-Docker-Kubernetes
Project Structure
css
Copy
Edit
simple-time-service/
├── app/
│   └── main.py
├── Dockerfile
├── .dockerignore
├── .gitignore
├── README.md
└── requirements.txt
✅ app/main.py
python
Copy
Edit
from flask import Flask, request, jsonify
from datetime import datetime

app = Flask(__name__)

@app.route("/", methods=["GET"])
def get_time():
    timestamp = datetime.utcnow().isoformat() + "Z"
    ip = request.headers.get("X-Forwarded-For", request.remote_addr)
    return jsonify({
        "timestamp": timestamp,
        "ip": ip
    })

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
✅ requirements.txt
ini
Copy
Edit
Flask==2.3.3
✅ Dockerfile
Dockerfile
Copy
Edit
FROM python:3.11-slim

# Create a non-root user
RUN useradd -m simpleuser

# Set working directory
WORKDIR /app

# Copy application files
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app/ ./app
WORKDIR /app/app

# Set permissions
RUN chown -R simpleuser:simpleuser /app

# Switch to non-root user
USER simpleuser

EXPOSE 5000

CMD ["python", "main.py"]
✅ .dockerignore
markdown
Copy
Edit
__pycache__/
*.pyc
*.pyo
*.pyd
.env
✅ README.md
markdown
Copy
Edit
# SimpleTimeService

A minimalist microservice that returns the current timestamp and visitor IP in JSON format.

## 🧪 Example Response

```json
{
  "timestamp": "2025-04-15T10:30:00Z",
  "ip": "203.0.113.1"
}
🐳 How to Build and Run
Build Docker Image
bash
Copy
Edit
docker build -t simpletimeservice .
Run Container
bash
Copy
Edit
docker run -p 5000:5000 simpletimeservice
Access
Open your browser or use curl:

arduino
Copy
Edit
http://localhost:5000/
🔒 Security
Runs as non-root user (simpleuser)

Small base image (python:3.11-slim)

yaml
Copy
Edit

---

### ✅ Public Git Repository

Once you've added your files, push to a new GitHub repo:

```bash
git init
git remote add origin https://github.com/<your-username>/simple-time-service.git
git add .
git commit -m "Initial commit - SimpleTimeService"
git push -u origin main
✅ Docker Hub Publishing (Example)
bash
Copy
Edit
docker tag simpletimeservice yourdockerhubusername/simpletimeservice
docker push yourdockerhubusername/simpletimeservice
