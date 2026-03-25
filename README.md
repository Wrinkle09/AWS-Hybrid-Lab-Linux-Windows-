# Laboratorio AWS Híbrido (Linux + Windows)

## Descripción del laboratorio
Este laboratorio recrea una infraestructura híbrida en AWS combinando un servidor Linux (Ubuntu) y un Windows Server.  
El objetivo ha sido practicar administración de sistemas, redes, seguridad y conceptos fundamentales de cloud computing utilizando únicamente recursos del Free Tier.

---

## Arquitectura desplegada
- VPC personalizada (10.0.0.0/16)
- Subred pública (10.0.1.0/24)
- Internet Gateway y tabla de rutas asociada
- Instancia EC2 Linux (Ubuntu Server)
- Instancia EC2 Windows Server 2019/2022
- Security Groups configurados para SSH, RDP e ICMP
- Comunicación privada entre máquinas

---

## Pasos realizados

### 1. Creación de la red en AWS
- Creación de la VPC desde cero
- Subred pública configurada
- Internet Gateway asociado a la VPC
- Ruta 0.0.0.0/0 configurada hacia el IGW
- Diagnóstico de conectividad inicial (sin Internet por falta de IGW/rutas)
- Problemas solucionados ajustando routing y subredes

---

### 2. Instancia Linux y acceso SSH
- EC2 Ubuntu desplegada en la subred pública
- IP pública habilitada
- Security Group restringido a **solo mi IP**
- Acceso SSH probado correctamente
- Conectividad a Internet verificada

---

### 3. Servidor Web (Nginx)
- Instalación con `apt`
- Servicio habilitado
- Apertura del puerto 80 en el SG
- Página por defecto personalizada
- Acceso externo confirmado mediante IP pública

---

### 4. Seguridad en Linux

#### UFW
```bash
ufw allow OpenSSH
ufw allow 'Nginx Full'
ufw enable
