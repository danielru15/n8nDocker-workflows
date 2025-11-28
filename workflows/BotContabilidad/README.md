# 🤖 Camila Bot - Asistente de Contabilidad Personal

Bot de Telegram inteligente para gestionar finanzas personales en Colombia, con capacidad de procesar facturas por imagen, voz y texto.

## 📋 Descripción

Camila Bot es un asistente financiero automatizado que facilita el registro y consulta de gastos e ingresos personales. Utiliza inteligencia artificial para:
- Extraer información de facturas desde imágenes
- Transcribir y procesar notas de voz
- Responder consultas en lenguaje natural
- Almacenar datos estructurados en PostgreSQL
- Respaldar facturas en Google Drive

## ✨ Características Principales

### 🖼️ Procesamiento de Imágenes
- **OCR Inteligente**: Analiza facturas y recibos usando Google Gemini 2.5 Flash
- **Extracción Automática**: Captura proveedor, fecha, subtotal, IVA, total, categoría
- **Respaldo en la Nube**: Guarda automáticamente las imágenes en Google Drive
- **Validación Colombiana**: Calcula correctamente IVA del 19% según normativa local

### 🎤 Comandos por Voz
- **Transcripción Automática**: Convierte notas de voz a texto con Gemini
- **Respuestas de Audio**: Genera respuestas habladas con ElevenLabs (voz "danielRamirez")
- **Procesamiento Natural**: Interpreta comandos como "ingresar 50000" o "gastos de este mes"

### 💬 Interacción por Texto
- **IA Conversacional**: Entiende lenguaje natural en español
- **Memoria de Contexto**: Recuerda conversaciones previas por usuario
- **Comandos Inteligentes**: Detecta intenciones automáticamente

## 🎯 Funcionalidades

### 1️⃣ Registro de Gastos (por imagen)
```
Usuario: [Envía foto de factura]
Bot: ✅ Has añadido un gasto con los siguientes detalles:
     • Proveedor: Uber
     • Concepto: Viaje
     • Valor: $25,000
     • Categoría: transporte
```

**Datos extraídos:**
- Proveedor/comercio
- Fecha (formato YYYY-MM-DD)
- Concepto/descripción
- Subtotal (sin IVA)
- IVA (19% en Colombia)
- Total pagado
- Moneda (COP/USD)
- Método de pago
- Categoría automática

**Categorías soportadas:**
- Comidas
- Transporte
- Software
- Hospedaje
- Oficina
- Servicios
- Combustible
- Salud
- Educación
- Entretenimiento
- Supermercado
- Otros

### 2️⃣ Registro de Ingresos (por texto o voz)
```
Usuario: "ingresar 500000"
Bot: ✅ ¡Ingreso registrado!
     💰 Monto: $500,000 COP
     🏦 Proveedor: BANCOLOMBIA
     📝 Concepto: TRANSFERENCIA BANCARIA
     📅 Fecha: 2025-11-28
```

**Flujo:**
1. Usuario escribe "ingresar" o "ingresar [monto]"
2. Si no especifica monto, el bot lo solicita
3. Se guarda en BD con categoría "INGRESO"

### 3️⃣ Consultas Financieras
```
Usuario: "total de gastos este mes"
Bot: 📊 Resultados de consulta
     📝 Registros encontrados: 15
     
     1. 📅 2025-11-25
        🏦 Uber
        📋 Viaje al centro
        💰 $18,500 COP
        🏷️ transporte
     
     [... más registros ...]
     
     💵 Total: $850,000 COP
```

**Tipos de consulta:**
- Total de ingresos
- Total de gastos
- Gastos por categoría
- Gastos por proveedor
- Rangos de fecha personalizados
- Agrupación (por categoría, proveedor, mes)

**Filtros temporales:**
- Hoy / ayer
- Esta semana / mes
- Mes pasado
- Este año
- Últimos 7/30 días
- Fechas específicas: "gastos del 22 de noviembre"
- Rangos: "gastos entre el 20 y 25 de noviembre"

## 🛠️ Tecnologías Utilizadas

### Inteligencia Artificial
- **Google Gemini 2.5 Flash**: OCR de facturas y transcripción de voz
- **Google Gemini Chat**: Procesamiento de lenguaje natural
- **ElevenLabs**: Síntesis de voz (Text-to-Speech)

### Almacenamiento
- **PostgreSQL**: Base de datos `finanzasPersonales`
- **Google Drive**: Respaldo de facturas en carpeta dedicada

### Comunicación
- **Telegram Bot API**: Interfaz de usuario

### Orquestación
- **n8n**: Automatización del flujo de trabajo


## 🔄 Flujo de Trabajo

```
┌─────────────────┐
│ Mensaje Telegram│
└────────┬────────┘
         │
    ┌────▼─────┐
    │ Switch   │
    │ Tipo Msg │
    └──┬──┬──┬─┘
       │  │  │
   ┌───┘  │  └───┐
   │      │      │
┌──▼──┐ ┌▼──┐ ┌─▼───┐
│Image│ │Text│ │Voice│
└──┬──┘ └┬──┘ └─┬───┘
   │     │      │
┌──▼──┐  │   ┌──▼────────┐
│Gemini│  │   │Transcribe │
│ OCR │  │   │(Gemini)   │
└──┬──┘  │   └──┬────────┘
   │     │      │
┌──▼─────▼──────▼──┐
│   AI Agent       │
│  (Clasificador)  │
└──┬───────────┬───┘
   │           │
┌──▼───┐   ┌───▼────┐
│Insert│   │Consulta│
│  DB  │   │   DB   │
└──┬───┘   └───┬────┘
   │           │
┌──▼───────────▼──┐
│  Respuesta Bot  │
│(Texto/Audio)    │
└─────────────────┘
```

## 🚀 Configuración

### Credenciales Requeridas

1. **Telegram Bot**
   - Token de CamilaBot
   - Webhook configurado

2. **Google Gemini (PaLM) API**
   - Cuenta activa
   - Modelo: `gemini-2.5-flash`

3. **PostgreSQL**
   - Base de datos configurada
   - Tabla `finanzasPersonales` creada

4. **Google Drive API**
   - Carpeta "facturas" (ID: `1R6vjon_EpmkU9nTHVuIxpUopPjT6j1Hq`)
   - Permisos OAuth2

5. **ElevenLabs API**
   - Cuenta activa
   - Voz "danielRamirez" (ID: `qT1zlDAVPK36nkOuvo7T`)

### Variables de Entorno

```env
TELEGRAM_BOT_TOKEN=your_token_here
GOOGLE_GEMINI_API_KEY=your_key_here
POSTGRES_CONNECTION_STRING=postgresql://...
GOOGLE_DRIVE_FOLDER_ID=
ELEVENLABS_API_KEY=your_key_here
```

## 📝 Ejemplos de Uso

### Registrar un gasto
```
Usuario: [Foto de factura de restaurante]
Bot: Has añadido un gasto con los siguientes detalles:
     • Proveedor: Restaurante El Sabor
     • Concepto: Almuerzo ejecutivo
     • Valor: 35000
     • Categoría: comidas
```

### Registrar un ingreso
```
Usuario: "ingresar 1500000"
Bot: ✅ ¡Ingreso registrado!
     💰 Monto: $1,500,000 COP
     🏦 Proveedor: BANCOLOMBIA
     📝 Concepto: TRANSFERENCIA BANCARIA
     📅 Fecha: 2025-11-28
```

### Consultar gastos
```
Usuario: "total de gastos este mes"
Bot: 📊 Resultados de consulta
     📝 Registros encontrados: 45
     💵 Total: $2,350,000 COP
```

```
Usuario: "gastos de Uber"
Bot: 📊 Resultados de consulta
     📝 Registros encontrados: 8
     💵 Total: $145,000 COP
```

```
Usuario: "gastos por categoría"
Bot: 📊 Resultados de consulta
     1. comidas: $850,000 (15 registros)
     2. transporte: $420,000 (12 registros)
     3. supermercado: $380,000 (5 registros)
     ...
```

### Consultas con fechas
```
Usuario: "gastos del 22 de noviembre"
Usuario: "ingresos de este año"
Usuario: "gastos entre el 20 y 25 de noviembre"
```

## ⚡ Características Avanzadas

### Memoria Conversacional
El bot mantiene contexto por usuario usando `Simple Memory` con la ID del chat como clave de sesión.

### Respuestas Adaptativas
- **Mensaje de texto**: Responde solo si no es nota de voz
- **Mensaje de audio**: Genera audio con ElevenLabs si el input fue voz
- **Indicadores visuales**: Muestra "grabando audio..." mientras genera respuestas

### Validación de Datos
- Formato de fecha estandarizado (YYYY-MM-DD)
- Conversión automática de zona horaria (America/Bogota)
- Redondeo a 2 decimales
- Manejo de errores con respuestas amigables

### Límites de Consulta
- Máximo 50 registros por consulta
- Primeros 10 registros mostrados en detalle
- Resumen de registros adicionales

## 🔐 Seguridad

- Sesión aislada por usuario (Telegram user ID)
- Webhooks seguros para Telegram
- Credenciales almacenadas en n8n encriptadas
- Sin almacenamiento de datos sensibles en texto plano

## 📄 Licencia

Proyecto privado - Uso personal

## 👨‍💻 Autor

Workflow de n8n para gestión financiera personal por @danielru15

---

**Nota**: Este bot está optimizado para Colombia (IVA 19%, moneda COP, zona horaria America/Bogota). Se puede adaptar fácilmente a otros países ajustando los parámetros en el nodo "Analyze an image" y "INSERT IN DB".

