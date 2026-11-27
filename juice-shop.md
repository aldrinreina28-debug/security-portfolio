# Writeup — Evaluación de seguridad de OWASP Juice Shop

> Ejercicio de laboratorio sobre OWASP Juice Shop, una aplicación web deliberadamente vulnerable. Evaluación combinando escaneo automático y explotación manual.

## Objetivo y alcance
Evaluar la seguridad de OWASP Juice Shop (http://localhost:3000) en un entorno de laboratorio aislado, identificando y explotando vulnerabilidades. Sin impacto en sistemas reales.

## Herramientas
- Kali Linux
- OWASP ZAP (escaneo automático)
- Navegador (pruebas manuales)

## Metodología
1. **Reconocimiento y escaneo automático:** spidering y escaneo activo con OWASP ZAP para mapear la aplicación e identificar alertas.
2. **Explotación manual** de las vulnerabilidades más relevantes.

## Hallazgos

### 1. Inyección SQL → Bypass de autenticación (Crítica)
El campo de email del login no sanea la entrada, lo que permite una **inyección SQL** que salta la autenticación. Introduciendo `[confirma el payload que usaste, p. ej. ' OR 1=1--]` en el email, se obtiene acceso como **administrador** (admin@juice-sh.op, rol `admin`).
- OWASP Top 10: A03 Injection.
- **Impacto:** acceso total como admin sin credenciales válidas.
- CVSS v3.1: ~9.8 (Critical) — *[revísalo y confírmalo]*.

### 2. Acceso a documento confidencial (Control de acceso)
Es posible acceder a un documento confidencial sin la autorización adecuada.
- OWASP Top 10: A01 Broken Access Control.
- **Impacto:** exposición de información sensible.

### 3. Gestión de errores deficiente
La aplicación devuelve un error no controlado que puede revelar información interna.
- OWASP Top 10: A05 Security Misconfiguration.
- **Impacto:** fuga de información útil para un atacante.



## Remediación
- **Inyección SQL:** consultas parametrizadas / sentencias preparadas; nunca concatenar la entrada del usuario en el SQL.
- **Control de acceso:** validar la autorización en el servidor para cada recurso.
- **Errores:** capturar excepciones y mostrar mensajes genéricos, sin trazas ni detalles internos.
