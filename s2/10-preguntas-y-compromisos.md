# 10 — Preguntas Pendientes, Compromisos de Andrés y Decisiones de Jefferson

**v0.9 · 8-sep-2026 · Documento VIVO — se actualiza a diario hasta la firma de requisitos v1.0 (vie 11-sep) y cierre de gaps (canal de respuesta por definir el viernes / E04).**

> Nada de lo listado aquí se asume como decidido en la arquitectura. Cada ítem indica **qué bloquea** y **cuál es el default de diseño** mientras tanto (la arquitectura absorbe la mayoría como configuración — `02` §7).

---

## A. Preguntas de negocio para Andrés (gaps)

### A.1 Bloquean la firma de requisitos v1.0 (viernes 11-sep)

| # | Pregunta exacta | Origen | Qué bloquea | Default de diseño mientras tanto |
| :-: | :---- | :---- | :---- | :---- |
| G1 | **Cementerios:** ¿arrancamos con la funeraria coordinando el cementerio (como hoy) y el módulo de cementerio con dispersión propia queda enchufable para después? ¿O hay cementerio aliado desde el piloto? | E03 (Andrés lo iba a pensar) | RN-RED-07; `cemetery_mode` en órdenes; alcance del onboarding | Módulo enchufable; fase 1 `PROVIDER_MANAGED` |
| G2 | **Pasarela:** ¿Wompi, ePayco o Bold? ¿Cuándo abres la cuenta y pides las llaves del API? (Tienes Davivienda → ePayco es de Davivienda; Wompi es de Bancolombia/Grupo Aval) | E03 | RF-PAG-01/06; inicio del módulo de pagos (semana 5–6 del cronograma) | Adaptador desacoplado; sandbox del primero que entregue llaves |
| G3 | **Merchant of record / contadora:** las cuotas de planes pasan por Aequus reteniendo comisión — ¿qué dice tu contadora sobre anticipo vs. recaudo a nombre de terceros? ¿La comisión de Aequus lleva IVA y cómo se factura a la previsora? | B2.10 (sin responder desde S2) | RN-PAG-08; modelo contable de `payments`; obligaciones fiscales | Aequus no factura el servicio; comisión como ingreso propio por definir tratamiento |
| G4 | **Reembolsos y cancelación a mitad de servicio:** si el proveedor rechaza tras el pago, ¿reembolso total automático? Si la familia cancela a mitad, ¿qué se cobra y qué se devuelve? | E04 B10.9 | RN-PAG-06; `refunds`; estados `CANCELADA_PARCIAL` | Reembolso total si no hay aceptación; política de devengado por checklist si hay servicio iniciado |
| G5 | **Fee de la pasarela (1.3–3.3% + fijo):** ¿lo asume Aequus dentro de su comisión o se negocia con el funerario? | E03 (Andrés lo piensa) | RN-PAG-07; márgenes del motor de liquidación | Línea explícita en `settlement_lines` para poder parametrizarlo |

### A.2 Pendientes del banco vivo S2 (canal y fecha de respuesta se acuerdan con Andrés el viernes 11-sep)

| # | Pregunta | Ref. banco | Qué bloquea |
| :-: | :---- | :---- | :---- |
| G6 | Coberturas con topes en SMMLV (bóveda, osario) vs incluido/no incluido | B1.4 | `coverage_policies.smmlv_caps` |
| G7 | Enfermedades que excluyen o encarecen; quién autoriza excepción | B1.8 | `coverage_policies.exclusions`; flujo de excepción |
| G8 | Recién nacido / nuevo cónyuge a mitad de año: ¿desde cuándo cubre? | B1.14/17 | Regla de ingreso fuera de ventana |
| G9 | Si la previsora cambia precio mañana, ¿afiliados actuales conservan el viejo? | B1.17 | Versionado de precios de plan |
| G10 | Plan individual: ¿canal propio de Aequus o solo continuidad de corporativo? ¿Lead que cotiza y no compra: qué flujo? | B3.2/B3.3 | Embudo del cotizador; origen de afiliación individual |
| G11 | Estrategia de cobro al corporativo moroso ("la otra estrategia") | B3.6 | RN-FAC-06; jobs de cobranza |
| G12 | SLA de llegada de carroza | E04 B5.9 | RN-URG-07; promesa en cotizador |
| G13 | Dos funerarias en la misma ciudad: ¿quién decide asignación, operador o regla automática? | E04 B9.8 | RN-RED-06; algoritmo de asignación |
| G14 | Ciudad sin cobertura (foráneo): ¿fallback? ¿Se muestra red aliada con sobreprecio? | E04 B9.2/B9.9 | RN-RED-08; flujo de excepción en cotizador |
| G15 | Marca blanca en B2B: ¿se revela la funeraria prestadora al demandante? | B11.2 | RN-B2B-04; visibilidad en consola B2B |
| G16 | Matriz de notificaciones: ¿qué evento va por WhatsApp y cuál por correo? ¿Toda la familia ve el servicio o un solo usuario? | B13.2/B13.3 | RN-SRV-08; `channel_matrix`; acceso familiar |
| G17 | Volúmenes esperados: servicios/mes por funeraria, afiliados por convenio, usuarios concurrentes | B13.5 | Dimensionamiento RNF-8; pruebas de carga 3.17 |
| G18 | Prepagado: qué se prepaga, congelamiento de precio, vigencia, muerte en otra ciudad | B7 | RN-URG-08; variante PREPAGADO completa |
| G19 | ¿A quién más entrevisto? (dueño de funeraria mediana, cementerio, aseguradora) | B14.3 | Validación de flujos del proveedor (semana 3) |

### A.3 Para E04 o semana 3 (spec del agente)

| # | Pregunta | Ref. | Qué bloquea |
| :-: | :---- | :---- | :---- |
| G20 | Momento exacto IA→humano, frases típicas del doliente, lo que la IA no dice jamás, tareas delegadas día 1 vs jamás | B8 | Spec fino del guardrail y del grafo (actividad 3.x del agente) |

---

## B. Compromisos de Andrés (con fecha)

| # | Compromiso | Fecha | Estado | Para qué se necesita |
| :-: | :---- | :---- | :-: | :---- |
| C1 | Contrato + carnet que genera hoy (ejemplo real) | **mié 9-sep** | ⏳ | RF-DOC-01/02: plantillas de contrato PDF y carnet QR |
| C2 | Factura del fondo + soporte de pago real | **mié 9-sep** | ⏳ | F2: prefactura cruzada y validación de facturas |
| C3 | Validación de requisitos v1.0 (firma en vivo) | **vie 11-sep (demo)** | ⏳ | Este paquete pasa a v1.0; desbloquea semana 3 |
| C4 | Nombres de clientes y funerarias del pipeline (para piloto) | **vie 11-sep** | ⏳ | Plan de pilotos H3–H4; condición de pago del H4 |
| C5 | Decisión de cementerios (G1) | **vie 11-sep** | ⏳ | RN-RED-07 |
| C6 | Decisión de pasarela + apertura de cuenta + solicitud de API (G2) | **vie 11-sep** (decisión) | ⏳ | Pagos semanas 5–6; sandbox de dispersión |
| C7 | Respuesta de la contadora (G3) | **vie 11-sep** | ⏳ | RN-PAG-08; modelo contable |
| C8 | Lista inicial de ~10 funerarias para curaduría | semana 3 | ⏳ | Onboarding de red (H2); condición H4 (5 intermedias/grandes al 27-nov) |
| C9 | 2 reuniones con funerarios, grabadas, para levantar flujo operativo real | semana 3 | ⏳ | Validar F5 (tracking) y la consola del proveedor con usuarios reales |
| C10 | Términos y condiciones de la plataforma (con abogado) | semana 3–4 | ⏳ | RN-AS-02 (reglas anti-salto contractuales), RN-AF-06 (declaración de responsabilidad), habeas data 18-sep |
| C11 | Pantallazos del JotForm actual (afiliación) | semana 2 | ⏳ | RF-DOC-03: replicar firma simple + campos actuales |
| C12 | Documentos al Drive: cotizaciones reales, propuesta de Pisco, reporte de siniestralidad, formulario de afiliación, su propio mapeo de procesos | semana 2 | ⏳ | Cotizador paramétrico con datos reales; benchmark Pisco |
| C13 | Directorio nacional de funerarias (Excel ya levantado) | semana 2–3 | ⏳ | Seed de `network`; prospección de curaduría |

---

## C. Decisiones técnicas de Jefferson (no requieren a Andrés)

| # | Decisión | Estado | Notas |
| :-: | :---- | :-: | :---- |
| D1 | Monolito modular vs microservicios | ✅ ADR-001 | Monolito modular |
| D2 | SSE vs WebSockets | ✅ ADR-004/09 | SSE + fallback polling |
| D3 | Outbox en Postgres vs broker | ✅ ADR-008 | Outbox; migrable |
| D4 | Adaptador de pasarela (Wompi/ePayco/Bold intercambiables) | ✅ ADR-006 | Desacopla G2 |
| D5 | Estructura de repos y convenciones (heredada) | ✅ | `aequus-api`, `aequus-web`, `aequus-agent` |
| D6 | Rehacer scaffolds `at-web`/`at-api` alineados a esta arquitectura | ⏳ | Después de la validación del viernes (los actuales están vacíos y no reflejan nada — se regeneran como `aequus-web`/`aequus-api`) |
| D7 | Actualizar Plan Técnico v1.1 → v1.2 con este paquete | ⏳ | Después de la validación del viernes |
| D8 | Proveedor de email primario (ACS vs Resend) | 🟡 | ACS primario; se confirma con prueba de entregabilidad en H2 |
| D9 | Hosting web: Static Web Apps vs Container Apps | 🟡 | Container Apps por uniformidad; SWA si el SSR no se justifica |
| D10 | Estrategia de seeds y datos sintéticos para staging | ⏳ | Semana 3, con el directorio de Andrés (C13) |
| D11 | Base de datos: **Neon** (PostgreSQL 16 serverless) | ✅ ADR-003 | Decidido 8-sep; branches para dev/staging, pgvector, PITR |
| D12 | WhatsApp vía **Kapso**: alertas por plantillas + agente conversacional en esta fase | ✅ ADR-007/008 | Decidido 8-sep; adelanta Plan §4.2 |

---

## D. Riesgos si no se cierran a tiempo

| Gap/compromiso | Si no se cierra para el… | Impacto en cronograma |
| :---- | :---- | :---- |
| G2/C6 pasarela | semana 5 | Pagos se construye contra sandbox propio; riesgo de retrabajo en webhooks y dispersión (mitigado por adaptador D4) |
| G3/C7 contadora | semana 5 | El motor de liquidación modela la comisión con parámetro configurable; la facturación de la comisión queda manual |
| G1/C5 cementerios | semana 4 | Se construye solo `PROVIDER_MANAGED`; enchufar cementerios después cuesta una migración menor (ya prevista) |
| C1/C2 artefactos | semana 3 | Plantillas de contrato/carnet se diseñan con supuestos; retrabajo en documentos (2.x) |
| C3 firma v1.0 | vie 11-sep | Sin requisitos firmados, todo lo construido en semanas 3–4 queda expuesto a cambio sin control (Plan §10) |
| C8/C9 red y reuniones | semana 3–4 | La consola de proveedor se diseña sin usuarios reales; riesgo de adopción (el riesgo #1 del proyecto, E03) |
| G17 volúmenes | semana 6 | Pruebas de carga (3.17) con supuestos; dimensionamiento prod por rangos |
