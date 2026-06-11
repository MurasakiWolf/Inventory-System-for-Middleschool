# Sistema de Inventario Escolar — Documentación completa

## Descripción general

Aplicación de escritorio para gestión de inventario en una secundaria. Permite registrar, editar, dar de baja y consultar el historial de equipos y mobiliario escolar. Desarrollada con React + Express + SQLite, empaquetada con Electron.

---

## Herramientas y tecnologías utilizadas

### Frontend
| Herramienta | Versión | Uso |
|---|---|---|
| React | 19 | Interfaz de usuario |
| React Router DOM | 7 | Navegación entre páginas |
| Axios | 1.x | Peticiones HTTP al backend |
| jsPDF | 4.x | Generación de reportes PDF |
| XLSX (SheetJS) | 0.18 | Exportación a Excel |
| react-barcode | 1.6 | Generación de códigos de barras |
| react-hook-form | 7 | Manejo de formularios |

### Backend
| Herramienta | Versión | Uso |
|---|---|---|
| Node.js | 24.x | Entorno de ejecución |
| Express | 4.x | Servidor HTTP / API REST |
| better-sqlite3 | 12.x | Base de datos SQLite local |
| CORS | 2.x | Permitir peticiones del frontend |

### Empaquetado
| Herramienta | Versión | Uso |
|---|---|---|
| Electron | 31.x | Ventana de escritorio |
| electron-builder | 24.x | Generación del instalador `.exe` |

---

## Estructura del proyecto

```
inventory_system/
├── electron/
│   └── main.js              ← Proceso principal de Electron
├── BackEnd/
│   ├── server.js            ← API REST (Express)
│   ├── db.js                ← Conexión y tablas SQLite
│   └── package.json
├── FrontEnd/
│   ├── src/
│   │   ├── pages/           ← Pantallas principales
│   │   ├── components/      ← Componentes reutilizables
│   │   ├── services/api.js  ← Llamadas al backend
│   │   ├── context/         ← Auth y tema (claro/oscuro)
│   │   └── styles/          ← Archivos CSS
│   └── package.json
└── package.json             ← Configuración raíz (Electron)
```

---

## Instalación y uso

### Opción A — Instalador `.exe` (recomendado para el director)

1. Ejecutar `Sistema de Inventario Setup 1.0.0.exe`
2. Seguir el instalador (siguiente, siguiente, instalar)
3. Se crea acceso directo en el escritorio
4. Doble clic en el ícono — la app abre sola, sin terminales

> El backend, la base de datos y el frontend vienen incluidos en el instalador. No requiere instalar Node.js ni ningún otro programa.

---

### Opción B — Modo desarrollo (para modificar el código)

#### Requisitos previos
- Node.js instalado (https://nodejs.org)

#### Pasos

**1. Instalar dependencias del backend:**
```bash
cd inventory_system/BackEnd
npm install
```

**2. Instalar dependencias del frontend:**
```bash
cd inventory_system/FrontEnd
npm install
```

**3. Instalar dependencias raíz (Electron):**
```bash
cd inventory_system
npm install
```

**4. Arrancar el backend:**
```bash
cd inventory_system/BackEnd
node server.js
```

**5. Arrancar el frontend:**
```bash
cd inventory_system/FrontEnd
npm start
```

La app queda disponible en `http://localhost:3000`

---

### Generar nuevo instalador `.exe`

Desde la carpeta raíz `inventory_system/`:
```bash
npm run dist
```

El instalador se genera en `dist-electron/Sistema de Inventario Setup 1.0.0.exe`

---

## Base de datos

- Motor: SQLite (archivo `.db`)
- En modo desarrollo: `BackEnd/inventario.db`
- En modo Electron (instalado): se guarda en la carpeta de datos del usuario del sistema operativo, por ejemplo:
  ```
  C:\Users\<usuario>\AppData\Roaming\Sistema de Inventario\inventario.db
  ```
  Esto garantiza que los datos persisten aunque se actualice o reinstale la app.

### Tablas
| Tabla | Descripción |
|---|---|
| `items` | Equipos y mobiliario registrados |
| `historial` | Movimientos (alta, edición, baja, reparación) |
| `ubicaciones` | Catálogo de ubicaciones del plantel |

---

## Configuración de red local (director + secretaria)

El sistema permite que varias computadoras en la misma red local compartan una sola base de datos.

### Cómo funciona
- La computadora del **director** actúa como servidor
- La **secretaria** (u otras computadoras) se conectan a la IP del director por el navegador
- Todos ven y modifican el mismo inventario en tiempo real

### Pasos de configuración

#### En la computadora del director
1. Abrir la app normalmente
2. El botón **🌐 Servidor** debe tener `localhost:4000` (valor por defecto, no cambiar)

#### En la computadora de la secretaria
1. Abrir el navegador y escribir la IP del director, ejemplo:
   ```
   http://192.168.1.23:3000
   ```
2. Hacer clic en el botón **🌐 Servidor** en la barra superior
3. Cambiar el valor a la IP del director con puerto 4000, ejemplo:
   ```
   192.168.1.23:4000
   ```
4. Hacer clic en **Probar conexión** — debe aparecer ✅
5. Hacer clic en **Guardar**

> Esta configuración se guarda en el navegador. Solo se hace una vez.

### Cómo saber la IP del director
En la computadora del director, abrir PowerShell y ejecutar:
```
ipconfig
```
Buscar el adaptador **Wi-Fi** y tomar la **Dirección IPv4**, ejemplo: `192.168.1.23`

### Condiciones para que funcione
- Ambas computadoras deben estar conectadas a la **misma red Wi-Fi o ethernet** del colegio
- La computadora del director debe tener la app abierta

---

## Funcionalidades principales

- Registrar equipos y mobiliario (con foto opcional)
- Editar registros existentes
- Dar de baja con motivo (daño, pérdida, sustitución, etc.)
- Historial completo de movimientos por equipo
- Filtros por tipo (SEJ / Interno), ubicación, estado
- Agrupación por ubicación al ver todo el inventario
- Exportar reporte en PDF
- Exportar a Excel (hoja de activos + hoja de bajas)
- Modo claro / modo oscuro
- Acceso con contraseña

---

## Repositorio

GitHub: https://github.com/MurasakiWolf/inventory_system
