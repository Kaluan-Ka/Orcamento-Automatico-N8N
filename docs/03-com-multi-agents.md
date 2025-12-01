Para uma execução assertiva entre workflow principal e subworkflows, é necessário criar protocolos de comunicação entre os agentes. 

# Comunicação Agente Principal e Consulta Precios

Nessa etapa, o Agente Principal deve agrupar todas as informações enviadas pelo usuário, e solicitar as informações ao Agente Consulta Precios em um único input. Abaixo está o protocolo usado no prompt original do projeto:

***PROTOCOLO DE COMUNICACIÓN CON TOOL CONSULTA DE PRECIOS
### Regla Crítica de Envío de Información

**SIEMPRE enviar solicitud COMPLETA a la tool en UNA SOLA LLAMADA**

#### ✅ Comportamiento CORRECTO:
Cuando tienes que consultar precios, AGRUPA TODA la información y envía en una sola llamada:
**Para un solo mueble:**
```
Necesito cotizar:
- [Tipo de mueble]
- Dimensiones: [ancho]m x [alto]m x [profundidad]m
- Material: [material específico si es bajo mesada]
- Adicionales: [lista completa de adicionales o "sin adicionales"]
- Transforo: [sí/no] [si es bajo mesada]
```
**Para múltiples muebles:**
```
Cotización para:
1. [Tipo]: [ancho]m x [alto]m x [prof]m, [material si aplica], [adicionales]
2. [Tipo]: [ancho]m x [alto]m x [prof]m, [material si aplica], [adicionales]
3. [Tipo]: [ancho]m x [alto]m x [prof]m, [material si aplica], [adicionales]
```
#### ❌ Comportamiento INCORRECTO (NUNCA hacer):
**NO enviar información en pedazos separados:**
```
❌ Primera llamada: "Bajo mesada 1.20 x 0.78 x 0.60"
❌ Segunda llamada: "Con granito gris mara"
❌ Tercera llamada: "Y también alacena..."
```
**NO hacer múltiples llamadas consecutivas para el mismo presupuesto:**
```
❌ Llamada 1: Consultar primer mueble
❌ Llamada 2: Consultar segundo mueble
❌ Llamada 3: Consultar tercer mueble
```
### Flujo de Trabajo Correcto
1. **RECOLECTAR** toda la información del usuario primero
2. **VERIFICAR** internamente que tienes todos los datos necesarios
3. **AGRUPAR** toda la información en un solo mensaje estructurado
4. **ENVIAR** una sola llamada a la tool CONSULTA DE PRECIOS con TODO el contenido
5. **RECIBIR** la respuesta completa de la tool
6. **PROCESAR** los cálculos finales con la calculadora
7. **PRESENTAR** el presupuesto al usuario
---
## 🔄 RECORDATORIO CRÍTICO
**ANTES de llamar a la tool CONSULTA DE PRECIOS:**
☐ ¿Tengo TODA la información necesaria?
☐ ¿Estoy enviando TODO en UNA SOLA llamada?
☐ ¿He estructurado la información de forma clara?
☐ ¿Incluí todos los muebles si son múltiples?
**Si la respuesta es SÍ a todas → Hacer la llamada**
**Si alguna respuesta es NO → Recolectar información faltante PRIMERO**



## COMUNICAÇÃO AGENTE PRINCIPAL E REGISTRO PRESUPUESTO

Nessa etapa, o agente principal deve informar os dados ao Agente Registro Presupuesto em um formato específico:

## 📤 Formato Obligatorio de Envío
Cuando llames a la tool REGISTRO PRESUPUESTO, **SIEMPRE** envía la información en este formato exacto:

### Para UN mueble:
```
Cliente: [Nombre Cliente]
Mueble: [Tipo de mueble]
Ancho: [ancho] metros
Alto: [alto] metros
Profundidad: [profundidad] metros
Material: [Material si hay] *si no hay enviar ""
Adicionales: [Lista de adicionales si los hay] *si no hay enviar ""
Precio Mueble: $[precio mueble] ARS
Precio Extra: $[precio de material específico de bajo mesada si aplica]
Envío e instalación: $[precio si hay] ARS
```
### Para MÚLTIPLES muebles:
```
Cliente: [Nombre Cliente]

Mueble 1:
Mueble: [Tipo de mueble]
Ancho: [ancho] metros
Alto: [alto] metros
Profundidad: [profundidad] metros
Material: [Material si hay] *si no hay enviar ""
Adicionales: [Lista de adicionales si los hay] *si no hay enviar ""
Precio Mueble: $[precio mueble] ARS
Precio Extra: $[precio de material específico de bajo mesada si aplica]
Envío e instalación: $[precio si hay] ARS

Mueble 2:
Mueble: [Tipo de mueble]
Ancho: [ancho] metros
Alto: [alto] metros
Profundidad: [profundidad] metros
Material: [Material si hay] *si no hay enviar ""
Adicionales: [Lista de adicionales si los hay] *si no hay enviar ""
Precio Mueble: $[precio mueble] ARS
Precio Extra: $[precio de material específico de bajo mesada si aplica]
Envío e instalación: $[precio si hay] ARS

Envío e instalación: $[precio si hay o "$0"] ARS


