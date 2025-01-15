aws maven web
FROM tomcat:9-jdk11
COPY target/*.war /usr/local/tomcat/webapps

aws web
FROM nginx:alpine
COPY ./usr/share/nginx/html
sudo docker build -t mywebapp .
sudo docker run -d -p 80:80 mywebapp

minikube
kubectl create deployment mynginx --image=nginx
kubectl expose deployment --type=NodePort --port=80 --target-port=80
kubectl scale deployment mynginx --replicas=4
kubectl port-forward svc/mynginx 8091:80

docker-compose:
services:
  wordpress:  # WordPress service
    image: wordpress:latest
    ports:
      - "6060:80"  # Map port 80 of the container to port 8080 of the host
    environment:
      WORDPRESS_DB_HOST: db:3306  # Database host
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db  # Ensures the db service starts first

  db:  # MySQL service
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
