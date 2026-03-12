# Base de Conhecimento

## Dados Utilizados

Foram utilizados arquivos da pasta data contendo informações financeiras simuladas para permitir que o agente responda de forma educativa, segura e baseada em dados.

| Arquivo                   | Formato | Utilização no Agente                                |
| ------------------------- | ------- | --------------------------------------------------- |
| transacoes.csv            | CSV     | Analisar gastos mensais e identificar excessos      |
| perfil_cliente.json       | JSON    | Definir perfil financeiro para orientar respostas   |
| metas_financeiras.json    | JSON    | Auxiliar no planejamento e controle de objetivos    |
| produtos_financeiros.json | JSON    | Explicar conceitos financeiros de forma educativa   |
| historico_atendimento.csv | CSV     | Manter contexto das conversas anteriores            |
| categorias_gastos.csv     | CSV     | Classificar despesas por tipo                       |
| limites_credito.json      | JSON    | Evitar recomendações acima da capacidade do cliente |
 dataset_financeiro_hf.csv  | CSV     | Dados públicos para simular cenários financeiros |

> 
Foi utilizado um dataset público da Hugging Face para enriquecer a base de conhecimento, permitindo que o agente tenha mais contexto para gerar respostas educativas e coerentes, sem inventar informações.

---

## Adaptações nos Dados

>Todos os arquivos mockados foram adaptados para o contexto de um agente financeiro educativo.
Foram adicionados campos relacionados a gastos, limites, perfil financeiro, metas e histórico de transações, permitindo que o agente gere respostas mais consistentes e alinhadas com o planejamento financeiro do usuário.

Os dados foram estruturados para que o agente responda apenas com base nas informações disponíveis, evitando alucinações.

---

## Estratégia de Integração

### Como os dados são carregados?
> Os dados são carregados a partir de arquivos CSV e JSON localizados na pasta `data/`.
O agente utiliza Python para ler os arquivos e armazenar as informações em memória
para serem utilizadas durante a geração das respostas.

Exemplo de carregamento:

```python
import pandas as pd
import json

# ===== CSV =====

transacoes = pd.read_csv("data/transacoes.csv")
historico = pd.read_csv("data/historico_atendimento.csv")
categorias = pd.read_csv("data/categorias_gastos.csv")

# ===== JSON =====

with open("data/perfil_cliente.json", "r", encoding="utf-8") as f:
    perfil = json.load(f)

with open("data/metas_financeiras.json", "r", encoding="utf-8") as f:
    metas = json.load(f)

with open("data/produtos_financeiros.json", "r", encoding="utf-8") as f:
    produtos = json.load(f)

with open("data/limites_credito.json", "r", encoding="utf-8") as f:
    limites = json.load(f)
```
> 

Esses dados são utilizados como base de conhecimento e incluídos no contexto enviado ao modelo de linguagem.

### Como os dados são usados no prompt?

>Os dados carregados são inseridos dinamicamente no contexto enviado ao modelo.

Antes de gerar a resposta, o agente monta um contexto contendo:

- Perfil do cliente
- Histórico de transações
- Limites financeiros
- Metas
- Categorias de gastos

Essas informações são incluídas no prompt para garantir que o modelo responda
apenas com base nos dados disponíveis.

O agente também recebe instruções para:

- Não inventar informações
- Informar quando não houver dados suficientes
- Responder de forma educativa

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000
- Limite de crédito: R$ 2.000

Metas:
- Economizar R$ 1.000 em 3 meses

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
- 05/11: Restaurante - R$ 120
- 07/11: Combustível - R$ 200

Categorias de gastos:
- Alimentação
- Transporte
- Lazer
- Moradia
  
O agente utiliza essas informações para gerar respostas educativas, sugerir melhorias no orçamento e orientar o usuário sem inventar dados.
