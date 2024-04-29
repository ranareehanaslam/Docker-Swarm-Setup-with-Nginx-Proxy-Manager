# Docker Swarm Setup with Nginx Proxy Manager 🚀

This guide provides step-by-step instructions on how to deploy Nginx Proxy Manager in a Docker Swarm environment, ideal for managing proxies with a simple web interface.

## 📋 Prerequisites

- Docker and Docker Compose installed
- Access to the command line interface of your server
- Docker Swarm initialized on your node

## Step 1: Initialize Docker Swarm

Initialize Docker Swarm mode on your manager node using your public IP address:

```bash
docker swarm init --advertise-addr public-ip
```

## 🛠 Step 2: Create Docker Compose File

Create a `docker-compose.yml` file with the following content. Make sure to replace `<yourpassword>` with a secure password of your choice.

```yaml
version: '3.9'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "StrongPassword@2024"
      DB_MYSQL_NAME: "npm"
    volumes:
      - npm_data:/data
      - npm_letsencrypt:/etc/letsencrypt
    networks:
      - npm-db
      - nginx_ingress

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'StrongPassword@2024'
    volumes:
      - npm_db_data:/var/lib/mysql
    networks:
      - npm-db

volumes:
  npm_data:
    driver: local
  npm_letsencrypt:
    driver: local
  npm_db_data:
    driver: local

networks:
  npm-db:
    name: npm-db
    attachable: true
  nginx_ingress:
    name: nginx_ingress
    attachable: true

```

## 🌐 Step 3: Deploy Your Stack

Deploy your stack using Docker Swarm with the following command:

```bash
docker stack deploy -c docker-compose.yml nginx_proxy_manager
```

## 🔗 Step 4: Access Nginx Proxy Manager

After deployment, access the Nginx Proxy Manager by opening your web browser and navigating to:

```
http://your-server-ip:81
```

Use the default credentials for initial login:
- **Username:** `admin@example.com`
- **Password:** `changeme`

🔒 **Important:** Change the default credentials immediately after your first login to secure your setup.

## 📚 Step 5: Configure Nginx Proxy Manager

1. Add proxy hosts to manage your domains.
2. Set up SSL with Let's Encrypt directly from the dashboard.
3. Customize access and forwarding rules as needed.

## ✔️ Step 5: Verify the Setup

Ensure all services are running smoothly:

```bash
docker service ls
```

Check the logs if you encounter any issues:

```bash
docker service logs <service_name>
```

## 📝 Conclusion

Congratulations! You have successfully set up Nginx Proxy Manager on Docker Swarm. Now, you can easily manage your proxy settings through a user-friendly web interface.

Feel free to contribute to the documentation or suggest improvements. Happy proxying! 🎉
```

This `README.md` includes all the necessary steps, explanations, and visual enhancements to make the setup process clear and easy to follow. Adjust the file paths, network names, and configurations as needed based on your specific environment and security policies.
