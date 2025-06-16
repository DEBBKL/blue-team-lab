# 🛡️ Casos Prácticos de Seguridad Informática

Este repositorio contiene la documentación de tres casos prácticos realizados sobre detección de intrusos con Snort, análisis de eventos en Windows 10, y mitigación de fuerza bruta en SSH con Fail2Ban.

---

## 📚 Índice de Contenidos

- [🔍 Caso práctico 1: Configuración de IDS con Snort](#-caso-práctico-1-configuración-de-ids-con-snort)
  - [✅ Objetivo](#-objetivo)
  - [🛠 Herramienta utilizada](#-herramienta-utilizada)
  - [🔧 Reglas creadas](#-reglas-creadas)
  - [📸 Capturas de pantalla](#-capturas-de-pantalla)
  - [▶️ Comprobación de funcionamiento](#-comprobación-de-funcionamiento)

- [🪟 Caso práctico 2: Análisis de eventos en Windows 10](#-caso-práctico-2-análisis-de-eventos-en-windows-10)
  - [✅ Objetivo](#-objetivo-1)
  - [🛠 Herramienta utilizada](#-herramienta-utilizada-1)
  - [🔎 Ejemplos de eventos](#-ejemplos-de-eventos)
  - [📸 Capturas de pantalla](#-capturas-de-pantalla-1)

- [🔐 Caso práctico 3: Bloqueo de ataques por fuerza bruta SSH con Fail2Ban](#-caso-práctico-3-bloqueo-de-ataques-por-fuerza-bruta-ssh-con-fail2ban)
  - [✅ Objetivo](#-objetivo-2)
  - [🛠 Herramientas utilizadas](#-herramientas-utilizadas)
  - [🔧 Pasos de configuración](#-pasos-de-configuración)
  - [🧪 Prueba de funcionamiento](#-prueba-de-funcionamiento)
  - [📸 Capturas de pantalla](#-capturas-de-pantalla-2)

- [🧾 Autor](#-autor)

---

## 🔍 Caso práctico 1: Configuración de IDS con Snort

### ✅ Objetivo

Configurar un sistema IDS que registre toda la actividad de la red y cree reglas personalizadas.

### 🛠 Herramienta utilizada
- **Snort**

Ubicación del archivo de reglas personalizadas:

/etc/snort/rules/local.rules

### 🔧 Reglas creadas

**1. Regla para detectar ataque de flood:**

```snort
alert ip any any -> 192.168.1.0/24 any (msg:"ALERTA: Posible ataque de flood detectado"; threshold: type both, track by_dst, count 100, seconds 1; sid:1000001;)
```

**2. Regla para detectar tráfico HTTP entrante:**

alert tcp any any -> 192.168.1.0/24 80 (msg:"ALERTA: Tráfico HTTP entrante detectado"; sid:1000002;)

**📸 Capturas de pantalla**
- local.rules antes y después de añadir las reglas.
- Terminal mostrando alertas generadas por Snort.

### ▶️ Comprobación de funcionamiento

Ejecutar Snort:

```bash
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
```

Generar tráfico simulado:

### Ataque de flood

```bash
sudo hping3 -c 200 -i u1000 -S 192.168.1.10
```

### Tráfico HTTP

```bash
curl http://192.168.1.10
```

---

## 📌 Caso práctico 2: Análisis de eventos en Windows 10 ##

### ✅ Objetivo
Utilizar el Visor de eventos para investigar incidencias inesperadas.

### 🛠 Herramienta utilizada
Visor de eventos (eventvwr.msc)
Ruta: Registros de Windows > Sistema

### 🔎 Ejemplos de eventos
| Nivel       | ID    | Origen              | Descripción                                          |
| ----------- | ----- | ------------------- | ---------------------------------------------------- |
| Información | 19    | WindowsUpdateClient | Windows Update comenzó a descargar una actualización |
| Error       | 6008  | EventLog            | Cierre inesperado del sistema                        |
| Crítico     | 41    | Kernel-Power        | Reinicio sin apagado limpio                          |
| Advertencia | 10016 | DistributedCOM      | Error de permisos COM                                |


### 📸 Capturas de pantalla

- Un evento por cada tipo (información, error, crítico, advertencia).
- Descripción completa de cada evento.

---

## 📌 Caso práctico 3: Bloqueo de ataques por fuerza bruta SSH con Fail2Ban ##

### ✅ Objetivo
Detectar y bloquear intentos de acceso fallidos al servicio SSH.

### 🛠 Herramientas utilizadas
- Fail2Ban
- OpenSSH Server

### 🔧 Pasos de configuración

- 1. Actualizar el sistema

```bash
sudo apt update && sudo apt upgrade -y && sudo apt full-upgrade -y
```

- 2. Instalar Fail2Ban y servidor SSH

```bash
sudo apt install fail2ban openssh-server
```

- 3. Activar y verificar el servicio SSH

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

- 4. Crear configuración personalizada

Archivo: 
  /etc/fail2ban/jail.local

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 5
bantime = 900
findtime = 600
```

- 5. Reiniciar Fail2Ban
   
```bash
sudo systemctl restart fail2ban
```

- 6. Ver estado del servicio.

```bash
sudo fail2ban-client status sshd
```

---

## 🧪 Prueba de funcionamiento

Desde Kali Linux:

```bash
hydra -l kali -P /usr/share/wordlists/rockyou.txt ssh://10.0.2.6
```

Ver en Ubuntu si la IP fue bloqueada:

```bash
sudo fail2ban-client status sshd
```

## 📸 Capturas de pantalla

- Archivo: jail.local.
- Resultado del comando: fail2ban-client status sshd con la IP bloqueada.
- Registro en: /var/log/fail2ban.log.

---

## 🧾 Autor

Déborah Loisel

📧 soydeborahloisel@gmail.com

📍 Las Palmas de Gran Canaria

🔐 Formación: Seguridad Informática
