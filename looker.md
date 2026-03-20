## Comparación de opciones Looker para el caso de colegios

| Opción | ¿Se puede embeber en su app? | ¿Se puede aislar por colegio? | Cómo encaja con el flujo `login -> backend -> school_id -> gráfica` | Costo aprox con 2 analistas | Qué tan bien se acopla |
|---|---|---|---|---|---|
| **Looker Studio** | Sí, por embed/iframe. | Sí, pero más apoyándose en la seguridad de BigQuery y configuración del reporte. | Se puede, pero queda menos natural para un SaaS multi-colegio porque no está tan orientado a signed embedding ni a pasar atributos desde backend. | **$0/mes** de licencias + BigQuery | **Bajo-medio** |
| **Looker Studio Pro** | Sí. | Sí, igual que Studio, con mejor administración y soporte. | Bueno para piloto/MVP, pero sigue sin ser el más natural para el patrón multi-tenant por `school_id`. | **$18/mes** por 2 analistas + BigQuery | **Medio** |
| **Looker (Google Cloud core)** | Sí. Además incluye embedding dentro de la plataforma. | Sí. Tiene user attributes y controles más fuertes. | Ya se parece más a lo que quieren, pero la edición pensada para este caso es Embed. | **Cotización**; sin tarifa pública fija | **Alto** |
| <span style="background-color:#d1fae5"><strong>Looker Embed</strong></span> | <span style="background-color:#d1fae5"><strong>Sí, es el mejor para eso. Soporta signed embedding.</strong></span> | <span style="background-color:#d1fae5"><strong>Sí, claramente. Puedes pasar user attributes como `school_id`.</strong></span> | <span style="background-color:#d1fae5"><strong>Es el que más se acopla al flujo exacto.</strong></span> | <span style="background-color:#d1fae5"><strong>Cotización</strong></span> | <span style="background-color:#d1fae5"><strong>Muy alto / el mejor</strong></span> |

## Diferencias más importantes para este caso

| Punto clave | Looker Studio / Pro | Looker Embed |
|---|---|---|
| Embebido simple | Sí | Sí |
| Embebido firmado desde backend | No es su fortaleza principal | **Sí**, con **signed embedding** |
| Pasar `school_id` desde backend | Más forzado / indirecto | **Sí**, con **user attributes** |
| Multi-colegio tipo SaaS | Posible, pero menos natural | **Sí, diseñado mucho mejor para eso** |
| Precio público visible | Sí. Gratis o **$9/usuario/proyecto/mes** | No. **Cotización** |

## Costos aproximados que sí se pueden afirmar

- **Looker Studio:** **$0/mes** de licencias  
- **Looker Studio Pro:** **$18/mes** para 2 analistas  
- **BigQuery:** va aparte en cualquier escenario  
- **Looker / Looker Embed:** **sin precio público fijo**, requiere cotización  
