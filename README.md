# Monitoreo

En este laboratorio exploraremos monitoreo con herramientas disponibles


## Aplicaciones
```bash
docker compose up -d --build
```

## Servicios y URLs
| Servicio       | URL                         | Notas                                  |
|----------------|-----------------------------|----------------------------------------|
| Frontend       | http://localhost:8080       | Hello World + botones de tráfico/carga |
| Backend (API)  | http://localhost:3001       | `/api/hello`, `/metrics`, `/load`      |
| Grafana        | http://localhost:3000       | admin / admin                          |
| Prometheus     | http://localhost:9090       | datasource ya provisionado             |
| Loki           | http://localhost:3100       | datasource ya provisionado             |
| Alloy (UI)     | http://localhost:12345      | estado del recolector de logs          |
| cAdvisor       | http://localhost:8081       | métricas por contenedor                |
| node-exporter  | http://localhost:9100/metrics | métricas del host                    |

## Configuraciones
- **Datasources** Prometheus y Loki (provisionados automáticamente).
- Logs etiquetados por Alloy con `tier=application` o `tier=infrastructure`.

## Actividad
- El **dashboard** (paneles de CPU + logs de app e infra).
- La **alarma** de CPU > 50%.

## Reset
```bash
docker compose down -v   # borra también dashboards/alarmas creados
```

> Nota de versiones: el tag `prom/prometheus:latest` apunta aún a la rama 2.x (LTS),
> por eso fijamos `v3.8.1`. Promtail EOL (2026-03-02); el recolector de logs
> es Grafana Alloy.


## Consideraciones del entorno

Durante el desarrollo del laboratorio se trabajó en **macOS utilizando Docker Desktop**, por lo que se presentó una diferencia respecto a la guía original.

La consulta propuesta para monitorear el consumo de CPU del contenedor backend fue:

```promql
sum(rate(container_cpu_usage_seconds_total{name="lab-backend"}[1m])) * 100
```

Sin embargo, en este entorno **cAdvisor no expuso correctamente la etiqueta `name="lab-backend"`**, por lo que la consulta no retornó datos.

Para solucionar este inconveniente se utilizó la métrica expuesta directamente por la aplicación backend:

```promql
rate(backend_process_cpu_seconds_total{job="backend"}[1m]) * 100
```

Esta métrica representa el consumo de CPU del proceso de la aplicación backend y permitió monitorear correctamente su comportamiento, configurar alertas y verificar el ciclo completo de observabilidad.

La regla de alerta configurada con esta métrica funcionó correctamente, alcanzando el estado **Firing** y generando los eventos esperados en Grafana y Loki.