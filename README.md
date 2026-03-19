# ZONALYTIC — Ficha de Implantación

> Herramienta interna de Regislab para la documentación estructurada de visitas de implantación de sistemas RFID en plantas industriales.

---

## ¿Qué es ZONALYTIC?

ZONALYTIC es una aplicación web que guía al técnico de Regislab durante la visita de implantación, permitiendo registrar de forma ordenada toda la información necesaria para configurar un sistema de trazabilidad RFID:

- **Estructura de zonas** y jerarquía de la planta
- **Puntos de control RFID** generados automáticamente desde las zonas hoja
- **Contexto productivo** — clientes, productos, turnos y costes por zona
- **Objetivos del piloto** — problemas detectados, KPIs y métricas de éxito
- **Análisis inteligente** con IA — árbol visual de zonas, puntos críticos priorizados y compartición por WhatsApp
- **Exportación** en formato JSON estructurado + imagen del árbol de zonas

---

## Tecnología

- HTML5 + CSS3 + JavaScript vanilla — sin dependencias de framework
- [Groq API](https://console.groq.com) — modelo `llama-3.3-70b-versatile` para análisis IA
- [Cloudflare Workers](https://workers.cloudflare.com) — proxy API para gestión segura de credenciales
- [GitHub Pages](https://pages.github.com) — despliegue estático

---

## Arquitectura

```
GitHub Pages (HTML estático)
        │
        ▼
Cloudflare Worker "zonalytic-api"   ← gestiona la API key de forma segura
        │
        ▼
Groq API (llama-3.3-70b-versatile)
```

La API key nunca está expuesta en el código fuente del repositorio.

---

## Despliegue

### Requisitos
- Node.js instalado
- Cuenta en [Cloudflare](https://cloudflare.com)
- API key de [Groq](https://console.groq.com) (gratuita, sin tarjeta)

### Pasos

**1. Clonar el repositorio**
```bash
git clone https://github.com/regislabsc/zonalytic.git
cd zonalytic
```

**2. Instalar Wrangler**
```bash
npm install -g wrangler
wrangler login
```

**3. Configurar y desplegar el Worker proxy**

Edita `worker_groq.js` y añade tu API key de Groq:
```javascript
const GROQ_KEY = 'gsk_...'; // tu key aquí
```

Despliega:
```bash
wrangler deploy worker_groq.js --name zonalytic-api --compatibility-date 2024-01-01
```

**4. Desplegar el frontend**

Sube `index.html` a GitHub Pages desde la configuración del repositorio (**Settings → Pages → Deploy from branch**).

---

## Uso

1. Abre la URL de GitHub Pages
2. Completa los 6 pasos del formulario
3. En el paso **Resumen**, pulsa **✦ Analizar con IA** para generar el árbol de zonas y los puntos críticos
4. Exporta el JSON estructurado y/o la imagen del árbol para compartir con el equipo

---

## Estructura del JSON exportado

```json
{
  "empresa": "Cítricos AND. S.A.T. 3719",
  "centro": "Planta Almería",
  "fecha": "2026-03-19",
  "tecnico": "Nombre técnico",
  "zonas": [...],
  "puntos_control": [...],
  "contexto": [...],
  "objetivo": {...}
}
```

---

## Licencia y propiedad intelectual

```
Copyright © 2026 Regislab. Todos los derechos reservados.

Este software es propiedad exclusiva de Regislab.
Queda prohibida su reproducción, distribución, modificación o uso
comercial total o parcial sin autorización expresa y por escrito de Regislab.

El acceso público a este repositorio tiene carácter exclusivamente
informativo y de referencia técnica. No constituye cesión de ningún
derecho de uso, licencia ni autorización implícita sobre el código,
diseño o metodología contenidos en el mismo.

Para solicitudes de uso, colaboración o licenciamiento:
info@regislab.es
```

---

<div align="center">
  <sub>Desarrollado por <strong>Regislab</strong> · 2026</sub>
</div>
