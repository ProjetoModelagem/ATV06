
## Membros
- Guilherme de Abreu – 22.222.028-7  
- Kaique Fernandes – 22.221.011-4  

# Exercício 1 — Teste Baseado em Grafo (Reembolsos)

## 1) Objetos (estados/páginas)
- **S0 – Nova Solicitação** (formulário preenchido e enviada)
- **S1 – Triagem Automática** (regra de valor)
- **S2 – Aprovada Automática** (≤ R$500)
- **S3 – Em Aprovação do Gestor** (> R$500)
- **S4 – Reprovada pelo Gestor**
- **S5 – Em Aprovação do Diretor** (> R$2000 e gestor aprovou)
- **S6 – Reprovada pelo Diretor**
- **S7 – Aprovada Final** (fim do fluxo com aprovação)
- **S8 – Ajuste/Correção pelo Funcionário** (pode reenviar)
- **S9 – Cancelada** (funcionário cancela após reprovação)

## 2) Relações (transições possíveis)
- **S0 → S1**: enviar solicitação
- **S1 → S2**: valor ≤ 500
- **S1 → S3**: valor > 500
- **S3 → S5**: gestor aprova e valor > 2000
- **S3 → S7**: gestor aprova e 500 < valor ≤ 2000
- **S3 → S4**: gestor reprova
- **S4 → S8**: funcionário ajusta
- **S4 → S9**: funcionário cancela
- **S5 → S7**: diretor aprova
- **S5 → S6**: diretor reprova
- **S6 → S8**: funcionário ajusta
- **S6 → S9**: funcionário cancela
- **S8 → S1**: reenviar após ajuste

## 3) Grafo (nós e arestas rotuladas)

<img width="753" height="431" alt="mermaid" src="https://github.com/user-attachments/assets/29f9eadd-0971-44d7-8869-240f994bd7b5" />

## 4) Casos de teste (5 caminhos diferentes)

- **CT1**: valor = R$500 → S0 → S1 → S2 → S7 → **Aprovado automático**

- **CT2**: valor = R$1.500 → S0 → S1 → S3 → (gestor aprova) → S7 → **Aprovado após gestor**

- **CT3**: valor = R$3.500 → S0 → S1 → S3 → (gestor aprova) → S5 → (diretor aprova) → S7 → **Aprovado após gestor e diretor**

- **CT4**: valor = R$1.200 → gestor reprova → funcionário ajusta para R$400 →  
  S0 → S1 → S3 → S4 → S8 → S1 → S2 → S7 → **Aprovado após ajuste**

- **CT5**: valor = R$2.800 → gestor aprova → diretor reprova → funcionário cancela →  
  S0 → S1 → S3 → S5 → S6 → S9 → **Cancelado**



# Exercício 2 — Tabela de Decisão (Empréstimos)

## Condições (entradas)
- **C1 – Renda**: Baixa (B) | Média (M) | Alta (A)
- **C2 – Crédito**: Ruim R) | Regular (G) | Bom (BOM)
- **C3 – Valor**: Baixo (VB) | Médio (VM) | Alto (VA)
- **C4 – Garantia**: Sim (S) | Não (N)

## Ações (saídas)
- **A1 – Aprovado 2% (taxa baixa)**
- **A2 – Aprovado 5% (taxa média)**
- **A3 – Aprovado 8% (taxa alta)**
- **A4 – Rejeitado**

### Regras de aprovação
- **2%**: Renda Alta + Crédito Bom
- **5%**: (Renda Média + Crédito Regular + Garantia) OU (Renda Alta + Crédito Regular)
- **8%**: Renda Baixa + Crédito Regular + Garantia + Valor Baixo
- **Rejeitado**: Crédito Ruim sem garantia OU Valor Alto com Renda Baixa

## Tabela de decisão (12 regras principais)

| Regra | C1 Renda | C2 Crédito | C3 Valor | C4 Garantia | Ação |
|------|---------|-----------|---------|------------|-----|
| R1   | Alta    | Bom      | qualquer | qualquer   | **A1 (2%)** |
| R2   | Alta    | Regular  | qualquer | qualquer   | **A2 (5%)** |
| R3   | Média   | Regular  | qualquer | Sim        | **A2 (5%)** |
| R4   | Baixa   | Regular  | Baixo    | Sim        | **A3 (8%)** |
| R5   | Baixa   | Regular  | Médio    | Sim        | **A4 (Rejeita)** |
| R6   | Baixa   | Regular  | Alto     | Sim        | **A4 (Rejeita)** |
| R7   | Baixa   | Regular  | Baixo    | Não        | **A4 (Rejeita)** |
| R8   | qualquer| Ruim     | qualquer | Não        | **A4 (Rejeita)** |
| R9   | qualquer| Ruim     | Alto     | Sim        | **A4 (Rejeita)** |
| R10  | Média   | Bom      | Baixo/Médio | qualquer | **A2 (5%)** |
| R11  | Alta    | Bom      | Alto     | qualquer   | **A1 (2%)** |
| R12  | Média   | Regular  | Alto     | Sim        | **A4 (Rejeita)** |

## Casos de teste derivados

- **CT-A1 (2%)**: Renda Alta, Crédito Bom, Valor Médio, Garantia Não → **Aprovado 2%**
- **CT-A2 (5%)**: Renda Média, Crédito Regular, Valor Baixo, Garantia Sim → **Aprovado 5%**
- **CT-R1**: Renda Baixa, Crédito Regular, Valor Baixo, Garantia Não → **Rejeitado**
- **CT-R2**: Crédito Ruim, Garantia Não (qualquer renda/valor) → **Rejeitado**
- **CT-L1**: Renda Alta (~R$8.100), Crédito Regular, Valor Médio (R$10k) → **Aprovado 5%**
- **CT-L2**: Valor Alto (R$50.100), Renda Baixa (R$2.900), Crédito Bom, Garantia Sim → **Rejeitado**


# Exercício 3 — Matriz Ortogonal L9 (Reservas Aéreas)

## Parâmetros e valores
- **P1 – Tipo de Passageiro**: 1 = Adulto, 2 = Criança, 3 = Idoso
- **P2 – Classe do Voo**: 1 = Econômica, 2 = Executiva, 3 = Primeira Classe
- **P3 – Tipo de Tarifa**: 1 = Flexível, 2 = Restrita, 3 = Promocional
- **P4 – Destino**: 1 = Nacional, 2 = Internacional, 3 = Intercontinental

## 1) Matriz L9 (3 níveis, 4 fatores)

| Teste | P1 | P2 | P3 | P4 |
|------|----|----|----|----|
| T1 | 1 | 1 | 1 | 1 |
| T2 | 1 | 2 | 2 | 2 |
| T3 | 1 | 3 | 3 | 3 |
| T4 | 2 | 1 | 2 | 3 |
| T5 | 2 | 2 | 3 | 1 |
| T6 | 2 | 3 | 1 | 2 |
| T7 | 3 | 1 | 3 | 2 |
| T8 | 3 | 2 | 1 | 3 |
| T9 | 3 | 3 | 2 | 1 |

## 2) Casos de teste (um por linha)

| Teste | Tipo de Passageiro | Classe do Voo | Tipo de Tarifa | Destino | Resultado esperado |
|------|--------------------|--------------|---------------|--------|--------------------|
| T1 | Adulto | Econômica | Flexível | Nacional | Compra normal com tarifas nacionais |
| T2 | Adulto | Executiva | Restrita | Internacional | Aplicar restrições de remarcação e taxas internacionais |
| T3 | Adulto | Primeira | Promocional | Intercontinental | Validar regras de tarifa promocional intercontinental |
| T4 | Criança | Econômica | Restrita | Intercontinental | Checar política de menor em voo intercontinental |
| T5 | Criança | Executiva | Promocional | Nacional | Validar restrições de promoções |
| T6 | Criança | Primeira | Flexível | Internacional | Possível incompatibilidade (criança + primeira classe) |
| T7 | Idoso | Econômica | Promocional | Internacional | Aplicar benefícios de idoso e restrições de promoção |
| T8 | Idoso | Executiva | Flexível | Intercontinental | Desconto de idoso (se aplicável) e regras internacionais |
| T9 | Idoso | Primeira | Restrita | Nacional | Validar restrição de remarcação em voos nacionais |

## 3) Análise crítica
- **Comparação com teste exaustivo**: 3⁴ = 81 combinações. A L9 cobre tudo com apenas 9 testes, cerca de 89% de redução.
- **Limitação**: interações de 3 ou mais fatores não são garantidas.
- **Uso recomendado**: quando o objetivo é encontrar defeitos em pares de fatores com número reduzido de casos. Se houver risco de bugs em combinações de 3+ fatores, complementar com testes direcionados.


