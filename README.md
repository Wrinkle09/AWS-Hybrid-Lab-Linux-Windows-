# Laboratorio AWS Híbrido (Linux + Windows)

## 6. Comunicación ICMP

### Windows

```powershell
netsh advfirewall firewall add rule name="Allow ICMPv4-In" protocol=icmpv4:8,any dir=in action=allow
```

### Linux

- ICMP ya permitido por defecto.

**Resultado:**  
Ping funcional en ambos sentidos usando IPs privadas (`10.0.x.x`).

---

## 7. SSH desde Windows hacia Linux

El primer intento fallaba por *timeout* porque la clave privada no estaba en el Windows Server.

### Solución aplicada

1. **Generar clave SSH en Windows:**

   ```powershell
   ssh-keygen
   ```

2. **Copiar la clave pública a Linux:**

   ```bash
   ~/.ssh/authorized_keys
   ```

3. **Ajustar permisos en Linux:**

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

- **Event Viewer:** revisar secciones *Security* y *System*.  
- Comprobación de eventos RDP.  
- Revisión de logs del firewall.

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
