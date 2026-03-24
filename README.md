# 📝 Prova Prática — Fundamentos de Testes

Implementação de 5 cenários de sistemas utilizando **TypeScript** e framework de testes customizado. Cada cenário possui regras de negócio específicas validadas por 10 testes automatizados.

---

## 🚀 Como rodar

### Instalar dependências

```bash
cd fundamentos/prova
npm install
```

### Rodar um cenário específico

```bash
npm run dev:cenario01   # 🍕 Restaurante
npm run dev:cenario02   # 🎬 Cinema
npm run dev:cenario03   # 🏋️ Academia
npm run dev:cenario04   # 🅿️ Estacionamento
npm run dev:cenario05   # 🏦 Banco
```

---

## 📁 Estrutura do Projeto

```
prova/
├── cenario01-restaurante/
│   └── index.ts
├── cenario02-cinema/
│   └── index.ts
├── cenario03-academia/
│   └── index.ts
├── cenario04-estacionamento/
│   └── index.ts
├── cenario05-banco/
│   └── index.ts
├── framework-teste.ts
├── package.json
└── tsconfig.json
```

---

## 🍕 Cenário 01 — Sistema de Pedidos de Restaurante

### Regras de Negócio

| # | Regra |
|---|-------|
| 1 | Taxa de entrega fixa: **R$ 8,00** |
| 2 | Subtotal acima de **R$ 100,00**: entrega grátis |
| 3 | Combo (prato + bebida + sobremesa): **15% de desconto** no subtotal |
| 4 | Gorjeta opcional: **10%** do subtotal (calculada antes do desconto) |
| 5 | Pedido mínimo para entrega: **R$ 25,00** (antes de descontos) |
| 6 | Máximo de **20 itens** por pedido (soma das quantidades) |

### Função implementada

```typescript
calcularPedido(pedido: IPedido): IResultadoPedido
```

### Testes

| # | Descrição | Esperado |
|---|-----------|----------|
| 1 | Pedido simples com frete | `valorTotal: 48` |
| 2 | Pedido com frete grátis acima de R$100 | `valorTotal: 129` |
| 3 | Pedido com combo — desconto 15% | `valorTotal: 54.75` |
| 4 | Pedido com combo + gorjeta | `valorTotal: 60.25` |
| 5 | Pedido abaixo do mínimo R$25 | `ehValido: false` |
| 6 | Pedido vazio | `ehValido: false` |
| 7 | Pedido acima de 20 itens | `ehValido: false` |
| 8 | Gorjeta sem combo | `valorTotal: 99.30` |
| 9 | Combo + frete grátis + gorjeta | `valorTotal: 146.30` |
| 10 | Só bebidas — sem desconto | `valorTotal: 36` |

---

## 🎬 Cenário 02 — Sistema de Venda de Ingressos de Cinema

### Regras de Negócio

| # | Regra |
|---|-------|
| 1 | Ingresso inteira: **R$ 40,00** |
| 2 | Meia-entrada (estudante ou idoso 60+): **50% de desconto** |
| 3 | Sessão 3D: acréscimo de **R$ 10,00** por ingresso |
| 4 | Terça-feira: **30% de desconto** no valor base (aplicado antes da meia-entrada) |
| 5 | Máximo de **6 ingressos** por compra |
| 6 | Menores de **16 anos** não podem assistir na sessão `noite` |
| 7 | Classificação indicativa do filme deve ser respeitada |

### Função implementada

```typescript
comprarIngressos(compra: ICompraIngresso): IResultadoCompra
```

### Testes

| # | Descrição | Esperado |
|---|-----------|----------|
| 1 | Ingresso inteira dia normal 2D | `valorTotal: 40` |
| 2 | Meia-entrada estudante | `valorTotal: 20` |
| 3 | Meia-entrada idoso 60+ | `valorTotal: 20` |
| 4 | Sessão 3D inteira | `valorTotal: 50` |
| 5 | Terça-feira + meia-entrada | `valorTotal: 14` |
| 6 | 6 ingressos (máximo) | `valorTotal: 240` |
| 7 | 7 ingressos | `ehValida: false` |
| 8 | Menor de 16 anos na sessão noite | `ehValida: false` |
| 9 | Sessão 3D na terça-feira | `valorTotal: 38` |
| 10 | Compra mista — inteira + meia | `valorTotal: 60` |

---

## 🏋️ Cenário 03 — Sistema de Academia

### Regras de Negócio

| # | Regra |
|---|-------|
| 1 | Plano mensal: **R$ 99,90** |
| 2 | Plano trimestral: **R$ 249,90** |
| 3 | Plano anual: **R$ 899,90** |
| 4 | Personal trainer: acréscimo de **R$ 50,00/mês** |
| 5 | Cancelamento antes do prazo: multa de **20%** do valor restante |
| 6 | Limite de **1 check-in por dia** por aluno |
| 7 | Horário de funcionamento: **6h às 23h** |
| 8 | Aluno com mensalidade vencida ou inativo: **check-in bloqueado** |

### Funções implementadas

```typescript
calcularPlano(plano: string, personal: boolean): IResultadoPlano
realizarCheckin(checkin: ICheckin): IResultadoCheckin
cancelarPlano(alunoId: number): IResultadoCancelamento
```

### Testes

| # | Descrição | Esperado |
|---|-----------|----------|
| 1 | Plano mensal sem personal | `valorMensal: 99.90` |
| 2 | Plano anual sem personal | `valorTotal: 899.90` |
| 3 | Check-in válido | `permitido: true` |
| 4 | Check-in duplicado no mesmo dia | `permitido: false` |
| 5 | Check-in fora do horário (5h) | `permitido: false` |
| 6 | Cancelamento com multa 20% | `ehValido: true` |
| 7 | Aluno inadimplente bloqueado | `permitido: false` |
| 8 | Plano mensal com personal | `valorMensal: 149.90` |
| 9 | Plano anual com personal — total | `valorTotal: 1499.90` |
| 10 | Plano inválido | `ehValido: false` |

---

## 🅿️ Cenário 04 — Sistema de Estacionamento

### Regras de Negócio

| # | Regra |
|---|-------|
| 1 | Primeira hora: **R$ 10,00** |
| 2 | Hora adicional: **R$ 5,00** (frações arredondam para cima) |
| 3 | Diária máxima: **R$ 50,00** |
| 4 | Mensalista: **R$ 300,00/mês** — não paga por hora |
| 5 | Tolerância de **15 minutos** sem custo |
| 6 | Veículo não cadastrado: **entrada bloqueada** |
| 7 | Máximo de **100 vagas** — lotado não permite entrada |
| 8 | Perda de ticket: multa fixa de **R$ 80,00** |

### Funções implementadas

```typescript
registrarEntrada(dados: IRegistrarEntrada): IResultadoEntrada
registrarSaida(dados: IRegistrarSaida): IResultadoSaida
```

### Testes

| # | Descrição | Esperado |
|---|-----------|----------|
| 1 | Tolerância 15min — 10min de estadia | `valor: 0` |
| 2 | Estadia de 1 hora exata | `valor: 10` |
| 3 | Estadia de 2h30min | `valor: 20` |
| 4 | Estadia de 10h — teto diária | `valor: 50` |
| 5 | Mensalista não paga por hora | `valor: 0` |
| 6 | Veículo não cadastrado | `ehValido: false` |
| 7 | Estacionamento lotado | `ehValido: false` |
| 8 | Perda de ticket | `valor: 80` |
| 9 | Estadia de 3h exatas | `valor: 20` |
| 10 | Entrada válida retorna ticket | `ehValido: true` |

---

## 🏦 Cenário 05 — Sistema Bancário (Conta Corrente)

### Regras de Negócio

| # | Regra |
|---|-------|
| 1 | Depósito mínimo: **R$ 10,00** |
| 2 | Saque não pode ultrapassar o saldo **(sem cheque especial)** |
| 3 | Transferência: limite de **R$ 5.000,00** por transação |
| 4 | Transferência para outro banco: taxa de **R$ 2,50** (descontada do remetente) |
| 5 | Conta deve estar **ativa** para qualquer operação |
| 6 | Extrato registra **tipo, valor e data** de cada movimentação |

### Funções implementadas

```typescript
depositar(dados: IDepositar): boolean
sacar(dados: ISacar): boolean
transferir(dados: ITransferir): boolean
```

### Testes

| # | Descrição | Esperado |
|---|-----------|----------|
| 1 | Depósito válido R$100 | `true` |
| 2 | Depósito abaixo do mínimo R$5 | `false` |
| 3 | Saque válido R$200 | `true` |
| 4 | Saque acima do saldo | `false` |
| 5 | Transferência mesmo banco — sem taxa | `true` |
| 6 | Transferência outro banco — taxa R$2,50 | `saldo: 797.50` |
| 7 | Transferência acima do limite R$5000 | `false` |
| 8 | Operação em conta inativa | `false` |
| 9 | Saldo correto após múltiplas operações | `saldo: 1000` |
| 10 | Extrato registra movimentação | `extrato.length: 1` |

---

## 🧪 Framework de Testes

```typescript
validar({
    descricao: 'descrição do teste',
    atual: valorRetornadoPelaFuncao,
    esperado: valorCorreto
})
```

### Saída esperada

```
✔ [PASSOU] - descrição do teste
❌ [FALHOU] - descrição do teste
   Esperava: X | Recebeu: Y
```

---

## 📊 Critérios de Avaliação

| Critério | Pontos |
|----------|--------|
| Cada teste correto | 1 ponto |
| Total por cenário | 10 pontos |
| **Total geral** | **50 pontos** |
