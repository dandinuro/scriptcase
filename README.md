# Scriptcase Docker Environment

This project offers a ready-to-use Docker image for **Scriptcase 9.11.017**, built with a lightweight and versatile environment including **PHP 8.1**, **MySQL**, and based on **Ubuntu 22.04**. This container is designed for developers who want to use Scriptcase seamlessly with Docker, featuring options for persisting data using Docker volumes.

---

## Features

- **Scriptcase Version**: 9.11.017
- **Base Image**: Ubuntu 22.04
- **PHP Version**: 8.1
- **Database**: MySQL
- **Optional YAML Configuration**: Supports persistent data storage for `/var/www/html/netmake`.

---

## Getting Started

You can run the container with Docker commands or by using the provided **YAML file** for Docker Compose.

### Option A: Running With `docker run`

Run the following command to start the container:

```bash
docker run -d \
  --name scriptcase-app \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/Berlin \
  -e APP_URL=https://subdomain.example.com \
  -v /home/your_user/your_path/netmake:/var/www/html/netmake \
  -p 8097:80 \
  dandinuro/scriptcase
```

### Option B: Using the YAML File (Docker Compose)

For flexibility, use this **docker-compose.yml** file to launch the container, create volumes, and persist data.

```yaml
---
services:
  scriptcase-service:
    image: dandinuro/scriptcase
    container_name: scriptcase-app
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
      - APP_URL=https://subdomain.example.com
    volumes:
      - scriptcase-data:/var/www/html/netmake
    ports:
      - 8097:80/tcp
    tty: true
    dns:
      - 8.8.8.8
      - 8.8.4.4
    restart: unless-stopped

volumes:
  scriptcase-data:
    driver: local
    driver_opts:
      type: none
      device: /home/your_user/your_path/netmake
      o: bind
```

> **Important**: Before using the YAML file, customize the following:
> - `TZ=Europe/Berlin`: Update this to match your timezone.
> - `APP_URL=https://subdomain.example.com`: Add your domain if you're using a reverse proxy.
> - `device: /home/your_user/your_path/netmake`: Set the host directory path to store persistent data.

---

## Environment Variables

| Variable        | Description                                                                          | Example                               |
|------------------|--------------------------------------------------------------------------------------|---------------------------------------|
| `PUID`          | User ID for the container environment                                               | `1000`                                |
| `PGID`          | Group ID for the container environment                                              | `1000`                                |
| `TZ`            | Timezone of the container                                                          | `Europe/Berlin`                       |
| `APP_URL`       | Application URL (useful with reverse proxies)                                       | `https://subdomain.example.com`       |

---

## Ports

The container exposes the following port:
- `8097`: HTTP access to the Scriptcase application.

You can change this in the YAML or Docker run command as needed.

---

## Volumes

The container uses volume mapping to store persistent data. By default, you can map:
- **Host Path**: `/home/your_user/your_path/netmake`
- **Container Path**: `/var/www/html/netmake`

This allows your Scriptcase data to persist even if the container is stopped.

---

## Customization

### Reverse Proxy
If using a reverse proxy (like Nginx, Traefik, or Caddy), update the `APP_URL` variable in the YAML file or `docker run` command.

Example Nginx proxy configuration:
```nginx
server {
    listen 80;
    server_name subdomain.example.com;

    location / {
        proxy_pass http://localhost:8097;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

## License

This project is licensed under the **Apache License 2.0**.

You can view the license in detail [here](LICENSE).

---

## Support

If you encounter issues or have questions, feel free to open an issue in the GitHub repository or comment on Docker Hub.

---

## Additional Resources

- [Scriptcase Documentation](https://www.scriptcase.net/docs/)
- [Docker Official Documentation](https://docs.docker.com/)
- [Apache License 2.0](https://opensource.org/licenses/Apache-2.0)