
<img width="1125" height="391" alt="image" src="https://github.com/user-attachments/assets/9fb55636-826a-4926-a377-787feac1f33e" />

# Arquitectura trabajada

> [!NOTE]
> Se trabajó una arquitectura en la que la información de MongoDB Atlas se llevó a BigQuery para disponer de una tabla analítica; posteriormente, esa data fue consumida desde Power BI para construir el reporte; y, finalmente, se revisó la integración con Azure + Power BI Embed para mostrar el reporte dentro de la aplicación con seguridad por colegio mediante RLS.

---

## Tabla de contenido

- [MongoDB Atlas](#mongodb-atlas)
- [Datastream](#datastream)
- [Dataflow](#dataflow)
- [BigQuery](#bigquery)
- [Power BI](#power-bi)
- [Azure / Microsoft Entra ID](#azure--microsoft-entra-id)
- [Node.js / Backend](#nodejs--backend)
- [React / Frontend](#react--frontend)
- [Seguridad por roles](#seguridad-por-roles)
- [Flujo de autenticación y embed trabajado](#flujo-de-autenticación-y-embed-trabajado)

---

## MongoDB Atlas

Se usó como fuente original de datos.

### Lo que trabajamos

- identificar la información de `admissions`
- tomar esa data como origen del flujo analítico

### Rol dentro de la arquitectura

| Rol |
|------|
| sistema transaccional |
| fuente primaria de datos |

---

## Datastream

Se consideró dentro del flujo entre MongoDB y BigQuery.

### Lo que trabajamos

- ubicarlo como componente de replicación/captura de cambios desde MongoDB hacia la capa analítica

### Rol dentro de la arquitectura

| Rol |
|------|
| mover cambios desde MongoDB al pipeline analítico |

---

## Dataflow

Se consideró como parte del paso de datos hacia BigQuery.

### Lo que trabajamos

- ubicarlo como componente del flujo técnico para cargar la información en BigQuery

### Rol dentro de la arquitectura

| Rol |
|------|
| procesamiento / movimiento de datos |

---

## BigQuery

Se usó como capa analítica.

### Lo que trabajamos

- crear y usar la tabla
- validar que la data esté lista para reporting
- revisar campos

### Rol dentro de la arquitectura

| Rol |
|------|
| almacenamiento analítico |
| base para reporting |

---

## Power BI

Se usó como capa de visualización.

### Lo que trabajamos

- consumir la data preparada para el reporte
- construir visuales

### Rol dentro de la arquitectura

| Rol |
|------|
| visualización |
| publicación de reportes |

---

## Azure / Microsoft Entra ID

Se trabajó para la autenticación de la aplicación con Power BI.

### Lo que trabajamos

- definir configuración con variables:
  - `POWERBI_TENANT_ID`
  - `POWERBI_CLIENT_ID`
  - `POWERBI_CLIENT_SECRET`
- usar el endpoint de token con el tenant
- usar scope de Power BI:
  - `https://analysis.windows.net/powerbi/api/.default`

### Rol dentro de la arquitectura

| Rol |
|------|
| autenticación de la app contra Microsoft |
| obtención del access token para llamar la API de Power BI |

---

## Node.js / Backend

Se usó para la integración con Power BI Embed.

### Lo que trabajamos

- revisar el código backend para:
  - obtener access token desde Azure
  - generar embed token
  - usar variables:
    - `WORKSPACE_ID`
    - `REPORT_ID`
    - `DATASET_ID`
    - `RLS_ROLE`
  - dejar configurado el rol por defecto:
    - `SchoolRole`

### Rol dentro de la arquitectura

| Rol |
|------|
| backend de integración con Power BI |
| generación de tokens |
| envío del contexto de seguridad al reporte |

---

## React / Frontend

Se usó como capa de presentación.

### Lo que trabajamos

- considerar el consumo del reporte embebido desde la aplicación
- mostrar el reporte dentro de la app según el usuario

### Rol dentro de la arquitectura

| Rol |
|------|
| interfaz de usuario |
| contenedor del reporte embebido |

---

## Seguridad por roles

Se trabajó el enfoque de RLS (*Row-Level Security*) en Power BI.

### Lo que hicimos

- definir el uso del rol:
  - `SchoolRole`
- plantear el filtro por:
  - `school_id`
- hacer que el backend pase el rol y la identidad efectiva al momento de generar el embed token

### Resultado esperado

> [!IMPORTANT]
> - cada colegio entra al mismo reporte  
> - el rol filtra por `school_id`  
> - cada colegio ve solo su propia información

---

## Flujo de autenticación y embed trabajado

El flujo quedó planteado de la siguiente manera:

### 1. Backend en Node.js

La aplicación backend en Node.js usa:

- `POWERBI_TENANT_ID`
- `POWERBI_CLIENT_ID`
- `POWERBI_CLIENT_SECRET`

Con esta configuración, solicita un access token a Azure / Microsoft Entra ID.

### 2. Generación del embed token

Luego, llama a la API de Power BI para generar el embed token.

En esa generación se envía:

- el `REPORT_ID`
- el `DATASET_ID`
- el rol `SchoolRole`
- el identificador del colegio (`school_id`) como contexto de seguridad

### 3. Consumo desde frontend

Finalmente, el frontend en React recibe el token y embebe el reporte.

### 4. Aplicación de seguridad

Power BI aplica RLS y muestra únicamente la data correspondiente al colegio.

---

## Resumen visual de componentes

| Componente | Función dentro de la arquitectura |
|-----------|-----------------------------------|
| MongoDB Atlas | fuente original de datos |
| Datastream | replicación / captura de cambios |
| Dataflow | procesamiento / movimiento de datos |
| BigQuery | almacenamiento analítico |
| Power BI | visualización y publicación |
| Azure / Microsoft Entra ID | autenticación |
| Node.js / Backend | integración, tokens y seguridad |
| React / Frontend | presentación del reporte |
| RLS | filtrado por colegio mediante `school_id` |

---
