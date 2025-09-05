# MyProject - Django Cover Letter Demo

A simple Django demo application that serves a static **Cover Letter** page.
This project is structured for local development and Kubernetes deployment (EKS) with ALB ingress.

---

## 🚀 Features

* Single Django app (`demo`) rendering a static Cover Letter
* Clean project structure with templates
* Ready for static files (CSS/JS/images)
* Kubernetes-ready deployment manifests
* Easy to extend for additional Django apps
* CI/CD ready for automated deployment

---

## 💾 Local Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/Kunal2703/myproject.git
cd myproject
```

### 2. Create and activate a virtual environment

**macOS/Linux**

```bash
python -m venv venv
source venv/bin/activate
```

**Windows**

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install django
```

### 4. Apply migrations

```bash
python manage.py migrate
```

### 5. Start development server

```bash
python manage.py runserver
```

Open [http://127.0.0.1:8000/](http://127.0.0.1:8000/) to view your Cover Letter page 🎉

---

## 🔧 Tech Stack

* Python 3
* Django 5.x
* HTML/CSS (static page)
* Kubernetes (Deployment, Service, Ingress)
* AWS EKS / ALB
* Docker

---

## 📂 Project Structure

```text
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
├── Dockerfile
├── manage.py
└── README.md
```

---

## ⚙️ Docker Build & Run

### 1. Build Docker image

```bash
docker build -t myproject:latest .
```

### 2. Run container locally

```bash
docker run -p 8000:8000 myproject:latest
```

---

## 🏋️ Kubernetes Deployment (AWS EKS)

### Deployment (`k8s/deployment.yaml`)

```yaml
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
```

### Service (`k8s/service.yaml`)

```yaml
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
```

### Ingress (ALB) (`k8s/ingress.yaml`)

```yaml
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
```

### Apply Kubernetes Manifests

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### Verify Resources

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
```

Your Django app will be accessible through the ALB address listed under `kubectl get ingress`.

---

## 💡 CI/CD Notes

* Use GitHub Actions or AWS CodePipeline
* Source stage: trigger on push to main branch
* Build stage: Docker image build, basic security scan, push to ECR
* Deploy stage: deploy to staging, run integration tests, deploy to production with manual approval

---

## 📄 Terraform Infrastructure Reference

For full infrastructure provisioning (VPC, ALB, RDS, EC2, Bastion host, Security Groups), refer to the Terraform repo:
[https://github.com/Kunal2703/myproject-terraform.git](https://github.com/Kunal2703/myproject-terraform.git)

---

## 📝 Contribution

* Fork the project, submit PRs for bug fixes or features
* Open issues for suggestions or improvements

---

## 📜 License

MIT License – free to use and modify.
