# Project Sahelanthropus (HomeLab)

Infraestructura de virtualización *bare-metal* basada en contenedores para prácticas de SysAdmin y Ciberseguridad.
El objetivo es simular un entorno de producción Enterprise utilizando distribuciones RHEL-based y segmentación de almacenamiento estratégica, optimizada para hardware de recursos limitados.

### Dashboard Principal
<img width="100%" alt="Portainer Dashboard" src="https://github.com/user-attachments/assets/cd762f5b-7c3f-4e55-8c53-d4865aff980a" />

---

## Hardware Specs

**Host:** HPE ProLiant MicroServer N40L
* **CPU:** AMD Turion II Neo N40L (Dual-Core @ 1.5GHz)
* **RAM:** 2GB ECC DDR3 (Server Grade)
* **Network:** 1Gbps Ethernet (Onboard)

### Storage Layout & Terminal verification
<img width="100%" alt="Terminal Verification" src="https://github.com/user-attachments/assets/a25a34ee-f878-4df8-8c64-1551736cd27b" />

| Disk ID | Size | Type | Mount Point | FS | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `sdb` | 250GB | SSD | `/` (Root) | XFS | OS & System Config |
| `sda` | 500GB | HDD | `/data` | XFS | Docker Volumes & Mass Storage |

---

## Software Stack

* **OS:** AlmaLinux 8.10 (Minimal Install) - *Selected for resource efficiency on 2GB RAM*
* **Container Engine:** Docker CE (Community Edition) v27+
* **Orchestration:** Portainer CE
* **Monitoring:** Uptime Kuma (HTTP/TCP checks)
* **Management:** Cockpit Project (Web Console)

---

## Configuración e Implementación

### 1. Sistema Operativo & Red
Instalación "Minimal" para reducir la superficie de ataque y el consumo de memoria.
* **FirewallD:** Política restrictiva por defecto (`Drop`). Puertos abiertos manualmente: `9090`, `9443`, `3001`.
* **Cockpit:** Habilitado como servicio systemd para gestión ligera.

### 2. Gestión de Almacenamiento (Critical)
Montaje persistente del disco secundario para evitar saturación del root filesystem.

**Configuration (`/etc/fstab`):**
```bash
# Montaje persistente por UUID
UUID=ddce45a7-8868-44f1-9a56-d89758af6d38  /data  xfs  defaults  0  0
```

### 3. Docker Daemon Relocation
Por defecto, Docker almacena volúmenes en `/var/lib/docker` (Disco OS). Se modificó el demonio para apuntar al disco de almacenamiento masivo.

**File: `/etc/docker/daemon.json`**
```json
{
  "data-root": "/data/docker"
}
```

### 4. Proof of Concept
Monitoreo activo de servicios externos con Uptime Kuma:

<img width="100%" alt="Uptime Kuma" src="https://github.com/user-attachments/assets/143a657c-0ad9-44da-b23a-bfd62d82dc4d" />

### Roadmap
- [x] Hardware Restoration & BIOS Check
- [x] OS Installation (AlmaLinux 8.10) & Tuning
- [x] Storage Partitioning (XFS on `/data`)
- [x] Docker & Portainer Implementation
- [x] Basic Monitoring (Uptime Kuma)
- [ ] DNS Filtering & Network Defense (AdGuard Home)
- [ ] Reverse Proxy (Nginx Proxy Manager)
- [ ] Dashboard Centralization (Homepage)
