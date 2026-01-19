{
  "nodes": [
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "6e588e01-b03c-4311-9354-03b56f77436b",
                    "leftValue": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][text]'] }}",
                    "rightValue": "[empty]",
                    "operator": {
                      "type": "string",
                      "operation": "notEmpty",
                      "singleValue": true
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "text"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "e04bbf02-0fbf-4b50-9aa5-f29eb8caaea2",
                    "leftValue": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][attachment][type]'] }}",
                    "rightValue": "voice",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "audio"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 2
                },
                "conditions": [
                  {
                    "id": "9bdf6dd9-f60e-4d6f-ba34-34bad3308b3b",
                    "leftValue": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][attachment][type]'] }}",
                    "rightValue": "picture",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "image"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.2,
      "position": [
        -672,
        80
      ],
      "id": "1a6e69e5-c267-4aff-958c-209f4cd37cea",
      "name": "Elegir entre tipo de mensaje",
      "alwaysOutputData": false
    },
    {
      "parameters": {
        "resource": "audio",
        "operation": "transcribe",
        "binaryPropertyName": "=data",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1.8,
      "position": [
        -224,
        192
      ],
      "id": "c61e3388-a66d-4ccf-a8e1-a628948b686a",
      "name": "Transcripción del audio",
      "credentials": {
        "openAiApi": {
          "id": "gnA9sKIaU7C3fuxn",
          "name": "openai-api-el-agora"
        }
      }
    },
    {
      "parameters": {
        "url": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][attachment][link]'] }}",
        "options": {
          "response": {
            "response": {
              "responseFormat": "file"
            }
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -448,
        192
      ],
      "id": "8f25ef3c-23d7-404b-beec-bf0c23da956e",
      "name": "Descargar audio"
    },
    {
      "parameters": {
        "numberInputs": 3
      },
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        224,
        176
      ],
      "id": "f2cbd47c-a78e-45d8-94b9-a9052e613ddf",
      "name": "Merge"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "loose",
            "version": 2
          },
          "conditions": [
            {
              "id": "dd45ef77-1532-4771-8f8b-a6c207ce061d",
              "leftValue": "={{ \n  $json.custom_fields_values.find(\n    f => f.field_name === \"Activar IA\"\n  )?.values?.[0]?.value === true \n}}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "looseTypeValidation": true,
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -896,
        304
      ],
      "id": "e4417803-be14-409c-ac52-72a95a4ba4ab",
      "name": "¿Está el campo personalizado 'Atiende IA' activo?"
    },
    {
      "parameters": {
        "url": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][attachment][link]'] }}",
        "options": {
          "response": {
            "response": {
              "responseFormat": "file"
            }
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -448,
        384
      ],
      "id": "0dbf6ca7-6743-497a-b65c-8aa91bf46fdb",
      "name": "Descargar imagen"
    },
    {
      "parameters": {
        "jsCode": "const msg = $input.first().json.message;\n\nreturn [\n  {\n    json: {\n      formattedResponse: msg,\n    },\n  },\n];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        448,
        192
      ],
      "id": "903ae373-0f1b-4d35-9d4f-23ab935d2564",
      "name": "Formatear mensaje"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "b734b63d-56cc-4444-be74-ce02482b1a34",
              "name": "message",
              "value": "={{ $('Traer datos de Kommo').item.json.body['message[add][0][text]'] }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        0,
        0
      ],
      "id": "f70d4d94-f5ef-42fb-9342-638c316c689b",
      "name": "Formatear Texto"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "0e3f0780-ded2-45fc-b238-c4296ba57839",
              "name": "message",
              "value": "={{ $json.text }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        0,
        192
      ],
      "id": "e8099f29-4abb-437d-bdb1-c5a60c211f19",
      "name": "Formatear Texto1"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "0e3f0780-ded2-45fc-b238-c4296ba57839",
              "name": "message",
              "value": "=El usuario comparte lo siguiente: {{ $json.content }}\nJunto con la imagen el usuario dice esto: '{{ $('Traer datos de Kommo').item.json.body['message[add][0][text]'] }}'",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        0,
        384
      ],
      "id": "3808a6d5-f24c-4877-86cb-cd2386c5b7da",
      "name": "Formatear Texto2"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "loose",
            "version": 2
          },
          "conditions": [
            {
              "id": "5b3d05bc-1377-487d-ba87-7372b09f56ab",
              "leftValue": "={{ $json.custom_fields_values }}",
              "rightValue": "=null",
              "operator": {
                "type": "string",
                "operation": "exists",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "looseTypeValidation": true,
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -1120,
        400
      ],
      "id": "e6d5cda4-a555-4877-a794-e7454a9f96fc",
      "name": "¿Llegaron los campos personalizados?"
    },
    {
      "parameters": {
        "description": "Utiliza la herramienta para pensar en algo. No obtendrás nueva información ni modificarás la base de datos, sino que simplemente añadirá el pensamiento al registro. Úsala cuando se requiera un razonamiento complejo o algo de memoria caché."
      },
      "type": "@n8n/n8n-nodes-langchain.toolThink",
      "typeVersion": 1.1,
      "position": [
        928,
        416
      ],
      "id": "d596d14d-d6f3-4021-9d7d-e39b308e503b",
      "name": "Think"
    },
    {
      "parameters": {
        "method": "PATCH",
        "url": "=https://braulioga73.kommo.com/api/v4/leads/{{ $('Traer datos de Kommo').item.json.body['message[add][0][element_id]'] }}",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ \n  JSON.stringify({\n    custom_fields_values: [\n      { \n        field_id: 1024186, \n        values: [\n          { enum_id: 849320 }\n        ] \n      }\n    ]\n  }) \n}}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -672,
        640
      ],
      "id": "de400295-7956-49f5-a738-ac4cb9310c8b",
      "name": "Manda un campo personalizado nuevo",
      "credentials": {
        "oAuth2Api": {
          "id": "NTurPpO2eJSYQWsj",
          "name": "kommo-oauth2-el-agora"
        }
      }
    },
    {
      "parameters": {
        "url": "=https://braulioga73.kommo.com/api/v4/leads/{{ $('Traer datos de Kommo').item.json.body['message[add][0][entity_id]'] }}",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "options": {
          "response": {
            "response": {
              "responseFormat": "json"
            }
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -1344,
        400
      ],
      "id": "6bf1cc40-6845-42e3-a8e8-eb0fe012ab3e",
      "name": "Traer todos los datos del lead",
      "credentials": {
        "oAuth2Api": {
          "id": "NTurPpO2eJSYQWsj",
          "name": "kommo-oauth2-el-agora"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://braulioga73.kommo.com/api/v2/salesbot/run/",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "=[\n  {\n    \"bot_id\": 64882,\n    \"entity_id\": {{ $('Traer datos de Kommo').item.json.body['message[add][0][entity_id]'] }},\n    \"entity_type\": \"2\"\n  }\n] ",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        2128,
        192
      ],
      "id": "52b7bc37-71a3-4f7a-9055-ef9e68200443",
      "name": "Activar el bot",
      "credentials": {
        "oAuth2Api": {
          "id": "NTurPpO2eJSYQWsj",
          "name": "kommo-oauth2-el-agora"
        }
      }
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -672,
        304
      ],
      "id": "92141739-54f0-4121-9e6b-1b2e3eeb2f9f",
      "name": "No Operation, do nothing"
    },
    {
      "parameters": {
        "method": "PATCH",
        "url": "=https://braulioga73.kommo.com/api/v4/leads/{{ $('Traer datos de Kommo').item.json.body['message[add][0][element_id]'] }}",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Content-Type",
              "value": "application/json; charset=utf-8"
            },
            {
              "name": "Accept",
              "value": "application/json"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ JSON.stringify({\n  custom_fields_values: [\n    { field_id: 1020428, values: [{ value: $json.mensaje_limpio }] }\n  ]\n}) }}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        1904,
        192
      ],
      "id": "6f9a8546-249f-4703-bb37-4ed470150260",
      "name": "Enviar mensaje a Kommo",
      "credentials": {
        "oAuth2Api": {
          "id": "NTurPpO2eJSYQWsj",
          "name": "kommo-oauth2-el-agora"
        }
      }
    },
    {
      "parameters": {
        "resource": "image",
        "operation": "analyze",
        "modelId": {
          "__rl": true,
          "value": "gpt-4o-mini",
          "mode": "list",
          "cachedResultName": "GPT-4O-MINI"
        },
        "text": "=Analiza la imagen",
        "inputType": "base64",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 1.8,
      "position": [
        -224,
        384
      ],
      "id": "fd9f5c5b-aedc-4e35-9839-cef8ee702a4f",
      "name": "Análisis de imagen",
      "credentials": {
        "openAiApi": {
          "id": "gnA9sKIaU7C3fuxn",
          "name": "openai-api-el-agora"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "let s = $('AI Agent').item.json.output ?? '';\ns = s.replace(/\\r\\n/g, '\\n').replace(/\\r/g, '\\n');   // normaliza a \\n\n// Convierte listas\ns = s.replace(/^\\s*[-*]\\s+/gm, '• ')\n     .replace(/^\\s*(\\d+)\\.\\s+/gm, (_, n) => `${n}) `);\n// CRLF y párrafos (doble salto)\ns = s.replace(/\\n/g, '\\r\\n').replace(/\\r\\n{3,}/g, '\\r\\n\\r\\n');\nreturn [{ json: { mensaje_limpio: s } }];\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1680,
        192
      ],
      "id": "900e5b98-f0d4-46cf-b4de-3308e1d3090e",
      "name": "Formatear mensaje para envío"
    },
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "94fb17c8-f4da-4d0d-8b36-971df659048f",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2.1,
      "position": [
        -1792,
        464
      ],
      "id": "2a002aa9-9916-4e82-b702-4b0d93663168",
      "name": "Traer datos de Kommo",
      "webhookId": "94fb17c8-f4da-4d0d-8b36-971df659048f"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.formattedResponse }}",
        "options": {
          "systemMessage": "=## ROL Y OBJETIVO\n\nEres una **Asesora virtual de Ventas de EL ÁGORA**, te llamas Sofía, y estás especializada en bienes raíces con enfoque consultivo. Tu personalidad es cálida, empática y estratégica. Tu objetivo es **resolver las preguntas del cliente** y presentar opciones relevantes del EDIFICIO LUMINA.\n\n**FECHA Y HORA ACTUAL**: {{ $now.format('yyyy-MM-dd HH:MM')}}\n\n------\n\n## REGLAS DE COMUNICACIÓN CRÍTICAS\n\nEstilo de escritura:\n\n- Respuestas breves: máximo 2-3 oraciones por párrafo\n- Sin emojis, mantener tono amigable y profesional\n- Usar viñetas para listas, no párrafos largos\n\nComportamiento clave:\n\n- No repetir la misma respuesta dos veces\n- Agrupar departamentos con características idénticas en formato resumido\n\n------\n\n## SOP (Procedimientos Operativos Estándar)\n\n### ETAPA 1: CONEXIÓN INICIAL\n\n- Saludo cordial y presentación personal\n- Enviar inmediatamente el brochure: \"Te comparto el enlace de nuestro brochure donde podrás visualizar nuestras diversas tipologías: https://heyzine.com/flip-book/2ed6f324de.html\"\n- Pregunta abierta inicial: \"¿Qué tipo de departamento es de tu interés?\"\n- **IMPORTANTE:** Solo ofrecemos departamentos de 2 y 3 dormitorios (no existen de 1 dormitorio)\n\n### ETAPA 2: DESCUBRIMIENTO DE NECESIDADES\n\n- Escuchar activamente las respuestas del cliente\n- Si la respuesta es CLARA (2 dormitorios, 3 dormitorios, precio específico):\n  → Proceder a Etapa 3\n\n- Si la respuesta es AMBIGUA (nombre de zona, característica no clara, respuesta de una sola palabra):\n  → Pedir aclaración: \"¿Te refieres a [interpretación A] o [interpretación B]?\"\n  → NUNCA asumir interpretación sin confirmar\n  → Validar contra herramientas antes de hacer cualquier afirmación\n\n- Hacer preguntas estratégicas para entender:\n  - Número de dormitorios deseados (solo 2 o 3)\n  - Urgencia de entrega\n  - Características específicas que valora (ubicación, etc.)\n\n### ETAPA 3: PRESENTACIÓN DE OPCIONES\n\n- Usar `inventario_de_departamentos` para mostrar opciones relevantes\n- Resaltar características según lo descubierto en Etapa 2\n\n### ETAPA 4: PROFUNDIZACIÓN\n\n- Responder preguntas específicas usando `preguntas_frecuentes`\n- Abordar inquietudes sobre acabados, ubicación, etc.\n\n### ETAPA 5: COORDINACIÓN DE VISITA\n\n- Proponer visita cuando el cliente muestre interés: \"¿Qué día y hora te vendría bien para visitarnos?\"\n- Usar `gestor_de_citas` para verificar disponibilidad y confirmar\n\n**Notas clave SOP:**\n\n- No repetir saludos tras el mensaje inicial\n- No preguntar explícitamente cantidad de dormitorios en el saludo inicial\n\n------\n\n## PREGUNTAS ESTRATÉGICAS POR SITUACIÓN\n\n- **Primer contacto:** \"¿Qué tipo de departamento es de tu interés?\"\n- **Urgencia/entrega:** \"¿Deseas una entrega inmediata?\" → Si responde sí, mencionar Nazarenas\n\n------\n\n## HERRAMIENTAS DISPONIBLES\n\n### 1. preguntas_frecuentes\n\n**Cuándo usarla:**\n\n- **DIRECCIÓN Y UBICACIÓN DEL PROYECTO** (SIEMPRE consultar antes de derivar)\n- Características generales del proyecto (ubicación, seguridad)\n- Cronograma de construcción o fecha de entrega\n- Proceso de separación o requisitos para compra\n- Materiales de construcción, acabados o especificaciones técnicas\n- Datos de contacto, dirección de sala de ventas\n- Áreas comunes, ascensor, estacionamientos\n- Banco o financiamiento\n\n**Ejemplos:**\n\n- \"¿Cuál es la dirección?\" → `preguntas_frecuentes`\n- \"¿Dónde está el edificio?\" → `preguntas_frecuentes`\n- \"¿Qué áreas comunes tiene el edificio?\" → `preguntas_frecuentes`\n- \"¿Cuándo entregan el proyecto?\" → `preguntas_frecuentes`\n- \"¿Dónde está ubicada la sala de ventas?\" → `preguntas_frecuentes`\n\n***\n\n### 2. inventario_de_departamentos\n\n**Cuándo usarla:**\n\n- Departamentos con características específicas (dormitorios, baños, área)\n- Información de precios o rangos de precios\n- Comparación de tipologías o distribuciones\n- Disponibilidad por piso\n- Opciones dentro de presupuesto determinado\n- Dimensiones exactas o características particulares\n\n**Ejemplos:**\n\n- \"¿Qué departamentos de 3 dormitorios tienen?\" → `inventario_de_departamentos`\n- \"¿Cuánto cuesta el depa del piso 5?\" → `inventario_de_departamentos`\n- \"¿Qué opciones hay entre precio1 y precio2\" → `inventario_de_departamentos`\n\n**FORMATO DE PRESENTACIÓN:**\n\n✅ **CORRECTO** (agrupado con variables):\n\n```\nPisos [lista_pisos] - [tipología] [área_m2] - [dormitorios] dorm - [baños] baños - S/[precio]\n```\n\n❌ **INCORRECTO** (separado con datos específicos):\n\n```\nPiso 2 - Flat 102m² - 3 dorm - 2 baños - S/232,327\nPiso 3 - Flat 102m² - 3 dorm - 2 baños - S/232,327\n```\n\n**Reglas de formato:**\n\n- Agrupar por características idénticas\n- Usar variables genéricas: [lista_pisos], [tipología], [área_m2], [dormitorios], [baños], [precio]\n- Usar comas para separar pisos: \"Pisos 2, 3 y 4\"\n- Máximo 3-4 opciones por respuesta\n- Usar siempre sol peruano (S/)\n\n***\n\n### 3. gestor_de_citas\n\n**Cuándo usar:**\n\n- Verificar disponibilidad antes de confirmar cita al cliente\n- Crear cita después de confirmación del cliente\n- Modificar o cancelar cita\n- Consultar detalles de cita existente\n\n**Cómo funciona:**\n\n1. Envías solicitud con parámetros (fecha, hora, acción)\n2. El gestor ejecuta operación en calendario\n3. Recibes reporte (exitoso/fallido) y próximo paso\n\n**Validaciones automáticas:**\n\n- Horario operativo: Lunes-Sábado, 10:00 AM - 5:00 PM\n- Disponibilidad sin conflictos\n- Formato de hora correcto\n- Duración de 30 minutos\n\n**IMPORTANTE:** El gestor solo reporta a ti, nunca interactúa con el cliente. Tú decides qué comunicar según el reporte recibido.\n\n------\n\n## LÓGICA DE USO DE HERRAMIENTAS\n\n**Información general:**\n\n```\nCliente: \"¿El edificio tiene gimnasio?\"\n→ Usar: preguntas_frecuentes\n```\n\n**Inventario y precios:**\n\n```\nCliente: \"¿Cuánto cuesta un departamento de 2 dormitorios?\"\n→ Usar: inventario_de_departamentos\n```\n\n**Gestión de citas:**\n\n```\nCliente: \"Quiero visitarlos el sábado a las 11 AM\"\n→ Usar gestor_de_citas (verificar disponibilidad)\n→ Esperar reporte\n→ Confirmar o proponer alternativa\n```\n\n**Consultas combinadas:**\n\n```\nCliente: \"¿Qué depas de 3 dormitorios tienen y cuánto cuestan?\"\n→ Usar inventario_de_departamentos\n→ Presentar opciones\n→ Si pregunta detalles: usar preguntas_frecuentes\n```\n\n------\n\n## MANEJO DE INFORMACIÓN NO DISPONIBLE\n\n**IMPORTANTE:** Antes de derivar al asesor, SIEMPRE debes consultar las herramientas disponibles.\n\n**Preguntas que SIEMPRE debes responder con las herramientas:**\n- Dirección/ubicación del edificio → `preguntas_frecuentes`\n- Características del proyecto → `preguntas_frecuentes`\n- Precios y disponibilidad → `inventario_de_departamentos`\n- Coordinar visitas → `gestor_de_citas`\n\n**SOLO si después de consultar las herramientas NO encuentras la información:**\n\n**Respuesta:** \"Pronto un asesor especializado se conectará para darte respuesta ante esa pregunta\"\n\n**Recuerda:** No derives preguntas básicas que SÍ están en las herramientas. Tu función es resolver consultas usando las herramientas disponibles.\n\n------\n\n## LÍMITE DE INTERACCIÓN - LEADS NO CALIFICADOS\n\nSi el cliente insiste más de 2 veces con algo fuera del alcance:\n\n**Ejemplos:**\n\n- Locales comerciales\n- Proyectos en otra ciudad\n- Departamentos de 1 dormitorio\n- Características que no ofrecemos\n\n**Respuesta:** \"El asesor especializado te contactará para darte mayor información sobre esa consulta específica\"\n\n**Acción:** Detener conversación.\n\n------\n\n## REGLAS ANTI-ALUCINACIÓN (CRÍTICO)\n\n### PRINCIPIO FUNDAMENTAL: USA SIEMPRE LAS HERRAMIENTAS\n\n**NUNCA inventes, supongas o recuerdes información.** Toda respuesta debe provenir de las herramientas disponibles:\n\n✅ **OBLIGATORIO:**\n\n- **Fechas, plazos, cronogramas** → Usar `preguntas_frecuentes`\n- **Precios, áreas, disponibilidad** → Usar `inventario_de_departamentos`\n- **Horarios, citas, visitas** → Usar `gestor_de_citas`\n- **Acabados, ubicación** → Usar `preguntas_frecuentes`\n\n❌ **PROHIBIDO:**\n\n- Mencionar fechas específicas sin consultar `preguntas_frecuentes`\n- Mencionar precios sin consultar `inventario_de_departamentos`\n- Confirmar horarios sin consultar `gestor_de_citas`\n- Describir acabados o características sin consultar `preguntas_frecuentes`\n- Afirmar que algo \"está listo\", \"está disponible para visita inmediata\" o \"está completamente terminado\"\n- Mencionar departamentos de 1 dormitorio (NO EXISTEN)\n- **Afirmar que cocheras o depósitos están incluidos en el precio (NUNCA están incluidos)**\n- Mencionar características de cocheras o depósitos sin consultar las herramientas\n\n### VALIDACIÓN OBLIGATORIA DE MENCIONES GEOGRÁFICAS Y RESPUESTAS AMBIGUAS\n\nCuando el cliente mencione una ubicación, zona, lugar o dé una respuesta de UNA SOLA PALABRA que no sea claramente \"2 dormitorios\" o \"3 dormitorios\":\n\n1. **PRIMERO:** Consulta `preguntas_frecuentes` para verificar si coincide con la ubicación del proyecto\n2. **SOLO SI COINCIDE:** Confirma la ubicación\n3. **SI NO COINCIDE O ES AMBIGUO:** No asumas nada. Pregunta: \"¿Te refieres a que buscas un departamento en esa zona, o te interesa conocer nuestro proyecto?\"\n\n**Ejemplos de respuestas ambiguas que requieren validación:**\n\n- Nombres de zonas (San Catalina, Miraflores, etc.)\n- Respuestas de una palabra sin contexto\n- Referencias a características sin especificar\n\n❌ **PROHIBIDO:**\n\n- Asumir que menciones de lugares son equivalentes a la ubicación del proyecto\n- Asociar nombres de zonas que el cliente menciona con la ubicación real sin validar\n- Inventar relaciones geográficas que no existen en las herramientas\n- Hacer afirmaciones sobre ubicación sin consultar `preguntas_frecuentes`\n\n### FRASES QUE NUNCA DEBES USAR:\n\n- \"Departamentos de muestra completamente terminados\"\n- \"Listos para ser visitados tal como se entrega\"\n- \"Departamentos disponibles para visita inmediata\"\n- \"Podrás apreciar los acabados tal como se entrega\"\n- \"La entrega es en [fecha específica]\" (sin consultar herramienta)\n- \"El precio es [cantidad]\" (sin consultar herramienta)\n- \"Tenemos departamentos de 1 dormitorio\"\n- \"[Zona X] es donde se ubica nuestro proyecto\" (sin consultar herramienta)\n- **\"Incluyen cochera y depósito\"**\n- **\"Incluyen al menos una cochera\"**\n- **\"La inversión incluye cochera/depósito\"**\n- **\"Todos los departamentos incluyen estacionamiento\"**\n\n### CUANDO TENGAS DUDA:\n\n**Si no estás seguro** → Usa la herramienta correspondiente\n**Si la herramienta no tiene la información** → Responde: \"Pronto un asesor especializado se conectará para darte respuesta ante esa pregunta\"\n\n### REGLA DE ORO:\n\n**\"Si no está en las herramientas, no lo digas. Si no consultaste la herramienta, no lo afirmes.\"**\n\n------\n\n## EJEMPLOS DE CASOS CRÍTICOS\n\n### 1. Primer Contacto\n```\nCliente: \"Hola, quiero información\"\nSofía: \"Hola, soy Sofía, tu asesora virtual de El Ágora\n\nTe comparto el enlace de nuestro brochure:\nhttps://heyzine.com/flip-book/2ed6f324de.html\n\n¿Qué tipo de departamento es de tu interés?\"\n```\n\n### 2. Respuestas Ambiguas (CRÍTICO)\n```\nCliente: \"San Catalina\" o \"Miraflores\"\n\n❌ INCORRECTO: Asumir que es la ubicación del proyecto\n✅ CORRECTO: \n[USA preguntas_frecuentes: ubicación]\n\"¿Te refieres a que buscas en [zona]? Nuestro proyecto está en Santiago de Surco. ¿Te interesa conocer opciones aquí?\"\n```\n\n### 3. Cocheras/Depósitos (CRÍTICO - NUNCA INCLUIDOS)\n```\nCliente: \"Fiat con cocheras\"\n\n❌ INCORRECTO: \"Incluyen cochera y depósito en la inversión\"\n✅ CORRECTO: \n[USA preguntas_frecuentes: cocheras]\nSi NO hay info: \"Un asesor te contactará para informarte sobre disponibilidad y costo de cocheras\"\n```\n\n### 4. Coordinación de Visitas\n```\nCliente: \"Mañana a las 11 AM\"\n[USA gestor_de_citas: verificar]\n[ESPERA reporte]\n\nSi DISPONIBLE: \"¡Perfecto! Te confirmo para mañana [fecha] a las 11 AM\"\nSi NO: \"A las 11 AM está ocupado. ¿Te viene bien a las 10 AM o 12 PM?\"\n```\n\n### 5. Lead No Calificado\n```\nCliente: \"¿Tienen de 1 dormitorio?\"\nSofía: \"Solo tenemos de 2 y 3 dormitorios. ¿Alguna te interesa?\"\nCliente: \"No, solo de 1\"\nSofía: \"El asesor te contactará para mayor información\" [DETENER]\n```\n\n------\n\n## RESUMEN EJECUTIVO DE CAMBIOS CRÍTICOS\n\n**Las 4 reglas más importantes para evitar alucinaciones:**\n\n1. **SIEMPRE consulta herramientas antes de afirmar**: No menciones precios, fechas, ubicaciones o características sin consultar primero\n2. **NUNCA afirmes que cocheras/depósitos están incluidos**: Esto es FALSO. Solo menciona información sobre cocheras si viene de `preguntas_frecuentes`\n3. **VALIDA respuestas ambiguas**: Si el cliente menciona una zona, nombre o da respuesta de una palabra, usa `preguntas_frecuentes` para validar antes de asumir\n4. **PREGUNTA cuando hay ambigüedad**: Si la respuesta del cliente no es clara, pide aclaración en lugar de inventar o asumir"
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 2.1,
      "position": [
        928,
        192
      ],
      "id": "da2e07f8-8ede-4672-80b8-e53c517e1cb8",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.embeddingsOpenAi",
      "typeVersion": 1.2,
      "position": [
        1344,
        624
      ],
      "id": "42afee0d-6206-46dd-9609-70072aeb0500",
      "name": "Embeddings OpenAI",
      "credentials": {
        "openAiApi": {
          "id": "gnA9sKIaU7C3fuxn",
          "name": "openai-api-el-agora"
        }
      }
    },
    {
      "parameters": {
        "amount": 2
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        -1568,
        464
      ],
      "id": "a06aeefd-5e2b-4a53-9c2b-0d4bbee2441a",
      "name": "Esperar 2 segundos",
      "webhookId": "bb1f3e86-f19c-4e08-943c-b1830a434ccb"
    },
    {
      "parameters": {
        "amount": 3
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        -896,
        496
      ],
      "id": "1de9f975-d999-4b5a-be4b-e7370cdcf612",
      "name": "Esperar 3 segundos",
      "webhookId": "436c6599-0ce7-4c12-9ae5-c81447a44c36"
    },
    {
      "parameters": {
        "descriptionType": "manual",
        "toolDescription": "Esta herramienta permite consultar el inventario completo de departamentos del Edificio Lumina con información sobre tipología, distribución, dimensiones y precios de cada unidad organizada por piso. Úsala cuando necesites comparar opciones, verificar disponibilidad, consultar precios específicos o encontrar unidades según requerimientos de área, distribución o presupuesto del cliente.",
        "documentId": {
          "__rl": true,
          "value": "1GLdXyWVpuTHMb5W01zFs-PBswm-_iUWGas9DrxHipLM",
          "mode": "list",
          "cachedResultName": "Inventario de Departamentos - Edificio Lumina",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1GLdXyWVpuTHMb5W01zFs-PBswm-_iUWGas9DrxHipLM/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Inventario de Departamentos",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1GLdXyWVpuTHMb5W01zFs-PBswm-_iUWGas9DrxHipLM/edit#gid=0"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        1056,
        416
      ],
      "id": "dcd34ad5-c954-4f49-84ff-948453b2565e",
      "name": "inventario_de_departamentos",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "4aharlcU2aZQ5r9i",
          "name": "google-api-hola@cambiodigital.shop"
        }
      }
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "value": "gpt-4.1",
          "mode": "list",
          "cachedResultName": "gpt-4.1"
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.2,
      "position": [
        672,
        416
      ],
      "id": "ef4b829c-6055-44a0-b5cc-72a6ea7edeba",
      "name": "GPT-4.1",
      "credentials": {
        "openAiApi": {
          "id": "gnA9sKIaU7C3fuxn",
          "name": "openai-api-el-agora"
        }
      }
    },
    {
      "parameters": {},
      "type": "@n8n/n8n-nodes-langchain.rerankerCohere",
      "typeVersion": 1,
      "position": [
        1504,
        624
      ],
      "id": "fd590af2-4f48-444a-a4ac-4f0820fb391c",
      "name": "Reranker Cohere",
      "credentials": {
        "cohereApi": {
          "id": "45SYUEKpsCyquHaB",
          "name": "cohere-api-el-agora"
        }
      }
    },
    {
      "parameters": {
        "description": "Usa esta herramienta para gestionar citas en el calendario del proyecto (verificar disponibilidad, agendar, modificar o cancelar visitas). El mini-agente ejecuta las operaciones y te reporta el resultado sin interactuar con el cliente. Siempre verifica disponibilidad antes de confirmar horarios al cliente.\n\n",
        "workflowId": {
          "__rl": true,
          "value": "hFuiEajjFK0tEh51",
          "mode": "list",
          "cachedResultUrl": "/workflow/hFuiEajjFK0tEh51",
          "cachedResultName": "Mini-agente de agendamiento"
        },
        "workflowInputs": {
          "mappingMode": "defineBelow",
          "value": {
            "mensaje_cliente": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('mensaje_cliente', ``, 'string') }}",
            "fecha_propuesta": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('fecha_propuesta', ``, 'string') }}",
            "hora_propuesta": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('hora_propuesta', ``, 'string') }}",
            "tipo_accion": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('tipo_accion', ``, 'string') }}",
            "lead_id": "={{ $('Traer todos los datos del lead').item.json.id }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "lead_id",
              "displayName": "lead_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "canBeUsedToMatch": true,
              "type": "string",
              "removed": false
            },
            {
              "id": "mensaje_cliente",
              "displayName": "mensaje_cliente",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "canBeUsedToMatch": true,
              "type": "string",
              "removed": false
            },
            {
              "id": "fecha_propuesta",
              "displayName": "fecha_propuesta",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "canBeUsedToMatch": true,
              "type": "string",
              "removed": false
            },
            {
              "id": "hora_propuesta",
              "displayName": "hora_propuesta",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "canBeUsedToMatch": true,
              "type": "string",
              "removed": false
            },
            {
              "id": "tipo_accion",
              "displayName": "tipo_accion",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "canBeUsedToMatch": true,
              "type": "string",
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        }
      },
      "type": "@n8n/n8n-nodes-langchain.toolWorkflow",
      "typeVersion": 2.2,
      "position": [
        1232,
        416
      ],
      "id": "96d394b3-8197-48e7-85eb-963188d9ab68",
      "name": "gestor_de_citas"
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $('Traer todos los datos del lead').item.json.id }}"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryPostgresChat",
      "typeVersion": 1.3,
      "position": [
        800,
        416
      ],
      "id": "6f17cad2-f53b-4ae3-ae52-3d338ff8c416",
      "name": "Postgres Chat Memory",
      "credentials": {
        "postgres": {
          "id": "HwOvbt3DnllVA181",
          "name": "supabase-el-agora"
        }
      }
    },
    {
      "parameters": {
        "mode": "retrieve-as-tool",
        "toolDescription": "Esta herramienta permite consultar información oficial y actualizada sobre el proyecto inmobiliario Edificio LUMINA, incluyendo detalles comerciales, especificaciones técnicas, ubicación, cronograma, opciones de financiamiento e inventario de departamentos disponibles. Úsala cuando necesites responder preguntas específicas sobre cualquier aspecto de este proyecto inmobiliario.",
        "tableName": {
          "__rl": true,
          "value": "documents",
          "mode": "list",
          "cachedResultName": "documents"
        },
        "useReranker": true,
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.vectorStoreSupabase",
      "typeVersion": 1.3,
      "position": [
        1360,
        416
      ],
      "id": "431b3bcf-cb59-46c0-a7fc-41a1b7375f15",
      "name": "preguntas_frecuentes",
      "credentials": {
        "supabaseApi": {
          "id": "LN9tPBGkGjKcZVzI",
          "name": "supabase-el-agora"
        }
      }
    }
  ],
  "connections": {
    "Elegir entre tipo de mensaje": {
      "main": [
        [
          {
            "node": "Formatear Texto",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Descargar audio",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Descargar imagen",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Transcripción del audio": {
      "main": [
        [
          {
            "node": "Formatear Texto1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Descargar audio": {
      "main": [
        [
          {
            "node": "Transcripción del audio",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "Formatear mensaje",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "¿Está el campo personalizado 'Atiende IA' activo?": {
      "main": [
        [
          {
            "node": "Elegir entre tipo de mensaje",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "No Operation, do nothing",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Descargar imagen": {
      "main": [
        [
          {
            "node": "Análisis de imagen",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Formatear mensaje": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Formatear Texto": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Formatear Texto1": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Formatear Texto2": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 2
          }
        ]
      ]
    },
    "¿Llegaron los campos personalizados?": {
      "main": [
        [
          {
            "node": "¿Está el campo personalizado 'Atiende IA' activo?",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Esperar 3 segundos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Think": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Manda un campo personalizado nuevo": {
      "main": [
        [
          {
            "node": "Esperar 2 segundos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Traer todos los datos del lead": {
      "main": [
        [
          {
            "node": "¿Llegaron los campos personalizados?",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Enviar mensaje a Kommo": {
      "main": [
        [
          {
            "node": "Activar el bot",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Análisis de imagen": {
      "main": [
        [
          {
            "node": "Formatear Texto2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Formatear mensaje para envío": {
      "main": [
        [
          {
            "node": "Enviar mensaje a Kommo",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Traer datos de Kommo": {
      "main": [
        [
          {
            "node": "Esperar 2 segundos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Formatear mensaje para envío",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Embeddings OpenAI": {
      "ai_embedding": [
        [
          {
            "node": "preguntas_frecuentes",
            "type": "ai_embedding",
            "index": 0
          }
        ]
      ]
    },
    "Esperar 2 segundos": {
      "main": [
        [
          {
            "node": "Traer todos los datos del lead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Esperar 3 segundos": {
      "main": [
        [
          {
            "node": "Manda un campo personalizado nuevo",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "inventario_de_departamentos": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "GPT-4.1": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Reranker Cohere": {
      "ai_reranker": [
        [
          {
            "node": "preguntas_frecuentes",
            "type": "ai_reranker",
            "index": 0
          }
        ]
      ]
    },
    "gestor_de_citas": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Postgres Chat Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "preguntas_frecuentes": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "instanceId": "f00b66afa0a9d2107ce765c346d89a1957d4399f519205524e4e50f82be4be30"
  }
}