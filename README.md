# Laboratorio AWS Híbrido (Linux + Windows)

## Descripción del laboratorio

Este laboratorio recrea una infraestructura híbrida en AWS combinando un servidor **Linux (Ubuntu)** y un **Windows Server**.  
El objetivo ha sido practicar **administración de sistemas, redes, seguridad** y conceptos fundamentales de **cloud computing**, utilizando únicamente recursos del **Free Tier**.

---

## Arquitectura desplegada

- **VPC personalizada:** 10.0.0.0/16  
- **Subred pública:** 10.0.1.0/24  
- **Internet Gateway** y **tabla de rutas** asociada  
- **Instancia EC2 Linux (Ubuntu Server)**  
- **Instancia EC2 Windows Server 2019/2022**  
- **Security Groups** configurados para SSH, RDP e ICMP  
- **Comunicación privada** entre máquinas  

---

## 1. Creación de la red en AWS

- Creación de la **VPC desde cero**.  
- **Subred pública** configurada.  
- **Internet Gateway** asociado a la VPC.  
- Ruta `0.0.0.0/0` configurada hacia el Internet Gateway.  
- Diagnóstico de conectividad inicial (sin Internet por falta de IGW/rutas).  
- Problemas solucionados ajustando routing y subredes.  

---

## 2. Instancia Linux y acceso SSH

- EC2 **Ubuntu** desplegada en la subred pública.  
- **IP pública** habilitada.  
- **Security Group** restringido a solo mi IP.  
- Acceso **SSH probado correctamente**.  
- Conectividad a Internet verificada.  

---

## 3. Servidor Web (Nginx)

- Instalación con `apt`.  
- Servicio habilitado.  
- Apertura del **puerto 80** en el SG.  
- Página por defecto **personalizada**.  
- Acceso externo confirmado mediante **IP pública**.

---

## 4. Seguridad en Linux

### UFW

```bash
ufw allow OpenSSH
ufw allow 'Nginx Full'
ufw enable
```

### Hardening SSH

- Desactivado login de **root**.  
- Deshabilitada **autenticación por contraseña**.  

Archivo modificado:  
`/etc/ssh/sshd_config`

### Fail2ban

- Jail de **SSH** configurado.  
- Revisión de estado:

```bash
fail2ban-client status
fail2ban-client status sshd
```

### Actualizaciones automáticas

- Revisión de `unattended-upgrades`.

---

## 5. Instancia Windows Server

- **Windows Server 2019/2022** en la misma VPC.  
- Acceso por **RDP** restringido a mi IP.  
- Recuperación de contraseña con la **key PEM**.  
- Escritorio remoto funcionando correctamente.  

---

## 6. Conectividad Linux ↔ Windows

### ICMP

Para habilitar **ping** entre máquinas:

#### Windows → permitir ICMP entrante

```powershell
netsh advfirewall firewall add rule name="Allow ICMPv4-In" protocol=icmpv4:8,any dir=in action=allow
```

#### Linux → ICMP permitido por defecto

**Resultado:**  
Ping funcional en ambos sentidos usando IPs privadas (`10.0.x.x`).

---

## 7. SSH desde Windows hacia Linux

El primer intento fallaba por *timeout* porque la clave privada no estaba en el Windows Server.

**Solución aplicada:**

1. Generar clave en Windows:

   ```powershell
   ssh-keygen
   ```

2. Copiar la clave pública a Linux:

   ```bash
   ~/.ssh/authorized_keys
   ```

3. Ajustar permisos en Linux:

   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

**Resultado:**  
SSH desde Windows → Linux funcionando correctamente.

---

## 8. Monitorización básica

### Linux

```bash
htop
fail2ban-client status
fail2ban-client status sshd
ufw status verbose
tail -n 20 /var/log/ufw.log
```

### Windows

- **Event Viewer:** revisar *Security* / *System*.  
- Comprobación de eventos de **RDP**.  
- Revisión de **logs del firewall**.

---

## Aprendizajes clave

- Creación completa de una **VPC personalizada**.  
- Configuración de **routing público** en AWS.  
- Administración de **Security Groups** y **firewalls internos**.  
- **Hardening** de un servidor Linux.  
- Despliegue y gestión de **Windows Server en AWS**.  
- Uso y distribución de **claves SSH** entre sistemas.  
- Resolución de problemas de red (**ICMP**, **SSH**, **RDP**).  
- Comunicación entre instancias dentro de una **VPC**.  
- Introducción a **monitorización y análisis de logs**.

---

## Estado final del laboratorio

- Arquitectura híbrida **100% funcional**.  
- **Servidor web operativo** y accesible.  
- **SSH** y **RDP** configurados de forma segura.  
- **Firewall** y **Fail2ban** implementados.  
- Comunicación **Linux ↔ Windows** estable.  
- Entorno listo para **pruebas adicionales** (AD, DNS, IIS, etc.).
