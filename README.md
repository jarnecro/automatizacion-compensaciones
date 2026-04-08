# automatizacion-compensaciones

# Automatizador de Compensaciones con IA

Sistema de automatización end-to-end para el análisis y distribución de reportes de compensación, desarrollado con **n8n**, **Google Gemini** y aplicaciones de **Google Workspace**.

---

## 📋 Descripción del Problema

El área de Compensaciones e Incentivos de RRHH enfrenta un proceso manual y repetitivo: recibir datos de empleados, analizarlos individualmente, generar reportes, distribuirlos a jefaturas y notificar a empleados. Este proceso, si se hace de manualmente, puede tomar horas o días dependiendo del volumen.

**Este sistema automatiza completamente ese flujo**, reduciendo el tiempo de procesamiento de horas a minutos, eliminando errores manuales y garantizando consistencia en todos los reportes.

---

## ✅ Funcionalidades

- 📥 **Dos modos de ingreso:** registro individual vía Google Forms o carga masiva vía CSV
- 🤖 **Análisis con IA:** Gemini genera reportes profesionales personalizados por empleado
- 📂 **Organización automática:** crea carpetas en Google Drive por jefatura y fecha
- 🔒 **Privacidad:** los reportes se comparten directamente con cada empleado y jefatura vía permisos de Drive, sin exponer datos sensibles en emails
- 📧 **Notificaciones automáticas:** emails a empleados y resumen agrupado por jefatura
- 📊 **Trazabilidad:** para el registro individual actualiza estado en Google Sheets al finalizar cada procesamiento
- ⚠️ **Manejo de errores:** workflow de errores que notifica al administrador automáticamente
- 🔁 **Detección de duplicados:** verifica registros ya procesados antes de ejecutar

---

## 🏗️ Arquitectura del Sistema

### Flujo 1: Registro Individual (Google Forms)

<img width="3269" height="736" alt="imagen" src="https://github.com/user-attachments/assets/b0cb78fc-8a8f-4cc6-94dc-3c9978cd2cdb" />


### Flujo 2: Carga Masiva CSV (Google Drive)

<img width="3071" height="756" alt="imagen" src="https://github.com/user-attachments/assets/87765b65-7786-4d0c-acde-fb8dff044c24" />


---

## 🛠️ Stack Tecnológico

| Herramienta | Uso |
|------------|-----|
| **n8n** (self-hosted) | Orquestación de flujos de automatización |
| **Google Gemini** | Generación de reportes con IA |
| **Google Sheets** | Registro de empleados y trazabilidad |
| **Google Drive** | Almacenamiento y control de acceso a reportes |
| **Gmail** | Notificaciones automáticas |
| **Google Forms** | Ingreso individual de datos |
| **Docker** | Infraestructura local de n8n |

---

## 📊 Estructura del CSV (Carga Masiva)

```csv
Nombre;Correo;Correo jefatura;RUT;Cargo;Sueldo base;Bono;Fecha evaluación
Nombre Apellido;empleado@correo.com;jefatura@correo.com;12.543.656-3;Analista;1500000;200000;01-03-2024
```

### Validaciones del formulario individual
Para asegurar que los datos sean correctos se valida que:
- **RUT:** formato `12.345.678-9`
- **Sueldo/Bono:** formato `1.500.000`
- **Fecha:** formato `DD-MM-YYYY`

---

## 🔒 Seguridad y Privacidad

- Los datos sensibles (sueldo, bono) **no se envían por email**
- Los reportes se comparten mediante **permisos individuales de Google Drive**
- Cada empleado solo puede ver su propio reporte
- Cada jefatura solo puede ver los reportes de sus empleados
- n8n corre **localmente con Docker**

---

## ⚙️ Requisitos Previos

- Docker instalado
- Cuenta de Google
- API Key de Google Gemini (AI Studio)
- Proyecto en Google Cloud con las siguientes APIs habilitadas:
  - Google Sheets API
  - Google Drive API
  - Gmail API

---

## 🚀 Instalación

### 1. Levantar n8n con Docker

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

Acceder en: `http://localhost:5678`

### 2. Configurar credenciales en n8n

- **Google Sheets OAuth2:** Client ID y Secret de Google Cloud
- **Google Drive OAuth2:** mismas credenciales
- **Gmail OAuth2:** mismas credenciales
- **Google Gemini API:** API Key de AI Studio

### 3. Importar workflows

1. En n8n ir a **Workflows → Import**
2. Importar `flujo-individual.json`
3. Importar `flujo-masivo.json`
4. Configurar credenciales en cada nodo
5. Activar ambos workflows con **Publish**

---

## 📁 Estructura del Repositorio

```
automatizacion-compensaciones/
│
├── workflows/
│   ├── flujo-individual.json      # Workflow registro individual
│   └── flujo-masivo.json          # Workflow carga masiva CSV
│
├── ejemplos/
│   ├── plantilla-nomina.csv       # Plantilla CSV de ejemplo
│   └── reporte-ejemplo.txt        # Ejemplo de reporte generado
│
├── capturas/
│   ├── flujo-individual.png
│   ├── flujo-masivo.png
│   └── reporte-gemini.png
│
└── README.md
```

---

## 💡 Decisiones Técnicas

**¿Por qué n8n self-hosted?**
Permite mantener los datos dentro de la infraestructura propia, crítico para datos sensibles de compensaciones en una institución financiera.

**¿Por qué Google Workspace?**
Integración nativa entre Sheets, Drive, Forms y Gmail permite construir un ecosistema cohesivo sin dependencias externas complejas.

---

## ⚠️ Limitaciones y Mejoras Futuras

- **Personalizar el reporte para las jefaturas:** actualmente el reporte se crea en base a un prompt con Gemini. Mejora: incluir indicadores del área que evaluen el desempeño de los empleados y sugerir tomas de decisiones en base a ellos.
- **Descarga de información:** el archivo está previamente subido a la carpeta de Drive. Mejora: incluir flujo que descargue la información desde portal o ERP usado.
- **Rollback en errores:** si el proceso falla a mitad, las carpetas creadas quedan vacías. Mejora: implementar limpieza automática en caso de errores.
- **Escalabilidad:** para nóminas de miles de empleados, considerar n8n cloud para garantizar disponibilidad 24/7.
- **Portal de autoservicio:** reemplazar el email al empleado por un portal web donde pueda consultar su historial de compensaciones.

---

## 👤 Autor

Javier Contreras  
[LinkedIn](https://www.linkedin.com/in/jconv/) | [GitHub](https://github.com/jarnecro)
