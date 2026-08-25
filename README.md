# Piloto Vial Abierto

Capa abierta de implementación municipal para el inventario y monitoreo del estado vial con inteligencia artificial, construida sobre el motor de detección [Pavimentados](https://github.com/EL-BID/pavimentados) del Banco Interamericano de Desarrollo.

Propuesta presentada a la convocatoria de GovTech Connect (BID Lab y Red de Innovación Local) para la implementación de un piloto de código abierto en gobiernos locales de CIIAR Uruguay, en el eje de infraestructura urbana.

## El problema

El mantenimiento vial de un gobierno local opera, en general, de forma reactiva: se interviene cuando el deterioro ya es visible o cuando el reclamo ciudadano lo impone. El insumo que permitiría planificar, un inventario georreferenciado y actualizado del estado del pavimento y de la señalización, exige relevamientos manuales lentos y costosos que envejecen desde el día en que se terminan.

## Qué hace

Convierte los recorridos de los vehículos municipales que ya circulan por la ciudad (recolección, inspección) en un inventario georreferenciado y continuo del estado del pavimento y de la señalización vial. Cada pasada de un vehículo con cámara embarcada actualiza el inventario: la repetición en el tiempo es la diferencia entre una fotografía puntual y un monitoreo continuo que muestra qué calles se degradan más rápido.

Pavimentados, el motor de detección publicado por el BID, identifica nueve tipos de fallas de pavimento, señalización vertical en diez categorías y desgaste de la demarcación horizontal a partir de video con GPS, y entrega tablas de datos. Esta capa construye todo lo que un equipo municipal necesita alrededor de ese motor: captura, georreferenciación, persistencia, visualización y transferencia.

## Arquitectura

```
1. CAPTURA           Dashcams con GPS en vehículos municipales
                     Vía A: recorridos controlados (línea base)
                     Vía B: flota en operación ordinaria (monitoreo continuo)
                          |
2. GEORREFERENCIACIÓN Procesamiento de video y traza GPS
                     Muestreo de imágenes por distancia recorrida
                     Control de calidad de posición
                          |
3. DETECCIÓN         Motor Pavimentados (BID): fallas de pavimento,
                     señalización vertical, demarcación horizontal
                     Validación local contra inspección visual
                          |
4. PERSISTENCIA      PostgreSQL/PostGIS, organizada por tramo
                     y por campaña de captura (series temporales)
                          |
5. VISUALIZACIÓN     Mapa web por tramos con filtros (tipo de falla,
                     señalización, campaña, zona) y vista de tramos
                     ordenados por densidad de fallas + capas QGIS
                     para el equipo técnico de obras
                          |
6. TRANSFERENCIA     Capacitación, protocolo de captura documentado
                     y contribuido al repositorio del motor.
                     Publicación opcional del inventario de señalética
                     como dato abierto (catalogodatos.gub.uy, AGESIC)
```

## Componentes y licencias

| Componente | Rol | Licencia |
|---|---|---|
| [Pavimentados](https://github.com/EL-BID/pavimentados) | Motor de detección (BID) | Licencia abierta del BID, publicada en su repositorio |
| Pipeline de captura y georreferenciación | Video + GPS a imágenes georreferenciadas | MIT (esta capa) |
| PostgreSQL / PostGIS | Base de datos geográfica | PostgreSQL License |
| Mapa web (MapLibre) + QGIS | Visualización y análisis | BSD / GPL |
| Carga y modelo de datos por tramo | Comparación entre campañas | MIT (esta capa) |

Todo el código nuevo desarrollado en el piloto se publica en este repositorio bajo licencia MIT.

## Propiedad y transferencia

El gobierno local recibe la titularidad plena sobre el conjunto de los datos y las imágenes generados, alojados en su propia infraestructura. Los componentes elegidos son estándares que cualquier proveedor o técnico municipal puede mantener o reemplazar: el piloto no genera dependencia del implementador, no tiene licencias recurrentes y no produce lock-in de ningún tipo.

En materia de protección de datos personales (Ley n.º 18.331 de Uruguay), las imágenes crudas captadas en la vía pública se procesan y almacenan internamente y no se publican. La información sobre el estado del pavimento se concibe como insumo interno de gestión; la única publicación prevista, de carácter opcional y sujeta a aprobación expresa del gobierno local, es el inventario de señalética vial.

## Hoja de ruta

| Fase | Contenido | Módulos abiertos de referencia |
|---|---|---|
| Fase 1 (el piloto) | Inventario vial con IA, monitoreo continuo inicial, mapa de consulta con filtros, transferencia y capacitación | Pavimentados (licencia abierta del BID), PostgreSQL/PostGIS, MapLibre, QGIS |
| Fase 2 | Índice de condición por tramo alimentado por la serie temporal; memoria visual de la red con imagen a nivel de calle y anonimización automática de rostros y matrículas | Panoramax (MIT, IGN Francia + OpenStreetMap France), SGBlur (MIT), norma ASTM D6433 como referencia del índice |
| Fase 3 | Priorización presupuestaria abierta (qué tramo intervenir primero y con qué relación costo-beneficio); telemetría y procesamiento embarcado en la flota completa; replicación en las demás ciudades de la coalición | Metodologías públicas del Banco Mundial (RONET/RED) reimplementadas en MIT, Traccar (Apache 2.0) para telemetría de flota, inferencia embarcada en hardware de bajo costo |

## Estado

Propuesta en fase de postulación. El código de la capa de implementación se desarrollará y publicará en este repositorio durante la ejecución del piloto, junto con la documentación técnica y el protocolo de captura.

## Equipo y contacto

IberX Technologies S.L. (IberMove), Barcelona, España.

Contacto: mauricio.pigola@ibermove.com
