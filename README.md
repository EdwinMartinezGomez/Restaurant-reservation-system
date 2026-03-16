# Restaurant Reservation System

Plataforma full-stack de reservas de restaurantes construida con **Node.js + Express** (backend), **React + Vite** (frontend) y **TiDB Cloud** (base de datos serverless compatible con MySQL).

---

## 📁 Estructura Del Proyecto

```
restaurant-reservation-system/
├── backend/                  # API REST con Node.js + Express
│   ├── src/
│   │   ├── config/
│   │   │   ├── db.js         # Pool de conexion TiDB/MySQL
│   │   │   └── schema.sql    # Esquema de base de datos
│   │   ├── controllers/      # Logica de negocio
│   │   ├── middleware/       # Auth y manejo de errores
│   │   ├── models/           # Capa de acceso a datos
│   │   └── routes/           # Rutas de Express
│   ├── .env.example
│   └── package.json
│
└── frontend/                 # SPA con React + Vite
    ├── src/
    │   ├── __tests__/        # Pruebas de componentes (Vitest)
    │   ├── components/       # Componentes UI compartidos
    │   ├── context/          # React Context (auth)
    │   ├── hooks/            # Hooks personalizados
    │   ├── pages/            # Paginas por ruta
    │   └── services/         # Servicios API con Axios
    ├── .env.example
    └── package.json
```

---

## 🚀 Inicio Rapido

### Prerrequisitos

- Node.js ≥ 18
- Una cuenta gratuita de [TiDB Cloud](https://tidbcloud.com)

### 1. Crear La Base De Datos En TiDB

1. Inicia sesion en TiDB Cloud y crea un cluster/base de datos (ejemplo: `electivaI`).
2. Abre **SQL Editor** y ejecuta el script de [backend/src/config/schema.sql](backend/src/config/schema.sql).
3. Ve a **Connect → MySQL/Node.js** y copia los valores de conexion.

### 2. Configurar Backend

```bash
cd backend
cp .env.example .env
# Completa DATABASE_HOST, DATABASE_PORT, DATABASE_USERNAME, DATABASE_PASSWORD,
# DATABASE_NAME y JWT_SECRET
npm install
npm run dev
# → http://localhost:3001
```

### 3. Configurar Frontend

```bash
cd frontend
# Opcional en desarrollo local (Vite proxy gestiona /api)
# Obligatorio en build de produccion:
# VITE_API_BASE_URL=https://your-backend.onrender.com/api
npm install
npm run dev
# → http://localhost:5173
```

---

## 🌐 URLs En Produccion

- Frontend: https://restaurant-frontend-xcuz.onrender.com
- Backend API: https://restaurant-reservation-system-2ki2.onrender.com
- Health check: https://restaurant-reservation-system-2ki2.onrender.com/api/health

---

## 👤 Acceso Demo

Si cargaste los datos semilla, puedes iniciar sesion sin registrarte:

- Email: `admin@demo.com`
- Password: `Admin123!`

Si la cuenta no existe todavia, registrate.

---

## 🔑 Variables De Entorno

### Backend (`backend/.env`)

| Variable | Description |
|---|---|
| `PORT` | Puerto del servidor (por defecto `3001`) |
| `NODE_ENV` | `development` o `production` |
| `DATABASE_HOST` | Host de TiDB (sin `http://`) |
| `DATABASE_PORT` | Puerto de TiDB (normalmente `4000`) |
| `DATABASE_USERNAME` | Usuario SQL de TiDB |
| `DATABASE_PASSWORD` | Password SQL de TiDB |
| `DATABASE_NAME` | Nombre de la base de datos |
| `JWT_SECRET` | Clave secreta para firmar JWT |
| `JWT_EXPIRES_IN` | Tiempo de vida del JWT (ej. `7d`) |
| `CORS_ORIGIN` | Lista de origenes permitidos separada por comas |

### Frontend (`frontend/.env`)

| Variable | Description |
|---|---|
| `VITE_API_BASE_URL` | URL base de la API (obligatoria en produccion) |

---

## 🔌 Conexion A TiDB (Como Se Configuro)

El backend usa `mysql2` con TLS para conectarse a TiDB Cloud.

1. En TiDB Cloud, crea credenciales SQL y copia host/usuario/password/base de datos/puerto.
2. En Render (servicio backend), configura:
    - `DATABASE_HOST`
    - `DATABASE_PORT=4000`
    - `DATABASE_USERNAME`
    - `DATABASE_PASSWORD`
    - `DATABASE_NAME`
3. TLS se aplica en [backend/src/config/db.js](backend/src/config/db.js) con:
    - `ssl: { rejectUnauthorized: true }`
4. Tambien se normaliza el host en [backend/src/config/db.js](backend/src/config/db.js) para evitar valores invalidos en variables (`http://...` o `/` al final).
5. En Render (backend), configura `CORS_ORIGIN=https://restaurant-frontend-xcuz.onrender.com`.
6. En Render (frontend), configura `VITE_API_BASE_URL=https://restaurant-reservation-system-2ki2.onrender.com/api`.
7. Haz redeploy de ambos servicios despues de actualizar variables.

---

## 📡 Endpoints De La API

| Method | Path | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/register` | — | Registrar nuevo usuario |
| POST | `/api/auth/login` | — | Iniciar sesion y recibir JWT |
| GET  | `/api/auth/me` | JWT | Obtener usuario actual |
| GET  | `/api/restaurants` | — | Listar restaurantes |
| GET  | `/api/restaurants/:id` | — | Obtener un restaurante |
| POST | `/api/restaurants` | Admin | Crear restaurante |
| PUT  | `/api/restaurants/:id` | Admin | Actualizar restaurante |
| DELETE | `/api/restaurants/:id` | Admin | Eliminar restaurante |
| GET  | `/api/reservations` | JWT | Listar reservas del usuario (admin ve todas) |
| POST | `/api/reservations` | JWT | Crear reserva |
| PUT  | `/api/reservations/:id` | JWT | Actualizar reserva |
| DELETE | `/api/reservations/:id` | JWT | Cancelar reserva |

---

## 🧪 Ejecutar Pruebas

```bash
# Backend
cd backend && npm test

# Frontend
cd frontend && npm test
```

---

## 🛠 Stack Tecnologico

| Layer | Technology |
|---|---|
| Frontend | React 19, React Router v7, Vite 8, Axios |
| Backend | Node.js 18+, Express 5, express-validator, JWT, bcryptjs |
| Database | TiDB Cloud (DB serverless compatible con MySQL) |
| DB Driver | mysql2 |
| Despliegue | Docker, Docker Compose, Render |
