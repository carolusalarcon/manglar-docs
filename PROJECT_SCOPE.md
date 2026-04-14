# Alcance del ecosistema Manglar

Este documento describe el **alcance funcional y técnico** de los dos frontends que conforman la plataforma **Manglar**: la aplicación **pública** y la aplicación para **administradores**. Comparten filosofía de arquitectura (React, Vite, MUI, React Query, i18n) y el mismo backend conceptual (`/api/manglar` por defecto), pero difieren en **audiencia**, **flujo de autenticación** y **capacidades**.

---

## 1. Visión general

- **Objetivo conjunto**: ofrecer información y servicios (centros, actividades, reservas, contacto, donaciones) y, en paralelo, permitir que el personal autorizado **gestione** centros, actividades, usuarios y reservas desde un panel dedicado.
- **Separación de productos**: el sitio público no sustituye al admin; son **dos aplicaciones desplegables de forma independiente**.

---

## 2. Aplicación pública — `manglar-ui`

### 2.1 Propósito

Interfaz **orientada al visitante** y al usuario final: contenido institucional, exploración de centros culturales públicos, consulta de actividades, formularios de contacto/reserva y área de usuario (perfil y reservas) cuando hay sesión.

### 2.2 Alcance funcional (páginas y flujos)

| Área | Descripción |
|------|-------------|
| **Inicio, Nosotros, Alianzas, Donaciones, Contacto** | Contenido informativo y formulario de contacto. |
| **Centros** | Listado de centros **públicos** desde API; vista detalle al seleccionar un centro. |
| **Actividades** | Listado/calendario de actividades en centros públicos; **formulario de reserva** asociado a una actividad. |
| **Autenticación** | Registro, confirmación de correo, inicio y cierre de sesión integrados en la app. |
| **Reservas y perfil** | Disponibles solo con sesión; el acceso sin autenticación redirige al login. |
| **Notificaciones de reservas** | Envío de emails de confirmación y cancelación, y mensajes de WhatsApp donde el usuario puede responder para confirmar asistencia o cancelar. |

### 2.3 Experiencia transversal

- Internacionalización (ES/EN), tema claro/oscuro, layout con barra lateral y cabecera responsive.
- **Navegación principal** basada en estado (`activePage`), no en rutas URL para la mayoría de las secciones (la app monta `Router` pero el menú impulsa vistas por estado).

---

## 3. Aplicación administradores — `manglar-admin-ui`

### 3.1 Propósito

**Dashboard de backoffice** para personal interno: supervisar el sistema, gestionar **actividades**, **centros de conferencias**, **usuarios** y **reservas** (vista detalle por actividad), con un **calendario** operativo.

### 3.2 Alcance funcional (módulos)

| Módulo | Descripción |
|--------|-------------|
| **Dashboard** | Vista principal del panel (resumen / métricas según implementación). |
| **Actividades** | Listado; creación y edición mediante diálogo con **formulario de actividad** (`ActivityForm`). |
| **Actividad (detalle)** | Ruta `/activities/:id`: ficha de actividad y gestión de **reservas** (`BookingList`). |
| **Calendario** | Vista de calendario (FullCalendar) para la operación diaria. |
| **Usuarios** | Administración de usuarios (listado y formulario). |
| **Centros de conferencias** | CRUD / gestión de centros respecto al API. |

---

## 4. Relación entre ambos proyectos

| Aspecto | Público (`manglar-ui`) | Admin (`manglar-admin-ui`) |
|---------|------------------------|----------------------------|
| **Usuario típico** | Visitante, estudiante/familia, donante | Staff operativo / backoffice |
| **Auth** | Amplify (SDK en app) | OIDC + Hosted UI (navegación completa) |
| **Gestión de datos** | Lectura y acciones de usuario (reservas propias) | Creación/edición/listados completos |
| **Exportación / informes** | Según páginas (p. ej. jsPDF, xlsx donde aplique) | Mismo tipo de herramientas en dependencias |

Ambos consumen el **mismo modelo de negocio** en backend (centros, actividades, reservas, usuarios) desde el punto de vista de producto; las políticas de quién puede crear o aprobar qué se aplican en **API y Cognito (grupos/roles)**, no solo en el frontend.

---

## 5. Stack tecnológico compartido (referencia)

- **React 18**, **TypeScript**, **Vite**, **MUI**, **TanStack React Query**, **react-i18next**, **FullCalendar** (actividades/calendario), **jsPDF / jspdf-autotable**, **xlsx**, **ESLint / Prettier / Husky**.

---

## 6. Criterios de éxito por frente

**Público**

- Navegación clara en todas las secciones informativas; centros y actividades alineados con datos reales del API.
- Registro, login y flujos de reserva estables en entornos de prueba y producción.
- Notificaciones de reservas operativas por email y WhatsApp, con procesamiento correcto de respuestas para confirmar asistencia o cancelar.
- Despliegue con `VITE_API_BASE_URL` y configuración Amplify correctas.

**Administradores**

- Solo usuarios autenticados acceden al panel; callback OAuth operativo.
- Operaciones CRUD de actividades, centros y usuarios coherentes con el API; reservas gestionables desde el detalle de actividad.
- Despliegue con `VITE_API_URL` y variables `VITE_COGNITO_*` del entorno correspondiente.

---

