# n8n con Docker Compose

Este proyecto configura n8n con Traefik como reverse proxy y soporte para SSL/TLS automático.

## Requisitos Previos

- Docker y Docker Compose instalados
- (Opcional) Cuenta de ngrok para desarrollo/pruebas locales

## Configuración Inicial

### 1. Crear archivo .env

Crea un archivo `.env` en la raíz del proyecto con las siguientes variables:

```env
# Configuración de dominio
SUBDOMAIN=n8n
DOMAIN_NAME=tudominio.com
SSL_EMAIL=tu-email@ejemplo.com

# Zona horaria
GENERIC_TIMEZONE=America/colombia

# Configuración de ngrok (opcional para desarrollo)
NGROK_AUTHTOKEN=tu_token_de_ngrok
NGROK_DOMAIN=tu-dominio.ngrok-free.app
```


## Comandos Docker Compose

### Levantar los contenedores

```bash
# Iniciar en segundo plano
docker-compose up -d

# Iniciar y ver logs
docker-compose up
```

### Detener los contenedores

```bash
# Detener los contenedores
docker-compose down

# Detener y eliminar volúmenes (¡cuidado! elimina datos)
docker-compose down -v
```

### Ver logs

```bash
# Ver logs de todos los servicios
docker-compose logs -f

# Ver logs solo de n8n
docker-compose logs -f n8n

# Ver logs solo de traefik
docker-compose logs -f traefik
```

### Reiniciar servicios

```bash
# Reiniciar todos los servicios
docker-compose restart

# Reiniciar solo n8n
docker-compose restart n8n
```

## Configuración con ngrok (Desarrollo/Pruebas)

Para desarrollo local o pruebas sin un dominio público, puedes usar ngrok:

### 1. Instalar ngrok

```bash
# macOS
brew install ngrok

# Linux
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list
sudo apt update && sudo apt install ngrok
```

### 2. Configurar ngrok con tu token

```bash
ngrok config add-authtoken TU_TOKEN_DE_NGROK
```

### 3. Ejecutar ngrok

```bash
# Para exponer el puerto 5678 (n8n)
ngrok http 5678

# Con un dominio personalizado (requiere plan de pago)
ngrok http --domain=tu-dominio.ngrok-free.app 5678
```

## Acceder a n8n

- **Con dominio propio**: `https://n8n.tudominio.com`
- **Con ngrok**: Usa la URL que proporciona ngrok (ej: `https://abc123.ngrok-free.app`)
- **Local**: `http://localhost:5678` (solo si no usas Traefik)

## Estructura de Directorios

```
.
├── compose.yaml          # Configuración de Docker Compose
├── .env                  # Variables de entorno (no incluido en git)
├── local-files/          # Archivos locales accesibles desde n8n
├── workflows/            # Workflows de n8n
│   └── BotContabilidad/
└── README.md            # Este archivo
```

## Solución de Problemas

### Error de permisos en n8n

```bash
docker-compose exec n8n chown -R node:node /home/node/.n8n
```

### Ver estado de los contenedores

```bash
docker-compose ps
```

### Recrear contenedores

```bash
docker-compose up -d --force-recreate
```

### Limpiar todo y empezar de nuevo

```bash
docker-compose down -v
docker-compose up -d
```

## Notas Importantes

- El certificado SSL se genera automáticamente con Let's Encrypt
- Los datos de n8n se persisten en el volumen `n8n_data`
- Los archivos en `./local-files` son accesibles desde n8n en `/files`
- Para producción, se recomienda usar un dominio real con Traefik
- Para desarrollo/pruebas, ngrok es una excelente alternativa

