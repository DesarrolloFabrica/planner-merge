# 🚀 Planner Fabrica API Server

Backend API para Planner Fabrica - Fábrica de Contenido con Express + Node.js + PostgreSQL

## Instalación

```bash
cd server
npm install
```

## Configuración

Crea un archivo `.env` local. No publiques este archivo ni credenciales reales:

```env
PGHOST=localhost
PGPORT=5432
PGDATABASE=planner_db
PGUSER=postgres
PGPASSWORD=your_local_password
PORT=3001
JWT_SECRET=replace_with_a_long_random_secret
CORS_ORIGIN=http://localhost:8080
```

## Ejecutar el servidor

```bash
# Modo desarrollo (con hot reload)
npm run dev

# Modo producción
npm run build
npm start
```

El servidor se ejecutará en **http://localhost:3001**

## Endpoints Disponibles

### Health Check
```
GET /healthz
GET /readyz
```

### Autenticación

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "usuario@ejemplo.com",
  "password": "contraseña_configurada_localmente"
}
```

**Respuesta:**
```json
{
  "user": {
    "id": "uuid",
    "email": "usuario@ejemplo.com",
    "full_name": "Usuario de ejemplo",
    "avatar_url": null,
    "role": "admin"
  },
  "token": "jwt.token.here"
}
```

#### Register
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "nuevo@cun.edu.co",
  "password": "password123",
  "full_name": "Nuevo Usuario"
}
```

#### Get Current User
```http
GET /api/auth/me
Authorization: Bearer <token>
```

#### Logout
```http
POST /api/auth/logout
Authorization: Bearer <token>
```

## Estructura del Proyecto

```
server/
├── src/
│   ├── config/
│   │   └── database.ts      # Configuración de PostgreSQL
│   ├── controllers/
│   │   └── authController.ts # Controladores de autenticación
│   ├── middleware/
│   │   └── auth.ts           # Middleware de autenticación JWT
│   ├── routes/
│   │   └── auth.ts           # Rutas de autenticación
│   └── index.ts              # Servidor Express principal
├── .env                      # Variables de entorno
├── package.json
└── README.md
```

## Reportes

La metodología de capacidad, las fuentes de horas y los endpoints agregados en la rama `copia` están documentados en [README_REPORTES.md](./README_REPORTES.md).

## Usuarios de desarrollo

Las cuentas de prueba deben crearse localmente mediante los scripts de base de datos y sus contraseñas deben cambiarse antes de usar cualquier entorno compartido.

## Próximos Pasos

1. ✅ Configurar migraciones de base de datos
2. ✅ Crear backend API
3. ⏳ Actualizar frontend para usar API local
4. ⏳ Agregar endpoints para proyectos y tareas
