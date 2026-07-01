# Base de Conhecimento

## Dados Utilizados


| Arquivo | Formato | Utilização no Lipe |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar explicações sobre dúvidas do cliente |
| `produtos_financeiros.json` | JSON | conhecer produtos disponíveis para ensinar aos clientes |
| `transacoes.csv` | CSV | Analisar padrão de gastos dos clientes |
| `sugestoes.json`| JSON | Sugerir estratégias de poupança de acordo com o perfil dos clientes |



---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

o arquivo sugestoes.json foi criado para indicar estratégias de poupança para diminuir o tempo de atendimento da reserva de emergência

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

```python
import pandas as pd
import json

#CSVs
historico = pd.read_csv(`data/historico_atendimento.csv`)
transacoes = pd.read_csv(`data\transacoes.csv`)

#JSONs
with open('data/perfil_investidor.json', mode='r', encoding='utf-8') as arquivo:
    perfil = json.load(arquivo)
with open('data/produtos_financeiros.json', mode='r', encoding='utf-8') as arquivo:
    produtos = json.load(arquivo)
with open('data/sugestoes.json', mode='r', encoding='utf-8') as arquivo:
    sugestoes = json.load(arquivo)    
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?
Para simplificar, podemos simplesmente "injetar" os dados em nosso prompt, garantindo que o Agente tenha o melhor contexto possível. Lembrando que em soluções mais robustas o ideal é carregar as informações dinamicamente para ganhar flexibilidade.
> 
> 
```text
DADOS DO CLIENTE E PERFIL: [data/perfil_investidor.json]
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSACOES DO CLIENTE:[data/transacoes.csv]

data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

HISTORICO DE ATENDIMENTO DO CLIENTE [data/historico_atendimento.csv]

data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

PRODUTOS FINANCEIROS [data/produtos_financeiros.json]
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Multimercado",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "CDI + 2%",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
SUGESTOES DE ESTRATÉGIAS PARA OS CLIENTES [data/sugestoes.json]
[
     {
        "nome": "100/0",
        "categoria": "Pessoas sem recursos",
        "objetivo": "conseguir zerar dívidas e começar a poupar",
        "objetivo monetário": 0,
        "aporte_minimo": 0,
        "indicado_para": "pessoas com dívidas e sem recursos para poupar"
    },
    {
    "nome": "75/25",
    "categoria": "Pessoas com recursos limitadissimos",
    "objetivo": "conseguir reserva mínima de emergência",
    "objetivo monetário": 1000.00,
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva sobrevivência e iniciantes com espaço no orçamento abaixo de R$100,00"
  },
    {
        "nome": "70/20/10",
        "categoria": "Pessoas com recursos limitados",
        "objetivo": "conseguir reserva mínima de emergência",
        "objetivo monetário": 5000.00,
        "aporte_minimo": 150.00,
        "indicado_para": "Reserva sobrevivência e iniciantes com espaço no orçamento abaixo de R$200,00"
    },
    {
        "nome": "50/15/35",
        "categoria": "Pessoas com Recursos Médios",
        "objetivo": "conseguir reserva mínima de emergência",
        "objetivo monetário": 10000.00,
        "aporte_minimo": 250.00,
        "indicado_para": "Reserva sobrevivência e iniciantes com espaço no orçamento abaixo de R$350,00"
    }
]

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.
O exemplo de contexto montado abaixo é baseado na síntese dos dados da base de conhecimento, para otimizar o consumo de tokens, mas é importante ter todas as informações importantes do contexto
```
DADOS DO CLIENTE:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000
- Objetivo: Construir reserva de emergência
- Reserva aual: R$ 10.000 (meta: R$ 15.000)

RESUMO DE GASTOS:
- Moradia: R$ 1.500
- Alimentação: R$750
- Transporte: R$ 295
- Saúde: R$ 188
- Lazer: R$ 55
- Total de saídas: R$ 2.400,00

PRODUTOS DISPONÍVEIS PARA EXPLICAR:
- Tesouro Selic (risco baixo)
- CDB Liquidez Diária (risco baixo)
- LCI/LCA (risco baixo)
- Fundo Multimercado(risco médio)
- Fundo de Ações (risco alto)
ESTRATÉGIA DE POUPANÇA
- 50/15/35
- aporte mensal mínimo: R$1500
...
```
