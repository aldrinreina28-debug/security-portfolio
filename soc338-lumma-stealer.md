# Writeup — Investigación SOC: Phishing → infostealer (DLL Side-Loading vía ClickFix)

> Ejercicio práctico de análisis SOC en un entorno de laboratorio. Rol: analista de seguridad (blue team). Resultado: True Positive.

## Escenario
Alerta de severidad **crítica** (tipo: fuga de datos): posible infección por un **infostealer (familia Lumma)** distribuido mediante **DLL side-loading**, a partir de un phishing con técnica **ClickFix** (se engaña al usuario para que ejecute un script malicioso).

## Herramientas / entorno
- Gestión de logs (SIEM)
- Análisis de correo electrónico
- Revisión de acciones de red
- Threat intelligence (VirusTotal / AbuseIPDB)

## Investigación (triaje → análisis → contención)
1. **Triaje:** se recibe la alerta crítica. Se revisan el correo de phishing y el endpoint afectado.
2. **Análisis:** en la gestión de logs y en las acciones de red se confirma que el **host del usuario afectado accedió a la URL maliciosa y ejecutó el script**. Encaja con la cadena ClickFix → ejecución de script → DLL side-loading → infostealer.
3. **Verdicto:** **True Positive** — actividad maliciosa confirmada (ejecución real en el endpoint).
4. **Contención:** se contiene el host afectado y se **elimina el correo de phishing** del buzón del destinatario para evitar nuevas víctimas.

## Mapeo MITRE ATT&CK
- Initial Access — **Phishing** (T1566)
- Execution — **User Execution** (T1204) / Command and Scripting Interpreter (T1059)
- Defense Evasion — **DLL Side-Loading / Hijack Execution Flow** (T1574.002)
- Impacto — robo de información (infostealer)

## Conclusión
Incidente confirmado como infección por infostealer vía phishing con ClickFix. Acciones: contención del host y eliminación del correo. Recomendaciones: concienciación anti-phishing, bloqueo del dominio/URL maliciosos, revisión de credenciales potencialmente comprometidas y búsqueda de otros destinatarios del mismo correo.
