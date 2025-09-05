# MyProject - Django Cover Letter Demo

A simple Django demo application that serves a static **Cover Letter** page.  
This project is structured for local development and Kubernetes deployment (EKS) with ALB ingress.

---

## 🚀 Features

- Single Django app (`demo`) rendering a static Cover Letter
- Clean project structure with templates
- Ready for static files (CSS/JS/images)
- Kubernetes-ready deployment manifests
- Easy to extend for additional Django apps

---

## 📦 Local Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/Kunal2703/myproject.git
cd myproject
2. Create and activate a virtual environment
macOS/Linux:

bash
Copy code
python -m venv venv
source venv/bin/activate
Windows:

bash
Copy code
python -m venv venv
venv\Scripts\activate
3. Install dependencies
bash
Copy code
pip install django
4. Apply migrations
bash
Copy code
python manage.py migrate
5. Start development server
bash
Copy code
python manage.py runserver
Open http://127.0.0.1:8000/ to view your Cover Letter page 🎉

🛠 Tech Stack
Python 3

Django 5.x

HTML/CSS (static page)

Kubernetes (Deployment, Service, Ingress)

AWS EKS / ALB

📂 Project Structure
arduino
Copy code
myproject/
├── demo/
│   ├── templates/
│   │   └── home.html
│   ├── views.py
│   └── urls.py
├── myproject/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
├── manage.py
└── README.md
⚙️ Kubernetes Deployment (AWS EKS)
Deployment (k8s/deployment.yaml)
yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myproject-deployment
  labels:
    app: myproject
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myproject
  template:
    metadata:
      labels:
        app: myproject
    spec:
      containers:
        - name: myproject
          image: <your-dockerhub-username>/myproject:latest
          ports:
            - containerPort: 8000
Service (k8s/service.yaml)
yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: myproject-service
  labels:
    app: myproject
spec:
  type: NodePort
  selector:
    app: myproject
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
Ingress (ALB) (k8s/ingress.yaml)
yaml
Copy code
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myproject-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP":80}]'
    alb.ingress.kubernetes.io/load-balancer-name: myproject-alb
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: myproject-service
                port:
                  number: 80
Apply Kubernetes Manifests
bash
Copy code
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
Verify Resources
bash
Copy code
kubectl get pods
kubectl get svc
kubectl get ingress
Your Django app will be accessible through the ALB address listed under kubectl get ingress.

💡 Deployment Notes
Replace <your-dockerhub-username> with your Docker Hub username

Ensure EKS nodes have IAM permissions for ALB controller

Configure aws-load-balancer-controller in the kube-system namespace

Use proper Security Groups to allow HTTP/HTTPS traffic

💻 Contribution
Fork the project, submit PRs for bug fixes or features

Open issues for suggestions or improvements

📄 License
MIT License – free to use and modify.

pgsql
Copy code

---

If you want, I can also **add the Dockerfile + instructions to build, push, and deploy automatically** to EKS in this single README so it becomes truly end-to-end.  

Do you want me to do that?
