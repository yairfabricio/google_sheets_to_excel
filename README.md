---
title: "🧰 ETL desde Google Sheets — Consolidación 2023–2025"
description: >
  Este repositorio contiene un Jupyter Notebook que implementa un flujo de extracción
  y transformación de datos (ET/ETL) desde Google Sheets y consolida información
  de múltiples hojas/años (2023, 2024, 2025) en un único DataFrame listo para análisis.
tags:
  - Google Sheets
  - ETL
  - Python
  - Data Cleaning
---

# 🧰 ETL desde Google Sheets — Consolidación 2023–2025

### 📄 Descripción
Este repositorio contiene un **Jupyter Notebook** que implementa un flujo de **extracción y transformación de datos (ET/ETL)** desde **Google Sheets** y consolida información de **múltiples hojas/años (2023, 2024, 2025)** en un único `DataFrame` listo para análisis.

---

### 🧠 Resumen
Se autentica con una cuenta de servicio (`credenciales.json`), lee datos de Google Sheets, normaliza columnas y estandariza textos, fechas y países.  
Produce un dataset consolidado y puede incluir una etapa opcional de **carga (Load)** a CSV, base de datos o nueva hoja de Google Sheets.

---

### 🧭 Diagrama general
```Google Sheets (múltiples hojas / años)
↓
Extract (gspread)
↓
Transform (pandas: limpieza de columnas,
normalización de texto, parseo de fechas,
estandarización de países)
↓
DataFrame consolidado ✅
```

---

### ✨ Características principales
- 🔐 Autenticación vía **Service Account** (`credenciales.json`)
- 📥 Lectura desde **Google Sheets** usando `gspread` y `gspread_dataframe`
- 🧹 Normalización de columnas y nombres
- 🗓️ Parseo de fechas heterogéneas
- 🌎 Estandarización de países
- 🔤 Limpieza de texto (acentos y caracteres no deseados)
- 🧱 Consolidación de información **2023–2025**
- ⏳ Reintentos simples con `time.sleep` para evitar límites de API

---

### 🧩 Dependencias
```bash
pip install pandas numpy gspread oauth2client gspread-dataframe
```
### 🔑 Credenciales

1. Crea una Service Account en Google Cloud Console y descarga el JSON.  
2. Colócalo como `credenciales.json`.  
3. Comparte tus hojas con el correo de la service account (lector/editor).  
4. Scopes usados:  
   - [https://www.googleapis.com/auth/spreadsheets](https://www.googleapis.com/auth/spreadsheets)  
   - [https://www.googleapis.com/auth/drive](https://www.googleapis.com/auth/drive)  

### 🚀 Uso

Abre ETL_google_sheets.ipynb en Jupyter o VS Code.

Asegúrate de tener credenciales.json en el mismo directorio.

Ejecuta todas las celdas.

Obtendrás un DataFrame consolidado con datos 2023–2025.

### ⚙️ Proceso ETL

Importa librerías (pandas, numpy, gspread, oauth2client, gspread_dataframe).

Autenticación con Service Account.

Lectura de hojas (2023–2025).

Limpieza y normalización.

Consolidación en un único DataFrame.

El flujo actual implementa Extract + Transform (E+T).
Puedes añadir la L (Load) fácilmente.

### 🧪 Validaciones recomendadas

Verificar tipos (df.dtypes)

Revisar duplicados y valores nulos

Controlar outliers

Validar consistencia de fechas

### 🛡️ Buenas prácticas

Usa .gitignore para credenciales

Limpia las salidas antes de hacer commit

Divide funciones largas

Añade pequeñas pruebas automáticas

### 📁 Estructura del proyecto
```
.
├── ETL_google_sheets.ipynb
├── requirements.txt
├── README.md
├── data/
└── credenciales.json  # No subir a git
```

### ⚠️ Problemas comunes

AuthError / 403: comparte el Sheet con la cuenta de servicio.

Rate limit: usa time.sleep o divide lecturas.

Fechas incorrectas: revisa parse_fecha_mixta_especifica.
