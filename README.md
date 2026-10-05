# docker-todo-app1
deployment.yaml :
apiVersion: apps/v1
kind: Deployment
metadata:
  name: todo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: todo-app
  template:
    metadata:
      labels:
        app: todo-app
    spec:
      containers:
        - name: todo-container
          image: todo-app:1.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 3000
          volumeMounts:
            - name: todo-storage
              mountPath: /app/data
      volumes:
        - name: todo-storage
        
          persistentVolumeClaim:
            claimName: todo-pvc
pvc.yaml:
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: todo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

service.yaml :
apiVersion: v1
kind: Service
metadata:
  name: todo-service
spec:
  type: NodePort
  selector:
    app: todo-app
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 30080
