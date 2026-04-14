# Proyecto Manglar

**Manglar** es el ecosistema de software que da soporte digital a las casas de convivencias y retiros o centros culturales: difusión de su labor, gestión de **centros culturales**, **actividades**, **reservas** y relación con la comunidad (información, contacto, donaciones y alianzas).


## Qué problema resuelve

- **Ciudadanía y participantes**: un canal claro para conocer centros y actividades, inscribirse o reservar donde aplique, y mantener un perfil y sus reservas cuando la plataforma lo permita.
- **Equipo operativo**: un panel para administrar actividades, centros, usuarios y el seguimiento de reservas, alineado con la misma API de negocio.

## Componentes del ecosistema

| Componente | Rol |
|------------|-----|
| **`manglar-ui`** | Sitio web público (visitantes y usuarios finales). |
| **`manglar-admin-ui`** | Panel de administración para personal autorizado. |
| **`manglar`** | Backend / API de dominio (servicios compartidos por los frontends). |
| **`manglar-infra`** | Infraestructura como código y despliegue (entornos, redes, etc.). |

Las aplicaciones web comparten una base tecnológica similar (React, Vite, Material UI, React Query, internacionalización) e integran autenticación con **AWS Cognito** de formas distintas según el perfil (público frente a administradores). Los detalles están en el documento de alcance.

## Documentación en este repositorio

| Documento | Contenido |
|-----------|-----------|
| [**PROJECT_SCOPE.md**](PROJECT_SCOPE.md) | Alcance funcional y técnico del ecosistema: módulos, autenticación, API y criterios de éxito por frente. |

## Licencia y uso

El contenido de documentación en este repositorio se publica para describir el alcance y la arquitectura de alto nivel del proyecto Manglar. Si añades una licencia específica (por ejemplo CC BY para textos), indícala aquí o en un archivo `LICENSE`.
