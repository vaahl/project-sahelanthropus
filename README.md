# Project Sahelanthropus (HomeLab)

Infraestructura de virtualización *bare-metal* basada en contenedores para prácticas de SysAdmin y Ciberseguridad.
El objetivo es simular un entorno de producción Enterprise utilizando distribuciones RHEL-based y segmentación de almacenamiento estratégica.

### Dashboard Principal
<img width="2242" height="614" alt="image" src="https://github.com/user-attachments/assets/cd762f5b-7c3f-4e55-8c53-d4865aff980a" />


## Hardware Specs

**Host:** HPE ProLiant MicroServer N40L
* **CPU:** AMD Turion II Neo N40L (Dual-Core @ 1.5GHz)
* **RAM:** 4GB ECC DDR3 (Server Grade)
* **Network:** 1Gbps Ethernet (Onboard)

### Storage Layout & Terminal verification
<img width="688" height="212" alt="image" src="https://github.com/user-attachments/assets/a25a34ee-f878-4df8-8c64-1551736cd27b" />


| Disk ID | Size | Type | Mount Point | FS | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `sdb` | 250GB | SSD | `/` (Root) | XFS | OS & System Config |
| `sda` | 500GB | HDD | `/data` | XFS | Docker Volumes & Mass Storage |

---

## Software Stack

* **OS:** AlmaLinux 8.10 (Minimal Install) - *RHEL Binary Compatible*
* **Container Engine:** Docker CE (Community Edition) v27+
* **Orchestration:** Portainer CE
* **Monitoring:** Uptime Kuma (HTTP/TCP checks)
* **Management:** Cockpit Project (Web Console)

---

## ⚙️ Configuración e Implementación

### 1. Sistema Operativo & Red
Instalación "Minimal" para reducir la superficie de ataque.
* **FirewallD:** Política restrictiva por defecto (`Drop`). Puertos abiertos manualmente: `9090`, `9443`, `3001`.
* **Cockpit:** Habilitado como servicio systemd.

### 2. Gestión de Almacenamiento (Critical)
Montaje persistente del disco secundario para evitar saturación del root filesystem.

**Configuration (`/etc/fstab`):**
```bash
UUID=ddce45a7-8868-44f1-9a56-d89758af6d38  /data  xfs  defaults  0  0
```

### Proof of Concept:
Monitoreo activo de servicios externos:

<img width="1673" height="996" alt="image" src="https://github.com/user-attachments/assets/143a657c-0ad9-44da-b23a-bfd62d82dc4d" />
