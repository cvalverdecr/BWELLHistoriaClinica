# Herramienta local BWell - Historia clínica

## Uso
1. Abra `bwell_herramienta_local.html` directamente en Chrome, Edge o Firefox.
2. Complete la hoja requerida.
3. Marque la confirmación de base legal/consentimiento.
4. Use **Calcular resultado** para ver la regla automática.
5. Use **Crear PDF / imprimir** y elija **Guardar como PDF** en el diálogo del navegador.
6. Use **Limpiar campos** al finalizar.

## Privacidad aplicada
- No hay formulario HTML con submit tradicional.
- No se usa `localStorage` ni `sessionStorage`.
- No se incluye telemetría ni librerías externas.
- La política CSP bloquea conexiones de red con `connect-src 'none'` y `form-action 'none'`.
- El JavaScript deshabilita `fetch`, `XMLHttpRequest` y `sendBeacon`.
- El payload mínimo opcional contiene únicamente: versión, herramienta, hoja, resultado, códigos de regla y fecha local. No contiene datos personales, respuestas clínicas ni texto libre.

## Macros del Excel revisadas
- Crear PDF: exportaba la hoja activa como PDF y sugería nombre con hoja + paciente + fecha/hora.
- Limpiar campos: limpiaba celdas no bloqueadas y respetaba fórmulas.
- Construir diagnóstico/criterio:
  - Cardiometabólico: lista diagnósticos confirmados y calcula candidato según reglas de la hoja.
  - Gastro: signos de alarma impiden ingreso y derivan a gastroenterología; síntomas, tratamiento y calidad de vida activan candidato.
  - Clínica Articular y Espalda: banderas neurológicas impiden ingreso; diagnósticos/antecedentes seleccionados activan candidato.

## Nota legal operativa
Esta herramienta no sustituye revisión legal. Al capturar datos de salud, valide con su responsable de privacidad/compliance la base legal, consentimiento informado, aviso de privacidad, custodia del PDF generado, retención, acceso, auditoría y eliminación segura.
