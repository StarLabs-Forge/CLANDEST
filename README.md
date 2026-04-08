# 🕶️ CLANDEST

> Plataforma de descubrimiento de eventos, fiestas y conciertos — La Paz, Bolivia.

CLANDEST conecta a quienes buscan experiencias con quienes las crean. Desde eventos underground hasta conciertos locales, todo en un solo lugar.

---

## 📌 Índice

- [Descripción](#descripción)
- [Propuesta de valor](#propuesta-de-valor)
- [Funcionalidades principales](#funcionalidades-principales)
- [Tipos de usuarios](#tipos-de-usuarios)
- [Arquitectura del sistema](#arquitectura-del-sistema)
- [Modelo de datos](#modelo-de-datos)
- [Módulos del sistema](#módulos-del-sistema)
- [Alcance del MVP](#alcance-del-mvp)
- [Fases de desarrollo](#fases-de-desarrollo)
- [Modelo de negocio](#modelo-de-negocio)
- [Escalabilidad futura](#escalabilidad-futura)
- [Stack tecnológico](#stack-tecnológico)
- [Estado del proyecto](#estado-del-proyecto)

---

## Descripción

**CLANDEST** es una aplicación móvil orientada al descubrimiento de eventos sociales, fiestas y conciertos, con énfasis en experiencias exclusivas, locales y underground.

La plataforma actúa como puente entre organizadores (locales, DJs, productoras) y usuarios interesados en asistir a eventos, facilitando visibilidad, difusión y acceso a información en tiempo real. El MVP arranca en **La Paz, Bolivia**.

---

## Propuesta de valor

| Para asistentes | Para organizadores |
|---|---|
| Descubrir eventos que no aparecen en plataformas tradicionales | Publicar y gestionar eventos de forma directa y sin fricción |
| Filtrar por fecha, tipo, ubicación y precio | Acceder a métricas básicas (visualizaciones, guardados) |
| Guardar y compartir eventos fácilmente | Alcanzar a su audiencia objetivo sin intermediarios costosos |

---

## Funcionalidades principales

- 🔍 Descubrimiento de eventos en tiempo real
- 📍 Visualización de eventos cercanos con integración a mapas
- 📅 Filtros por fecha, tipo, ubicación y precio
- ❤️ Guardado de eventos favoritos
- 📤 Compartir eventos en redes sociales
- 🔔 Notificaciones push (alertas, recordatorios, nuevos eventos relevantes)
- 🎟️ Publicación y gestión de eventos por parte de organizadores

---

## Tipos de usuarios

### 👤 Usuario (Asistente)
- Explorar y descubrir eventos
- Filtrar por fecha, tipo y ubicación
- Ver el detalle completo de un evento
- Guardar eventos en favoritos
- Compartir eventos

### 🎛️ Organizador / Local
- Crear, editar y eliminar eventos
- Gestionar la información del evento (descripción, imagen, precio, ubicación)
- Consultar métricas básicas: visualizaciones y guardados

### 🛡️ Administrador
- Moderar contenido
- Gestionar usuarios
- Eliminar eventos inapropiados

---

## Arquitectura del sistema

```
┌─────────────────────────────────────────────────────────┐
│                    Aplicación Móvil                     │
│                  Flutter (iOS / Android)                │
└───────────────────────┬─────────────────────────────────┘
                        │ HTTP / REST
┌───────────────────────▼─────────────────────────────────┐
│                      API REST                           │
│                  Node.js + Express                      │
└──────────┬────────────────────────────┬─────────────────┘
           │                            │
┌──────────▼──────────┐    ┌────────────▼────────────────┐
│      Supabase       │    │      Servicios externos      │
│  Base de datos      │    │  • Google Maps API           │
│  Auth               │    │  • Push Notifications        │
│  Storage (imágenes) │    │    (FCM / APNs)              │
└─────────────────────┘    └─────────────────────────────┘
```

---

## Modelo de datos

### Usuario
```
id          : uuid (PK)
nombre      : string
correo      : string (unique)
rol         : enum [asistente, organizador, admin]
avatar_url  : string?
created_at  : timestamp
```

### Evento
```
id             : uuid (PK)
titulo         : string
descripcion    : text
fecha_inicio   : timestamp
fecha_fin      : timestamp?
ubicacion      : string
lat            : float
lng            : float
precio         : decimal?
imagen_url     : string?
tipo           : enum [fiesta, concierto, festival, underground, otro]
organizador_id : uuid (FK → Usuario)
created_at     : timestamp
```

### Favorito
```
id         : uuid (PK)
usuario_id : uuid (FK → Usuario)
evento_id  : uuid (FK → Evento)
created_at : timestamp
```

---

## Módulos del sistema

| Módulo | Descripción |
|---|---|
| **Autenticación** | Registro, login, gestión de credenciales, control de acceso por rol |
| **Gestión de usuarios** | Perfil, preferencias, historial y favoritos |
| **Gestión de eventos** | CRUD de eventos, detalle completo, visualización |
| **Búsqueda y filtrado** | Búsqueda por nombre, filtros por fecha / tipo / ubicación / precio |
| **Geolocalización** | Eventos cercanos, integración con Google Maps |
| **Notificaciones** | Alertas de eventos, recordatorios, nuevos eventos relevantes |
| **Administración** | Moderación de contenido, gestión de usuarios |

---

## Alcance del MVP

### ✅ Incluido
- Registro e inicio de sesión (asistente y organizador)
- Visualización de listado y detalle de eventos
- Publicación de eventos por organizadores
- Búsqueda y filtros básicos (fecha, tipo, ubicación)
- Guardado de favoritos

### ❌ Excluido en esta versión
- Sistema de pagos
- Ticketing integrado
- Análisis avanzado para organizadores
- Recomendaciones inteligentes
- Eventos con acceso restringido / privados

---

## Fases de desarrollo

El proyecto se estructura en cuatro fases progresivas, cada una construida sobre la anterior.

---

### 🔵 Fase 0 — Concepto y diseño *(actual)*
**Duración estimada: 3–4 semanas**

Definición del producto, validación de la idea y diseño de la experiencia antes de escribir una sola línea de código.

- [ ] Definición completa de requerimientos y flujos de usuario
- [ ] Wireframes de las pantallas principales
- [ ] Diseño UI en Figma (sistema de diseño, paleta, tipografía)
- [ ] Prototipo navegable (clickable prototype)
- [ ] Validación con primeros usuarios y organizadores en La Paz
- [ ] Definición del esquema de base de datos en Supabase
- [ ] Documentación técnica inicial (este README)

---

### 🟡 Fase 1 — MVP
**Duración estimada: 8–12 semanas**

Producto funcional mínimo para salir al mercado en La Paz y validar el modelo.

**Auth & usuarios**
- [ ] Registro e inicio de sesión con email/contraseña
- [ ] Roles: asistente y organizador
- [ ] Perfil de usuario editable

**Eventos**
- [ ] Listado de eventos con tarjetas
- [ ] Detalle completo del evento
- [ ] Creación y edición de eventos (organizadores)
- [ ] Subida de imagen de portada

**Búsqueda y filtros**
- [ ] Búsqueda por nombre
- [ ] Filtros: fecha, tipo, precio

**Extras básicos**
- [ ] Guardado de favoritos
- [ ] Compartir eventos (link + redes sociales)
- [ ] Geolocalización básica (ver eventos cercanos)
- [ ] Notificaciones push (recordatorios de eventos guardados)

**Infraestructura**
- [ ] API REST con Node.js + Express
- [ ] Supabase configurado (auth, DB, storage)
- [ ] Despliegue inicial del backend
- [ ] Build de la app en Flutter para pruebas internas (TestFlight / Firebase Distribution)

---

### 🟠 Fase 2 — Crecimiento
**Duración estimada: 6–10 semanas post-lanzamiento**

Mejoras basadas en feedback real de usuarios y primeras métricas de uso.

- [ ] Dashboard de métricas para organizadores (visualizaciones, guardados, alcance)
- [ ] Sistema de categorías y etiquetas ampliado
- [ ] Mapa interactivo de eventos (vista mapa completa)
- [ ] Mejoras en notificaciones (alertas por proximidad, nuevos eventos del tipo preferido)
- [ ] Moderación mejorada para administradores
- [ ] Onboarding para nuevos organizadores
- [ ] Plan de suscripción para organizadores (integración de pagos)
- [ ] Eventos destacados / boost pagado
- [ ] Patrocinios y alianzas con marcas locales

---

### 🔴 Fase 3 — Escala
**Duración estimada: a definir según tracción**

Expansión del producto y del mercado.

- [ ] Ticketing integrado con pasarela de pago local (QR, tarjeta)
- [ ] Comisión por venta de ticket procesada en plataforma
- [ ] Recomendaciones inteligentes basadas en historial del usuario
- [ ] Eventos privados con código de acceso o lista
- [ ] Sistema de reputación y verificación para organizadores
- [ ] Expansión a Santa Cruz y Cochabamba
- [ ] Versión web (PWA o sitio dedicado)
- [ ] API pública para integraciones de terceros

---

## Modelo de negocio

CLANDEST opera bajo un modelo **B2B2C**: genera valor para los asistentes (consumidores finales) a través de los organizadores (clientes de pago). La plataforma es gratuita para quienes buscan eventos; los ingresos provienen de quienes los publican y promocionan.

---

### 💰 Fuentes de ingreso

#### 1. Suscripción para organizadores *(Fase 1 → Fase 2)*

El acceso básico es gratuito con límites. Los organizadores que necesiten mayor capacidad y visibilidad acceden a un plan mensual.

| Plan | Precio estimado | Incluye |
|---|---|---|
| **Free** | Bs. 0 / mes | Hasta 2 eventos activos, visibilidad estándar |
| **Pro** | Bs. 99 / mes | Eventos ilimitados, métricas detalladas, soporte prioritario |
| **Boost** | Bs. 49 / evento | Evento destacado en feed principal por 7 días |

> Los precios son referenciales para el mercado paceño y deben validarse con organizadores reales antes del lanzamiento.

#### 2. Eventos destacados / Boost *(Fase 2)*

Los organizadores pueden pagar para que su evento aparezca primero en el feed o en la pantalla de inicio con una etiqueta visual diferenciada. Permite ingresos puntuales sin requerir suscripción.

#### 3. Comisión por ticket *(Fase 3)*

Una vez integrado el ticketing, CLANDEST cobra una comisión del 5–10% por cada ticket vendido. El organizador recibe el resto directamente. Este modelo escala de forma proporcional al volumen de eventos y asistentes.

#### 4. Patrocinios y alianzas locales *(Fase 2–3)*

Marcas de bebidas, ropa, audio o entretenimiento pueden patrocinar categorías de eventos, noches temáticas o notificaciones segmentadas. Especialmente relevante para el mercado nocturno y cultural paceño.

---

### 📊 Métricas clave (KPIs)

| Métrica | Qué mide |
|---|---|
| Eventos activos por semana | Salud del supply de contenido |
| Usuarios activos mensuales (MAU) | Adopción general de la plataforma |
| Tasa de conversión Free → Pro | Viabilidad del modelo de suscripción |
| Eventos guardados por usuario | Engagement y relevancia del contenido |
| Tiempo en app por sesión | Profundidad de uso |
| Tickets vendidos por evento | Volumen de transacciones (Fase 3) |

---

### 🎯 Segmento objetivo

**Usuarios (asistentes)**
Personas entre 18 y 35 años en La Paz que consumen cultura, música en vivo, fiestas y experiencias sociales. Activos en redes sociales y con acceso fragmentado a información sobre eventos locales.

**Organizadores (clientes de pago)**
DJs, productoras, bares, discotecas, colectivos culturales y organizadores independientes que hoy dependen de Instagram o WhatsApp para difundir sus eventos, sin herramientas propias de gestión ni métricas de impacto.

---

### 🏆 Ventaja competitiva

No existe hoy en Bolivia una plataforma móvil dedicada exclusivamente al descubrimiento de eventos locales y underground. Las alternativas actuales presentan limitaciones claras:

| Alternativa | Limitación |
|---|---|
| Instagram / Facebook | No filtra por eventos, no tiene geolocalización ni historial |
| WhatsApp / grupos | Información dispersa, sin estructura, difícil de descubrir |
| Eventbrite | Orientado a eventos grandes y corporativos, sin foco local |
| Boca a boca | No escala |

CLANDEST ocupa ese espacio vacío: **descubrimiento local, enfocado, con identidad propia**.

---

### 📈 Proyección de crecimiento (referencial)

| Período | Objetivo |
|---|---|
| Mes 1–2 (lanzamiento) | 10–20 organizadores activos en La Paz, 500 usuarios registrados |
| Mes 3–6 | 50+ organizadores, 2.000 MAU, primeros ingresos por suscripción Pro |
| Mes 7–12 | 100+ organizadores, 5.000 MAU, lanzamiento de Boost, exploración de otras ciudades |
| Año 2 | Ticketing integrado, expansión a Santa Cruz / Cochabamba, 15.000+ MAU |

---

## Escalabilidad futura

- 🎟️ Sistema de tickets y pagos integrado
- 🤖 Recomendaciones personalizadas por IA
- 🔒 Eventos privados con acceso restringido
- ⭐ Sistema de reputación y verificación para organizadores
- 📊 Dashboard avanzado de métricas para organizadores
- 🌎 Expansión a otras ciudades de Bolivia y Latinoamérica
- 🌐 Versión web / PWA
- 🔗 API pública para integraciones de terceros

---

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| Mobile | Flutter (iOS + Android) |
| Backend | Node.js + Express |
| Base de datos | Supabase (PostgreSQL) |
| Auth | Supabase Auth |
| Storage | Supabase Storage |
| Mapas | Google Maps API |
| Notificaciones | Firebase Cloud Messaging (FCM) |

---

## Estado del proyecto

🔵 **Fase 0 — Concepto / ideación**

El proyecto se encuentra en etapa de definición. Este documento representa la visión inicial del sistema y sirve como base para el diseño, prototipado y desarrollo del MVP.

**Próximo paso:** Wireframes y validación con organizadores de La Paz.

---

> Hecho con 🖤 en La Paz — CLANDEST, donde pasan las cosas que no se anuncian en todos lados.
