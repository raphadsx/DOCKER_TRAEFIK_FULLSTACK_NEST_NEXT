
---

# 🚀 FULLSTACK_NEST_ENCORE_NEXT_RSBUILD 
**Plataforma Monolito con Docker**

**FULLSTACK_NEST_ENCORE_NEXT_RSBUILD** 
es un proyecto fullstack moderno que combina **NestJS + Encore.ts** en el backend y **Next.js + Rsbuild** en el frontend.  
Diseñado bajo principios de **Clean Architecture** y **Vertical Slicing**, con autenticación segura, gestión de productos y despliegue simplificado con Docker.

- **Backend**: [NestJS](https://nestjs.com) + [Encore.ts](https://encore.dev/docs/ts/how-to/nestjs)
- **Frontend**: [Next.js](https://nextjs.org) + [Rsbuild](https://rsbuild.dev)
---

## 📋 Tabla de Contenidos
- [🎯 Propósito](#-propósito)  
- [🚀 Inicio Rápido](#-inicio-rápido)  
- [🏗️ Arquitectura](#️-arquitectura)  
- [💻 Desarrollo](#-desarrollo)  
- [🐳 Docker](#-docker)  
- [📡 API](#-api)  
- [🧪 Testing](#-testing)  
- [🚀 Deployment](#-deployment)  
- [🤝 Contribuir](#-contribuir)  

---

## 🎯 Propósito
Este proyecto busca ofrecer una **plataforma modular y segura** para:
- ✅ **Autenticación avanzada** con Better Auth (sesiones + tokens)  
- ✅ **Gestión de usuarios y productos** con Drizzle ORM y NestJS  
- ✅ **Arquitectura limpia** con separación en capas (Domain, Application, Infrastructure, Interface)  
- ✅ **Vertical Slicing**: cada feature es independiente y completa  
- ✅ **Infraestructura tipo-segura** con Encore.ts  
- ✅ **Frontend moderno** con Next.js + Rsbuild  

---

## 🚀 Inicio Rápido

### Prerrequisitos
- **Docker & Docker Compose**  
- **Node.js 20+**  
- **Git**  

### Instalación Rápida
```bash
# 1. Clonar el repositorio
git clone https://github.com/tu-org/FULLSTACK_NEST_ENCORE_NEXT_RSBUILD.git
cd FULLSTACK_NEST_ENCORE_NEXT_RSBUILD

# 2. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus credenciales

# 3. Levantar todos los servicios con Docker
docker compose up -d

# 4. Verificar que los servicios están activos
docker ps --format "table {{.Names}}\t{{.Status}}"

# 5. Acceder a la aplicación
# Frontend: http://localhost:3000
# Backend API: http://localhost:4000
```

---

## 🏗️ Arquitectura

```
FULLSTACK_NEST_ENCORE_NEXT_RSBUILD/
├── backend/          # NestJS + Encore.ts
│   ├── Dockerfile
│   └── src/...
├── frontend/         # Next.js + Rsbuild
│   ├── Dockerfile
│   └── src/...
├── docker-compose.yml
├── shared/        # opcional: tipos/constantes
├── README.md
└── .gitignore
```


### Backend (NestJS + Encore.ts)
```
src/
├── users/
│   ├── domain/          # Entidades, repositorios, servicios
│   ├── application/     # Casos de uso, DTOs
│   ├── infrastructure/  # Persistencia con Drizzle, mappers
│   ├── interface/       # Controladores Encore
│   └── users.module.ts
├── auth/                # Autenticación (Better Auth)
├── products/            # CRUD de productos
└── shared/              # Infraestructura y VO comunes
```

### Frontend (Next.js + Rsbuild)
*(pendiente de detallar en tu siguiente mensaje, aquí se agregará la estructura de `pages/`, `components/`, `rsbuild.config.ts`)*

---

## 🐳 Docker
Ejemplo de `docker-compose.yml`:
```yaml
version: "3.9"
services:
  backend:
    build: ./backend
    ports:
      - "4000:4000"
    env_file:
      - ./backend/.env
    volumes:
      - ./backend:/app

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    env_file:
      - ./frontend/.env
    volumes:
      - ./frontend:/app
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
- **Producción**: Docker Compose + Traefik (opcional para proxy inverso y SSL automático)  
- **Cache**: Redis (opcional para sesiones y rate limiting)  

---

## 🤝 Contribuir
1. Haz un fork del proyecto  
2. Crea una rama (`feature/nueva-funcionalidad`)  
3. Haz commit de tus cambios  
4. Abre un Pull Request  

---

### 💡 Sobre tu duda
- **Traefik**: sí, es muy buena idea si planeas exponer varios servicios y quieres SSL automático + routing limpio.  
- **Redis**: recomendable si necesitas cachear sesiones, tokens o rate limiting. En tu stack con Better Auth y Drizzle, Redis encaja perfecto para performance y seguridad.  

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
- En producción, montar secretos con Docker o un gestor externo (Vault, Doppler, Encore Secrets).
- Para pruebas de API, se incluye un entorno Postman/Insomnia en `docs/` con variables preconfiguradas.

