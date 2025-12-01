Nesse arquivo nao estao os prompts completos, apenas como gosto de estruturar, e como acredito que seja mais eficiente:

# 📋 Estrutura do Prompt do Agente Eero

## Visão Geral

O prompt foi desenhado com uma arquitetura modular e hierárquica que garante consistência, clareza e manutenibilidade. Esta documentação explica cada seção e seu propósito dentro do sistema.

---

## 🏗️ Arquitetura Geral

```
┌─────────────────────────────────────────┐
│         CARGO (Identidade)              │
├─────────────────────────────────────────┤
│         FERRAMENTAS DISPONÍVEIS         │
│  ┌─────────────────────────────────┐   │
│  │  • Consulta de Preços           │   │
│  │  • Calculator                   │   │
│  │  • Registro Orçamento           │   │
│  │  • Confirmar Orçamento          │   │
│  │  • Think                        │   │
│  └─────────────────────────────────┘   │
├─────────────────────────────────────────┤
│      CONTEXTO DE ATENDIMENTO            │
├─────────────────────────────────────────┤
│            REGRAS                       │
├─────────────────────────────────────────┤
│         RESTRIÇÕES                      │
├─────────────────────────────────────────┤
│         COMPORTAMENTO                   │
├─────────────────────────────────────────┤
│         PROCEDIMENTO                    │
├─────────────────────────────────────────┤
│          EXEMPLOS (JSON)                │
└─────────────────────────────────────────┘
```

---

## 📦 Componentes do Prompt

### 1. **Cargo (Identidade do Agente)**

**Propósito:** Define quem é o agente, sua missão e alcance.

**Conteúdo:**
- Nome do agente
- Papel específico
- Missão principal
- Público-alvo

---

### 2. **Ferramentas Disponíveis**

**Propósito:** Documentação completa de cada tool que o agente pode utilizar.

**Estrutura por ferramenta:**

```
### <<tool>> NOME_FERRAMENTA
**Descrição:** [O que faz]
**Uso:** [Quando e como usar]
**Protocolo de Comunicação:** [Regras específicas]
```

**Ferramentas incluídas:**

1. **CONSULTA DE PREÇOS**
   - Base de dados de produtos
   - Protocolo crítico de comunicação (envio em UMA única chamada)
   - Exemplos de uso correto e incorreto

2. **CALCULATOR1**
   - Cálculos matemáticos
   - Ordem lógica de operações
   - Regras de aplicação de preços

3. **REGISTRO ORÇAMENTO**
   - Formato obrigatório de envio
   - Regra de ativação (somente quando o usuário solicitar explicitamente)
   - Templates para um ou múltiplos móveis

4. **CONFIRMAR ORÇAMENTO**
   - Criação do documento final
   - Momento de ativação

5. **THINK**
   - Ferramenta de raciocínio
   - Uso prévio a cálculos complexos

**Elemento-chave:** O **Protocolo de Comunicação** para CONSULTA DE PREÇOS é crítico e está extensamente documentado para evitar erros comuns.

---

### 3. **Contexto de Atendimento**

**Propósito:** Define o ambiente de operação do agente.

**Elementos:**
- Perfil de usuários
- Tipo de comunicação (interna vs externa)
- Objetivo principal
- Formato de entrega

**Impacto:** Permite que o agente ajuste seu tom e nível técnico apropriadamente.

---

### 4. **Regras**

**Propósito:** Políticas operacionais que o agente deve seguir.

**Seções:**

#### 4.1 Coleta de Informações
- Estratégia de obtenção de dados
- Manejo de informação incompleta
- Política de não assumir valores

#### 4.2 Consulta às Ferramentas
- Ordem recomendada de consultas
- Frequência de uso
- Validação de dados

#### 4.3 Cálculo do Orçamento
- Sequência de operações
- Fórmulas específicas
- Formato de apresentação

---

### 5. **Restrições**

**Propósito:** Limites absolutos de operação (o que o agente NÃO pode fazer).

**Categorias:**

#### 5.1 Proibições Estritas
- Não sugerir alternativas fora do sistema
- Não estimar preços
- Não oferecer descontos não autorizados
- Não fornecer informações sobre prazos
- Não assumir dimensões

#### 5.2 Limitações de Informação
- Alcance de conhecimento
- Áreas fora de competência

**Por que são críticas:** Previnem que o agente exceda suas capacidades ou comprometa a empresa.

---

### 6. **Comportamento**

**Propósito:** Define como o agente deve se comunicar e responder.

**Seções:**

#### 6.1 Tom e Comunicação
- Nível de profissionalismo
- Estilo de linguagem
- Uso de terminologia técnica

#### 6.2 Manejo de Situações
- Respostas a casos específicos
- Estratégias de resolução
- Manutenção de contexto

---

### 7. **Procedimento**

**Propósito:** Fluxo de trabalho passo a passo para completar a tarefa principal.

**Fluxo Padrão:**

```
1. Recepção da solicitação
   ↓
2. Validação de informações
   ↓
3. Consulta às ferramentas (UMA única chamada)
   ↓
4. Cálculo com calculadora
   ↓
5. Apresentação do resultado
   ↓
6. Registro (se solicitado)
   ↓
7. Finalização
   ↓
8. Confirmação de documento (se solicitado)
```

**Inclui:**
- Templates de saída
- Formatos obrigatórios
- Checklist de validação

---

### 8. **Exemplos (JSON)**

**Propósito:** Casos de uso concretos que ilustram comportamento correto e incorreto.

**Estrutura do JSON:**

```json
{
  "examples": [
    {
      "id": número,
      "scenario": "Descrição do caso",
      "input": "Solicitação do usuário",
      "internal_process": ["Passo 1", "Passo 2", "..."],
      "correct_response": "Resposta correta",
      "incorrect_behavior": "O que NÃO fazer",
      "critical_rule": "Regra-chave a lembrar"
    }
  ],
  "communication_protocol": {
    "golden_rule": "Regra de ouro",
    "workflow_checklist": ["Checklist"],
    "correct_format_single": "Formato correto",
    "correct_format_multiple": "Formato para múltiplos"
  },
  "key_rules": {
    "critical_formatting": ["Regras críticas"],
    "workflow_sequence": ["Sequência de trabalho"],
    "never_do": ["Proibições"],
    "always_do": ["Obrigações"]
  }
}
```

**Exemplos incluídos:**

1. **Solicitação simples** - Um móvel sem extras
2. **Com adicional** - Aplicação de porcentagens
3. **Baixo mesada com material** - Cálculo de Preço Extra
4. **Múltiplos móveis** - Gestão de cotações complexas
5. **Caso complexo** - Taxa, envio, material e transforo

**Valor agregado:** Cada exemplo inclui:
- ✅ Comportamento correto
- ❌ Comportamento incorreto
- 🎯 Regra crítica a lembrar

