# 🚗 Tienda de Vehículos - Proyecto Cloud

Aplicación web para la gestión de vehículos (CRUD) desarrollada bajo una arquitectura de tres capas y desplegada en AWS utilizando Docker y Docker Compose.

---

## 🏗️ Arquitectura

El proyecto está compuesto por los siguientes servicios:

```text
             [ Usuario / Navegador ]
                      │
                      ▼
            [ Frontend - Nginx ]
                      │
                      ▼
          [ Backend - Node.js / Express ]
                      │
                      ▼
               [ Amazon RDS - MySQL ]
```

---

## 📂 Estructura del Proyecto

```text
.
├── docker-compose.yml          # Orquestación de los servicios
├── tienda-vehiculos-frontend/  # Frontend (HTML, CSS, JavaScript, Nginx)
├── tienda-vehiculos-backend/   # API REST desarrollada en Node.js y Express
└── tienda-vehiculos-db/        # Scripts SQL para inicializar la base de datos
```

---

## 🚀 Despliegue

### 1. Requisitos Previos

Antes de ejecutar la aplicación, asegúrate de contar con lo siguiente:

- Instancia Amazon EC2 (Amazon Linux 2023).
- Docker instalado.
- Docker Compose instalado.
- Base de datos MySQL en Amazon RDS.
- Grupo de seguridad configurado para permitir las conexiones necesarias.

---

### 2. Configuración Inicial

#### Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto con la siguiente configuración:

```env
DB_HOST=tu-endpoint-rds.amazonaws.com
DB_USER=alumno
DB_PASSWORD=alumno123
DB_NAME=tienda_vehiculos
DB_PORT=3306
```

#### Inicializar la Base de Datos

Importa el script SQL en tu instancia de Amazon RDS:

```bash
mysql -h <TU_ENDPOINT_RDS> -u admin -p < tienda-vehiculos-db/init.sql
```

---

### 3. Ejecución

Desde la carpeta raíz del proyecto ejecuta:

```bash
# Construir las imágenes
sudo docker compose build

# Levantar los contenedores
sudo docker compose up -d
```

Para verificar que todos los contenedores estén ejecutándose:

```bash
sudo docker ps
```

---

## 🛠️ Solución de Problemas (Troubleshooting)

| Problema | Causa probable | Solución |
|----------|----------------|----------|
| **ERR_CONNECTION_REFUSED** | Los puertos requeridos no están habilitados en el Security Group de la instancia EC2. | Verifica que el **Security Group** permita tráfico TCP en los puertos **80** (Frontend) y **3001** (Backend). |
| **La tabla no carga información** | El frontend está apuntando a una dirección IP incorrecta del backend. | Comprueba que el archivo **app.js** del frontend utilice la **IP pública correcta** de la instancia EC2. |
| **Error de conexión con la base de datos** | Endpoint de Amazon RDS o credenciales incorrectas. | Revisa el archivo **.env**, verifica el endpoint de RDS y comprueba la conectividad ejecutando: `telnet <RDS_ENDPOINT> 3306`. |

---

## 📜 Licencia

Proyecto desarrollado con fines académicos para el laboratorio de la asignatura **Ingeniería en Infraestructura Tecnológica**.
