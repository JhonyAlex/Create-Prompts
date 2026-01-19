# 🧪 Test de Desambiguación Geográfica

## Objetivo del Test

Validar que la IA **NUNCA asuma automáticamente** que una zona mencionada por el cliente es la ubicación del proyecto, sala de ventas o dirección oficial.

---

## 🔴 Casos de Prueba Críticos

### Test 1: Mención Ambigua de Zona (San Catalina)

**Input del Cliente:**
```
"Hola, me interesa San Catalina"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"¡Perfecto! El Edificio Lumina está ubicado en San Catalina. Contamos con departamentos de 2 y 3 dormitorios..."
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Hola, soy Sofía, tu asesora virtual de El Ágora

Te comparto el enlace de nuestro brochure:
https://heyzine.com/flip-book/2ed6f324de.html

Solo para confirmar:
¿San Catalina es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"
```

---

### Test 2: Mención de Otro Distrito (Miraflores)

**Input del Cliente:**
```
"Miraflores"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"Sí, estamos en Miraflores. ¿Qué tipo de departamento buscas?"
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Perfecto 😊
Solo para confirmar:
¿Miraflores es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"
```

---

### Test 3: Mención de Zona en Contexto de Búsqueda

**Input del Cliente:**
```
"Busco departamento en Surco"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"¡Excelente! Nuestro proyecto está en Surco. Tenemos departamentos de 2 y 3 dormitorios..."
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Entendido. Solo para confirmar:
¿Surco es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta de nuestro proyecto?"
```

---

### Test 4: Pregunta Explícita por Ubicación del Proyecto

**Input del Cliente:**
```
"¿Dónde está ubicado el proyecto?"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"El proyecto está en [zona inventada o asumida]"
```

**✅ Respuesta CORRECTA (APROBADO):**
```
[USA preguntas_frecuentes: ubicación del proyecto]
"[Respuesta según herramienta con ubicación oficial exacta]"
```

---

### Test 5: Pregunta por Sala de Ventas

**Input del Cliente:**
```
"¿Dónde queda la sala de ventas?"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"La sala de ventas está en [zona mencionada anteriormente por el cliente]"
```

**✅ Respuesta CORRECTA (APROBADO):**
```
[USA preguntas_frecuentes: ubicación sala de ventas]
"[Respuesta según herramienta con dirección oficial exacta]"
```

---

### Test 6: Confirmación Tras Aclaración (Zona de Interés)

**Conversación:**
```
Cliente: "San Catalina"

Sofía: "Solo para confirmar:
¿San Catalina es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"

Cliente: "Es donde me gustaría vivir"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"Perfecto, el proyecto está en San Catalina. ¿Qué tipo de departamento buscas?"
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Entendido. ¿Qué tipo de departamento es de tu interés?"
```

---

### Test 7: Confirmación Tras Aclaración (Ubicación del Proyecto)

**Conversación:**
```
Cliente: "Miraflores"

Sofía: "Solo para confirmar:
¿Miraflores es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"

Cliente: "Quiero saber dónde está el proyecto"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"El proyecto está en Miraflores, en la Av. [inventada]..."
```

**✅ Respuesta CORRECTA (APROBADO):**
```
[USA preguntas_frecuentes: ubicación del proyecto]
"[Respuesta según herramienta con ubicación oficial]"
```

---

### Test 8: Múltiples Zonas Mencionadas

**Input del Cliente:**
```
"Me interesa La Molina o San Borja"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"Perfecto, estamos en La Molina. ¿Qué tipo de departamento buscas?"
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Perfecto 😊
Solo para confirmar:
¿La Molina o San Borja son las zonas donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"
```

---

### Test 9: Zona + Tipo de Departamento

**Input del Cliente:**
```
"Busco depa de 3 dormitorios en Jesús María"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"¡Excelente! Tenemos departamentos de 3 dormitorios en Jesús María. Los precios son..."
```

**✅ Respuesta CORRECTA (APROBADO):**
```
"Perfecto, tenemos departamentos de 3 dormitorios.

Solo para confirmar:
¿Jesús María es la zona donde te gustaría vivir, o deseas conocer la ubicación exacta del proyecto?"
```

---

### Test 10: Referencia Indirecta a Ubicación

**Input del Cliente:**
```
"¿Están cerca del Óvalo Gutiérrez?"
```

**❌ Respuesta INCORRECTA (FALLO):**
```
"Sí, estamos muy cerca del Óvalo Gutiérrez, a 5 minutos..."
```

**✅ Respuesta CORRECTA (APROBADO):**
```
[USA preguntas_frecuentes: ubicación del proyecto]
"[Respuesta según herramienta con ubicación oficial, luego calcular distancia si es posible]"
```

---

## 📊 Criterios de Aprobación

Para que el agente **APRUEBE** este test, debe:

✅ **NUNCA afirmar ubicaciones** sin validar intención del cliente
✅ **SIEMPRE preguntar** cuando hay ambigüedad geográfica
✅ **USAR herramientas** (`preguntas_frecuentes`) para ubicaciones oficiales
✅ **DISTINGUIR** entre zona de interés del cliente vs ubicación del proyecto
✅ **NO asumir** que una zona mencionada = ubicación del proyecto

---

## 🔴 Señales de Fallo Crítico

El agente **FALLA** si:

❌ Afirma ubicación del proyecto basándose en lo que dice el cliente
❌ Confirma que "está en [zona]" sin consultar herramienta
❌ Menciona direcciones inventadas o asumidas
❌ No pregunta ante ambigüedad geográfica
❌ Intercambia "zona de interés" con "ubicación del proyecto"

---

## 🎯 Recomendación de Uso

1. **Ejecutar estos 10 tests** antes de poner el agente en producción
2. **Validar cada respuesta** contra los criterios de aprobación
3. **Documentar cualquier fallo** y ajustar el prompt
4. **Re-testear** hasta que todos los casos pasen
5. **Monitorear conversaciones reales** para detectar nuevos casos edge

---

## 📝 Registro de Pruebas

| Test | Fecha | Resultado | Observaciones |
|------|-------|-----------|---------------|
| Test 1 | | ⬜ PENDIENTE | |
| Test 2 | | ⬜ PENDIENTE | |
| Test 3 | | ⬜ PENDIENTE | |
| Test 4 | | ⬜ PENDIENTE | |
| Test 5 | | ⬜ PENDIENTE | |
| Test 6 | | ⬜ PENDIENTE | |
| Test 7 | | ⬜ PENDIENTE | |
| Test 8 | | ⬜ PENDIENTE | |
| Test 9 | | ⬜ PENDIENTE | |
| Test 10 | | ⬜ PENDIENTE | |

**Leyenda:**
- ⬜ PENDIENTE
- ✅ APROBADO
- ❌ FALLO

---

## 🚀 Próximos Pasos

Una vez que todos los tests pasen:

1. Implementar el prompt actualizado en el agente de producción
2. Monitorear las primeras 50 conversaciones reales
3. Identificar nuevos casos edge no contemplados
4. Actualizar este documento con nuevos tests si es necesario
5. Crear alertas automáticas para detectar frases prohibidas
