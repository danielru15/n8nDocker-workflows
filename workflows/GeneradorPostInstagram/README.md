# 📸 Generador de Posts para Instagram - PROYEXAPP

Workflow automatizado de n8n para generar contenido completo de Instagram (copy, hashtags y carruseles de imágenes) utilizando inteligencia artificial.

## 📋 Descripción

Este workflow automatiza la creación de posts para Instagram orientados a marketing digital para startups y emprendimientos. Genera:

- **Copy optimizado** (150-250 palabras)
- **Hashtags estratégicos** (25-30 hashtags)
- **Carruseles de imágenes** (4-6 slides) generados con IA

## 🏗️ Arquitectura del Workflow

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Schedule        │────▶│ Create Folder   │────▶│ AI Agent        │
│ Trigger         │     │ (Google Drive)  │     │ (Gemini)        │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Organizar       │◀────│ Organizar       │◀────│ Transformar     │
│ Información     │     │ Prompt Diseño   │     │ Respuesta       │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │
        ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Append Row      │────▶│ Split Out       │────▶│ Loop Over       │
│ (Google Sheets) │     │                 │     │ Items           │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Traer Imagen    │◀────│ Crear Imagen    │◀────┤ (Por cada       │
│ Generada        │     │ (Nano Banana)   │     │  slide)         │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │
        ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Switch          │────▶│ Descargar       │────▶│ Upload File     │
│ (Estado)        │     │ Imagen          │     │ (Google Drive)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## 🔧 Componentes Principales

### 1. **Schedule Trigger**
Dispara el workflow automáticamente según el intervalo configurado.

### 2. **Create Folder (Google Drive)**
Crea una carpeta nueva en Google Drive llamada "PUBLICACION" dentro de `PUBLICACIONES_TIKTOK_IG` para almacenar las imágenes generadas.

### 3. **AI Agent (Google Gemini)**
Genera todo el contenido utilizando un prompt especializado que incluye:
- Contexto de marca PROYEXAPP
- Paleta de colores corporativa (#0E2D5F, #CED7E4, #F3F3F3)
- Especificaciones de diseño detalladas
- Estructura del carrusel (portada, contenido, CTA)

### 4. **Transformar Respuesta de Gemini**
Parsea la respuesta del AI extrayendo:
- `COPY_INSTAGRAM`
- `HASHTAGS_INSTAGRAM`
- `DISEÑO_CARRUSEL_JSON`

### 5. **Append Row (Google Sheets)**
Guarda el contenido generado en una hoja de cálculo con columnas:
- COPY
- HASHTAGS
- CARROUSEL PROMPT

### 6. **Generación de Imágenes (Nano Banana)**
Utiliza la API de [kie.ai](https://kie.ai/) con el modelo `nano-banana-pro` para generar cada slide del carrusel.

### 7. **Sistema de Polling**
- Verifica el estado de generación (success, waiting, generating, fail)
- Espera automáticamente si la imagen está procesándose
- Maneja errores con mensajes descriptivos

### 8. **Upload File (Google Drive)**
Sube cada imagen generada a la carpeta creada al inicio del workflow.

## 📦 Dependencias y Credenciales

| Servicio | Tipo de Credencial | Uso |
|----------|-------------------|-----|
| Google Gemini | API Key (PaLM) | Generación de contenido |
| Google Sheets | OAuth2 | Almacenamiento de posts |
| Google Drive | OAuth2 | Almacenamiento de imágenes |
| kie.ai (Nano Banana) | Header Auth | Generación de imágenes |

## 🎨 Identidad de Marca

El workflow está configurado para la marca **PROYEXAPP** con:

| Elemento | Color |
|----------|-------|
| Azul oscuro principal | `#0E2D5F` |
| Azul claro (acentos) | `#CED7E4` |
| Gris muy claro (fondos) | `#F3F3F3` |

## 📝 Temas Disponibles

El AI puede generar contenido sobre:

1. Errores fatales al lanzar una web/app
2. Señales de que tu startup necesita digitalización
3. Paso a paso para validar tu idea sin gastar miles
4. ROI real de invertir en tecnología
5. Mitos vs Realidad del desarrollo web
6. Cómo elegir entre web o app para tu startup
7. El proceso real de crear un MVP exitoso
8. De la idea al lanzamiento: timeline realista
9. El costo real de NO digitalizarse
10. Cómo escalar tu startup sin aumentar costos
11. Tecnologías que toda startup debe implementar en 2025

## 🚀 Configuración

### Requisitos Previos

1. Cuenta de Google Cloud con APIs habilitadas:
   - Google Sheets API
   - Google Drive API
   - Generative Language API (Gemini)

2. Cuenta en [kie.ai](https://kie.ai/) con créditos para Nano Banana

3. Instancia de n8n funcionando

### Pasos de Configuración

1. **Importar el workflow** en n8n
2. **Configurar credenciales**:
   - Google Gemini (PaLM) API
   - Google Sheets OAuth2
   - Google Drive OAuth2
   - Header Auth para kie.ai (API Key)
3. **Ajustar IDs de carpeta y hoja de cálculo** según tu configuración
4. **Activar el workflow**

## 📊 Salida del Workflow

Por cada ejecución, el workflow genera:

```
📁 PUBLICACIONES_TIKTOK_IG/
└── 📁 PUBLICACION/
    ├── 🖼️ 1.png (Portada)
    ├── 🖼️ 2.png (Contenido)
    ├── 🖼️ 3.png (Contenido)
    ├── 🖼️ 4.png (Contenido)
    └── 🖼️ 5.png (CTA)

📊 Google Sheets:
└── COPY | HASHTAGS | CARROUSEL PROMPT
```

## ⚠️ Notas Importantes

- El modelo Nano Banana puede tardar varios segundos en generar cada imagen
- El workflow incluye un sistema de espera automática (Wait) para manejar tiempos de procesamiento
- Se recomienda no ejecutar el workflow muy frecuentemente para evitar costos excesivos en la API de generación de imágenes

## 📄 Licencia

Este workflow es parte del proyecto n8n-compose de PROYEXAPP.

---

**Desarrollado para PROYEXAPP** - Desarrollo web y aplicaciones móviles para founders, emprendedores y dueños de negocios.

