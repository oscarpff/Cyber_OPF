+++
title = "SIEM Home Lab con Elastic Stack"
date = 2026-01-05
draft = false
tags = ["siem", "elastic", "lab", "home-lab"]
categories = ["Proyectos", "Blue Team"]
+++

## 🎯 Objetivo del Proyecto

Montar un **SIEM casero** usando Elastic Stack para practicar detección de amenazas y análisis de logs en un entorno controlado.

## 🛠️ Herramientas Utilizadas

- **Elasticsearch**: Motor de búsqueda y análisis
- **Kibana**: Visualización de datos
- **Winlogbeat**: Recolección de logs de Windows
- **Filebeat**: Recolección de logs de Linux
- **VirtualBox**: Virtualización

## 📋 Desarrollo

### Paso 1: Arquitectura del Lab

```
┌─────────────────────────────────────┐
│       Elastic Stack Server          │
│  ┌──────────────────────────────┐   │
│  │   Elasticsearch:9200         │   │
│  │   Kibana:5601                │   │
│  └──────────────────────────────┘   │
└─────────────────┬───────────────────┘
                  │
        ┌─────────┴─────────┐
        │                   │
┌───────▼────────┐  ┌──────▼────────┐
│  Windows 10    │  │  Ubuntu 22.04 │
│  (Winlogbeat)  │  │  (Filebeat)   │
└────────────────┘  └───────────────┘
```

### Paso 2: Instalación Elastic Stack

```bash
# Descargar e instalar Elasticsearch
wget https://artifacts.elastic.co/downloads/elasticsearch/...
sudo dpkg -i elasticsearch-*.deb

# Configurar Elasticsearch
sudo nano /etc/elasticsearch/elasticsearch.yml
# network.host: 0.0.0.0
# discovery.type: single-node

# Iniciar servicio
sudo systemctl start elasticsearch
```

### Paso 3: Configuración de Winlogbeat

```yaml
# winlogbeat.yml
winlogbeat.event_logs:
  - name: Security
  - name: System
  - name: Application
  - name: Microsoft-Windows-Sysmon/Operational

output.elasticsearch:
  hosts: ["192.168.1.100:9200"]
```

### Paso 4: Creación de Dashboards

Configuré dashboards personalizados para:
- ✅ Failed login attempts
- ✅ PowerShell execution events
- ✅ Network connections
- ✅ Process creation (Sysmon)

## 📊 Resultados

Después de 24 horas recolectando logs:

| Métrica | Valor |
|---------|-------|
| Eventos totales | 45,230 |
| Failed logins | 127 |
| PowerShell events | 892 |
| Network connections | 5,341 |

### Detecciones Configuradas

1. **Brute Force Detection**: >5 failed logins en 5 minutos
2. **PowerShell Empire**: Patrones de ofuscación comunes
3. **Port Scanning**: Múltiples conexiones a diferentes puertos
4. **Lateral Movement**: PsExec / WMI execution

## 🔍 Conclusiones

✅ **Lo que funcionó bien:**
- Elastic Stack es muy potente y flexible
- Los dashboards son intuitivos
- Fácil de escalar

⚠️ **Desafíos encontrados:**
- Consumo de recursos (RAM >8GB recomendado)
- Fine-tuning de detecciones para reducir falsos positivos
- Gestión de índices y retención de datos

## 🚀 Próximos Pasos

- [ ] Agregar Suricata para IDS/IPS
- [ ] Integrar threat intelligence feeds
- [ ] Automatizar respuesta con SOAR
- [ ] Añadir más reglas MITRE ATT&CK

## 📚 Referencias

- [Elastic SIEM Documentation](https://www.elastic.co/security)
- [Detection Engineering Guide](https://github.com/...)
- [MITRE ATT&CK Framework](https://attack.mitre.org)

---

💡 **Tip**: Si montas este lab, empieza con una VM pequeña y aumenta recursos según necesites.
