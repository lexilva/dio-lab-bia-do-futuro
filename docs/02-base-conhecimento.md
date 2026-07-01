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

[ex: Os JSON/CSV são carregados no início da sessão e incluídos no contexto do prompt]

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

[Sua descrição aqui]

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
