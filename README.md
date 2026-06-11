# Contexto del Proyecto — Sistema de Inventario Escolar

## ¿Qué es?

Sistema de inventario para una secundaria. El director y su secretaria pueden registrar, consultar y dar seguimiento a todos los equipos y mobiliario del plantel (computadoras, proyectores, mesas, sillas, etc.). La app corre como programa de escritorio en Windows, sin necesidad de internet.

---

## ¿Por qué existe?

El director necesitaba una herramienta propia para llevar el control del inventario escolar sin depender de hojas de Excel desorganizadas. El sistema permite tener un registro centralizado, con historial de movimientos, filtros, reportes y acceso compartido con la secretaria dentro de la red del colegio.

---

## ¿Cómo está construido?

Es una aplicación web (React + Express) empaquetada como app de escritorio con Electron. La base de datos es SQLite local — no requiere internet ni servidores externos.

- **Frontend:** React corriendo en el navegador integrado de Electron
- **Backend:** Express sirviendo una API REST en `localhost:4000`
- **Base de datos:** SQLite manejada con `better-sqlite3`
- **Escritorio:** Electron envuelve todo en un `.exe` instalable

---

## ¿Qué puede hacer la app?

### Inventario
- Registrar equipos y mobiliario con: nombre, descripción, tipo (SEJ o Interno), ubicación, estado y foto opcional
- Editar cualquier registro
- Dar de baja un equipo con motivo (daño, pérdida, sustitución, robo, etc.)
- Ver todos los equipos activos o todos los dados de baja
- Filtrar por tipo, ubicación, estado o buscar por texto
- Cuando el filtro es "Todas las ubicaciones", los equipos se agrupan visualmente por ubicación con un subtítulo

### Historial
- Ver todos los movimientos del inventario: altas, ediciones, bajas, reparaciones
- Filtrar por tipo de movimiento
- Hacer clic en un movimiento para ver el detalle del equipo

### Exportación
- Generar reporte en PDF con listado de activos y bajas
- Exportar a Excel con dos hojas: activos y bajas

### Ubicaciones
- Catálogo de ubicaciones del plantel (aulas, oficinas, laboratorio, etc.)
- Agregar, renombrar y eliminar ubicaciones desde la app
- No se puede eliminar una ubicación que tenga equipos activos asignados

### Usuarios y sesión
- Acceso protegido con contraseña
- Cierre de sesión

### Apariencia
- Modo oscuro y modo claro, conmutable desde el header

### Red local
- Botón "🌐 Servidor" en el header para configurar a qué servidor conectarse
- Permite que la secretaria use la app desde su propia computadora conectándose al backend del director por la red del colegio
- La configuración se guarda en el navegador (localStorage)

---

## ¿Cómo se usa en el colegio?

### Director
- Abre la app con doble clic en el ícono del escritorio
- La app arranca sola (backend + frontend integrados)
- Usa `localhost:4000` como servidor (por defecto)
- Puede usar la app en casa también — sus cambios se guardan localmente y cuando regresa al colegio la secretaria los ve automáticamente

### Secretaria
- Abre su navegador y entra a `http://[IP del director]:3000`
- La primera vez configura el servidor en el botón 🌐 con la IP del director
- A partir de ahí usa la app normalmente, viendo la misma base de datos que el director

---

## ¿Dónde viven los datos?

- En modo instalado (Electron): `C:\Users\[usuario]\AppData\Roaming\Sistema de Inventario\inventario.db`
- En modo desarrollo: `BackEnd/inventario.db`
- Los datos persisten aunque se actualice o reinstale la app

---

## Tipos de inventario

| Tipo | Descripción |
|---|---|
| SEJ | Bienes asignados por la Secretaría de Educación de Jalisco |
| Interno | Bienes adquiridos por la propia escuela |

## Estados de un equipo

| Estado | Descripción |
|---|---|
| Nuevo | Recién dado de alta, sin uso |
| Usado | En uso normal |
| En reparación | Fuera de servicio temporalmente |
| Deteriorado | Con daño visible pero funcional |
| Baja | Dado de baja definitivamente |

---

## Flujo de vida de un equipo

```
Alta → Usado → En reparación → Usado (si se repara)
                             → Baja (si no se repara o se pierde)
     → Deteriorado → Baja
     → Baja directa (pérdida, robo, etc.)
```

---

## Archivos clave

| Archivo | Qué hace |
|---|---|
| `electron/main.js` | Arranca el backend y abre la ventana de Electron |
| `BackEnd/server.js` | Define todos los endpoints de la API |
| `BackEnd/db.js` | Crea las tablas y conecta SQLite |
| `FrontEnd/src/pages/InventoryPage.js` | Página principal del inventario |
| `FrontEnd/src/pages/HistoryPage.js` | Página del historial |
| `FrontEnd/src/services/api.js` | Todas las llamadas al backend |
| `FrontEnd/src/components/Header.js` | Barra superior con nav, tema y servidor |
| `FrontEnd/src/styles/global.css` | Variables de color para ambos temas |

---

## Repositorio

GitHub: https://github.com/MurasakiWolf/inventory_system
