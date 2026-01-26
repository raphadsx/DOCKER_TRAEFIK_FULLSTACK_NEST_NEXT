
---

# 🚀 DOCKER_TRAEFIK_FULLSTACK_NEST_NEXT  
**Plataforma Fullstack con Traefik + Docker Compose**

**DOCKER_TRAEFIK_FULLSTACK_NEST_NEXT** es un proyecto fullstack moderno que combina **NestJS** en el backend y **Next.js** en el frontend.  
Diseñado bajo principios de **Clean Architecture** y **Vertical Slicing**, con autenticación segura, gestión de productos y despliegue simplificado con **Docker Compose + Traefik** para routing y SSL automático.

- **Backend**: [NestJS](https://nestjs.com)  
- **Frontend**: [Next.js](https://nextjs.org)  
- **Proxy / Routing**: [Traefik](https://traefik.io)  

---

## 📋 Tabla de Contenidos
- [🎯 Propósito](#-propósito)  
- [🚀 Inicio Rápido](#-inicio-rápido)  
- [🏗️ Arquitectura](#️-arquitectura)  
- [💻 Desarrollo](#-desarrollo)  
- [🐳 Docker + Traefik](#-docker--traefik)  
- [📡 API](#-api)  
- [🧪 Testing](#-testing)  
- [🚀 Deployment](#-deployment)  
- [🤝 Contribuir](#-contribuir)  
- [🔒 Secrets Management](#-secrets-management)  

---

## 🎯 Propósito
Este proyecto busca ofrecer una **plataforma modular y segura** para:
- ✅ **Autenticación avanzada** con Better Auth (sesiones + tokens)  
- ✅ **Gestión de usuarios y productos** con Drizzle ORM y NestJS  
- ✅ **Arquitectura limpia** con separación en capas (Domain, Application, Infrastructure, Interface)  
- ✅ **Vertical Slicing**: cada feature es independiente y completa  
- ✅ **Frontend moderno** con Next.js  
- ✅ **Routing y SSL automático** con Traefik  

---

## 🚀 Inicio Rápido

### Prerrequisitos
- **Docker & Docker Compose**  
- **Node.js 20+**  
- **Git**  

### Instalación Rápida
```bash
# 1. Clonar el repositorio
git clone https://github.com/tu-org/DOCKER_TRAEFIK_FULLSTACK_NEST_NEXT.git
cd DOCKER_TRAEFIK_FULLSTACK_NEST_NEXT

# 2. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales

# 3. Levantar todos los servicios con Docker Compose
docker compose up -d

# 4. Verificar que los servicios están activos
docker ps --format "table {{.Names}}\t{{.Status}}"

# 5. Acceder a la aplicación
# Frontend: http://localhost
# Backend API: http://localhost/api
```

---

## 🏗️ Arquitectura

```
DOCKER_TRAEFIK_FULLSTACK_NEST_NEXT/
├── backend/          # NestJS
│   ├── Dockerfile
│   └── src/...
├── frontend/         # Next.js
│   ├── Dockerfile
│   └── src/...
├── docker-compose.yml
├── README.md
└── .gitignore
```

### Backend (NestJS)
```
src/
├── users/
│   ├── domain/          # Entidades, repositorios, servicios
│   ├── application/     # Casos de uso, DTOs
│   ├── infrastructure/  # Persistencia con Drizzle, mappers
│   ├── interface/       # Controladores REST
│   └── users.module.ts
├── auth/                # Autenticación (Better Auth)
├── products/            # CRUD de productos
└── shared/              # Infraestructura y VO comunes
```

### Frontend (Next.js)
```
src/
├── pages/               # Rutas de la aplicación
├── components/          # Componentes reutilizables
├── styles/              # Estilos globales
├── lib/                 # Configuración y helpers
└── next.config.js
```

---

## 🐳 Docker + Traefik

Ejemplo de `docker-compose.yml`:

```yaml
version: "3.9"
services:
  backend:
    build: ./backend
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.backend.rule=PathPrefix(`/api`)"
      - "traefik.http.services.backend.loadbalancer.server.port=4000"
    env_file:
      - ./backend/.env
    volumes:
      - ./backend:/app

  frontend:
    build: ./frontend
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.frontend.rule=PathPrefix(`/`)"
      - "traefik.http.services.frontend.loadbalancer.server.port=3000"
    env_file:
      - ./frontend/.env
    volumes:
      - ./frontend:/app

  traefik:
    image: traefik:v3.0
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080" # Dashboard
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

---

## 📡 API
- `GET /api/v1/users` → Listar usuarios  
- `POST /api/v1/auth/login` → Autenticación  
- `GET /api/v1/products` → Listar productos  

*(Se irá ampliando con la documentación de cada módulo)*

---

## 🧪 Testing
- **Backend**: Jest + Supertest  
- **Frontend**: Playwright + React Testing Library  

---

## 🚀 Deployment
- **Producción**: Docker Compose + Traefik (routing + SSL automático con Let's Encrypt)  
- **Cache**: Redis (opcional para sesiones y rate limiting)  

---

## 🤝 Contribuir
1. Haz un fork del proyecto  
2. Crea una rama (`feature/nueva-funcionalidad`)  
3. Haz commit de tus cambios  
4. Abre un Pull Request  

---

## 🔒 Secrets Management

Este proyecto utiliza variables de entorno y secretos para manejar credenciales sensibles de forma segura.

### Desarrollo Local
- Se usa el archivo `.env` (ignorado por Git) para credenciales reales.  
- El archivo `.env.example` documenta las variables necesarias y sirve como plantilla.  

### Producción
- Los secretos se gestionan mediante **Docker secrets** en `docker-compose.prod.yml`.  
- Los archivos de secretos se encuentran en el directorio `.secrets/` y **no deben versionarse**.  

### Archivos de secretos
- `.secrets/db_password.txt` → contraseña de la base de datos  
- `.secrets/jwt_secret.txt` → clave secreta para JWT  
- `.secrets/redis_password.txt` → contraseña de Redis  

### Buenas prácticas
- Nunca subir `.env` ni `.secrets/` al repositorio.  
- Usar `.env.example` para documentar variables.  
- En producción, montar secretos con Docker o un gestor externo (Vault, Doppler, Traefik + Let's Encrypt).  
- Para pruebas de API, se incluye un entorno Postman/Insomnia en `docs/` con variables preconfiguradas.  

---
