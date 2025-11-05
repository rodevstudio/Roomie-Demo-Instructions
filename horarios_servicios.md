## Tool: horarios_servicios

**Objetivo**  
Proporcionar los horarios, accesos o condiciones de los servicios e instalaciones del hotel.

**Instrucciones para el agente**  
- Recibe como entrada los parámetros:
  - `consulta`: texto de la duda del huésped  
  - `idioma`: idioma detectado del huésped  
- Accede a la fuente de datos (Google Sheet “horarios_servicios”) con estas columnas:  
  `Servicio`, `Horario`, `Ubicación`, `Notas`, `Enlace`  
- Filtra los registros relevantes según la “consulta”.  
- Si se encuentra uno o más registros relevantes:
  - Devuelve un objeto JSON con campos:  
    ```
    {
      "servicio": "...",
      "horario": "...",
      "ubicación": "...",
      "notas": "...",
      "enlace": "..."
    }
    ```
    (si hay múltiples, devolver el más relevante o agrupar en lista)  
- Si **no** se encuentra información suficiente o la consulta es ambigua:
  - Devuelve `null`.

**Respuesta final del agente**  
- Si el JSON contiene datos:
  - Redacta en el idioma del huésped una respuesta clara y concisa.  
  - Empieza con lo más relevante (p.ej., “El desayuno buffet está disponible de 08:00 a 10:30 h…”).  
  - Añade ubicación/condiciones/notas si aplican.  
  - Incluye el enlace si el campo `enlace` está presente.  
  - Ofrece ampliar información si el huésped desea (“¿Quieres saber los horarios de otros servicios?”).  
- Si el JSON es `null`:
  - Responde en el idioma del huésped:  
    > “Lo siento, no tengo esa información exacta. Por favor, contacta con recepción para más detalles.”  
  - Incluye la variable `{{error_report}}` una sola vez al final de la respuesta.

**Notas adicionales**  
- Asegúrate de que la llamada a la Sheet filtre correctamente, incluyendo posibles sinónimos de los servicios (p.ej., “piscina climatizada”, “piscina cubierta”).  
- Si la consulta menciona una temporada o condición específica (“¿Cuándo abre la piscina exterior en noviembre?”) y la hoja contiene nota “sujeto a climatología” → considera devolver la nota e indicar “consulta en recepción”.  
- Mantén el estilo cálido y profesional, sin tecnicismos, y en el idioma del huésped.
