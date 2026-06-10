# Respuestas – Laboratorio de Observabilidad

Este documento contiene las respuestas a las preguntas planteadas en la guía del laboratorio.

## 1. ¿Por qué necesitamos Loki además de Prometheus si ya tenemos /metrics?

Prometheus almacena **métricas numéricas** en series temporales, como uso de CPU, memoria o cantidad de peticiones. En cambio, Loki almacena **logs o registros de eventos** generados por las aplicaciones y la infraestructura.

Ambas herramientas son complementarias: las métricas permiten identificar **qué está ocurriendo** en el sistema, mientras que los logs ayudan a comprender **por qué ocurrió** un determinado evento o problema.

---

## 2. ¿Qué ventaja aporta que las fuentes de datos de Grafana estén aprovisionadas como código y no creadas a mano?

El aprovisionamiento como código permite que la configuración de las fuentes de datos sea **reproducible, versionable y automatizable**.

Esto evita configuraciones manuales en Grafana, reduce errores humanos y facilita la implementación consistente en distintos entornos de desarrollo, pruebas y producción.

---

## 3. El panel "CPU contenedor" y el panel "CPU host" pueden mostrar valores muy distintos. ¿Por qué? ¿Cuál usarías para alertar sobre una aplicación concreta?

La métrica de CPU del host representa el consumo total de recursos de la máquina donde se ejecutan todos los servicios.

Por otro lado, la métrica de CPU del contenedor refleja únicamente el consumo de recursos de una aplicación o servicio específico. Por ello, el uso de CPU del host suele ser diferente al del contenedor.

Para monitorear el comportamiento de una aplicación concreta resulta más útil utilizar métricas del contenedor o del proceso asociado.

---

## 4. ¿Qué diferencia hay entre el *evaluation interval* y el *pending period de una alarma*?

El **evaluation interval** define cada cuánto tiempo Grafana evalúa la condición configurada en una regla de alerta.

El **pending period** define cuánto tiempo debe mantenerse la condición de alerta antes de cambiar al estado **Firing**.

Esta separación permite evitar falsas alarmas provocadas por picos temporales o fluctuaciones momentáneas de las métricas monitoreadas.
