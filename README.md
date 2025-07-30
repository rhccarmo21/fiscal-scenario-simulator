
# 💰 Fiscal Scenario Simulator

[![Licença MIT](https://img.shields.io/badge/Licença-MIT-green)](https://pt.wikipedia.org/wiki/Licen%C3%A7a_MIT)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/downloads/)
[![Panel](https://img.shields.io/badge/Panel-Interactive_Dashboards-orange)](https://panel.holoviz.org/)

## 📌 Sumário
1. [Visão Geral](#-visão-geral)  
2. [Principais Recursos](#-principais-recursos)
3. [Fontes de Dados](#-fontes-de-dados)
4. [Modelos Financeiros](#-modelos-financeiros)
5. [Metodologia](#-metodologia)
6. [Instalação](#-instalação)
7. [Como Usar](#-como-usar)
8. [Casos de Uso](#-casos-de-uso)
9. [Contribuição](#-contribuição)
10. [Contato](#-contato)

---

## 🌐 Visão Geral

Ferramenta de simulação fiscal que permite:

- 📊 **Projeção de cenários** orçamentários multidimensionais
- ⚖️ **Análise de equilíbrio** receita-despesa
- 🏛️ **Conformidade com LRF** (Lei de Responsabilidade Fiscal)
- 📈 **Simulação de políticas** tributárias e de gastos

**Aplicações:**
- Planejamento orçamentário plurianual
- Avaliação de sustentabilidade fiscal
- Preparação para crises financeiras
- Comunicação transparente com a sociedade

---

## 🛠️ Principais Recursos

### Simulações Disponíveis
| Recurso | Descrição | Parâmetros Ajustáveis |
|---------|-----------|-----------------------|
| Receitas | Projeção de arrecadação | Elasticidade tributária, Crescimento econômico |
| Despesas | Projeção de custos | Inflação setorial, Reajustes |
| Endividamento | Capacidade de pagamento | Taxas de juros, Prazo |
| Investimentos | ROI de projetos | Custo de oportunidade |

```python
from fiscal_simulator import ScenarioEngine

engine = ScenarioEngine(
    entity="SP", 
    level="state",
    base_year=2023
)
```

---

## 📂 Fontes de Dados

| Fonte | Dados | Cobertura | Atualização |
|-------|-------|-----------|-------------|
| Siconfi | Execução orçamentária | Municipal | Mensal |
| BACEN | Dívida consolidada | Estadual | Trimestral |
| IBGE | Atividade econômica | Regional | Anual |
| Tesouro Transparente | Transferências | Nacional | Diária |

**Estrutura Básica:**
```python
import pandas as pd

df = pd.read_parquet('receitas_municipais.parquet')
print(df.query('COD_IBGE == 3550308')[['ANO', 'RECEITA_CORRENTE']])
```

---

## 📈 Modelos Financeiros

### 1. Modelo de Receitas
```python
def revenue_projections(
    base_value: float,
    growth_rate: float,
    elasticity: float = 1.1
) -> pd.Series:
    """Projeta receitas considerando elasticidade-PIB"""
    return base_value * (1 + (growth_rate * elasticity)) ** np.arange(years)
```

### 2. Análise de Sustentabilidade
```python
from fiscal_simulator import calculate_fiscal_gap

gap = calculate_fiscal_gap(
    revenues=revenue_projections,
    expenditures=expenditure_trends,
    horizon=10
)
```

---

## ⚙️ Metodologia

1. **Previsão Baseada em Cenários**:
   - Otimista/Neutro/Pessimista
   - Stress tests financeiros

2. **Conformidade Legal**:
   ```python
   from fiscal_rules import check_lrf_compliance

   compliance_report = check_lrf_compliance(
       personnel_expenses=0.6,
       debt_limits=0.2,
       fiscal_year=2024
   )
   ```

3. **Visualização Interativa**:
   ```python
   from fiscal_viz import plot_scenario_comparison

   plot_scenario_comparison(
       scenarios=['baseline', 'austerity', 'investment'],
       years=range(2023, 2033)
   )
   ```

---

## 🛠️ Instalação

### Via pip
```bash
pip install fiscal-simulator
```

### Com módulos avançados
```bash
pip install fiscal-simulator[advanced]
```

### Docker
```bash
docker pull fiscalsim/engine:3.1
```

---

## 🚀 Como Usar

### 1. Simulação Básica
```python
from fiscal_simulator import run_simulation

results = run_simulation(
    entity=3106200,  # Belo Horizonte
    scenario={
        'tax_adjustment': 0.02,
        'health_spending': 0.15
    },
    horizon=5
)
```

### 2. Painel de Controle
```bash
panel serve app/fiscal_dashboard.py --show
```

### 3. Linha de Comando
```bash
fiscal-sim --municipio "Rio de Janeiro" --cenario "reforma_tributaria" --saida rio_simulacao.xlsx
```

---

## 🏛️ Casos de Uso

### Exemplo: Impacto de Aumento Salarial
| Ano | Receita Projetada (R$) | Despesa com Pessoal (R$) | Resultado Líquido (R$) |
|-----|------------------------|--------------------------|------------------------|
| 2024 | 10.2 bi | 6.8 bi | +3.4 bi |
| 2025 | 10.7 bi | 7.3 bi | +3.4 bi |
| 2026 | 11.1 bi | 7.9 bi | +3.2 bi |

**Alerta Automático**: Em 2026, despesa com pessoal atingiria 59.8% da receita (limite LRF: 60%)

---

## 🗂 Estrutura do Projeto

```
fiscal-simulator/
├── data/
│   ├── legal/            # Parâmetros da LRF/LC 101
│   ├── historical/       # Séries temporais fiscais
├── core/
│   ├── models/           # Modelos econométricos
│   ├── rules/            # Regras fiscais
├── app/
│   ├── dashboard/        # Visualizações interativas
├── docs/
│   ├── api_reference.md  # Documentação técnica
└── tests/
```

---

## 🤝 Contribuição

1. **Adicionar Modelos**:
   ```python
   class NewRevenueModel(FiscalModel):
       def __init__(self):
           self.name = "Modelo de ICMS Ecológico"
           self.parameters = ['tax_rate', 'environmental_index']
   ```

2. **Padrões de Código**:
   ```python
   def calculate_debt_sustainability(
       primary_balance: float, 
       interest_rate: float
   ) -> float:
       """Calcula trajetória da dívida com tratamento de edge cases"""
       return balance / interest_rate if interest_rate > 0 else float('inf')
   ```

3. **Testes**:
   ```bash
   pytest tests/test_scenario_validation.py -v
   ```

---

## 📧 Contato

**Equipe de Análise Fiscal**  
[suporte.simulador@tesouro.gov.br](mailto:suporte.simulador@tesouro.gov.br)

**Parcerias Estratégicas**  
[parcerias@conselhofiscal.org.br](mailto:parcerias@conselhofiscal.org.br)

**Acesso ao Simulador Online**  
[![Portal](https://img.shields.io/badge/Simulador_Web-Acesse_Aqui-blue)](https://simulador.tesouro.gov.br)

---

💡 **Para Gestores Públicos:**  
Baixe nosso manual de boas práticas:  
[📘 Guia de Sustentabilidade Fiscal](docs/guia_fiscal.pdf)

> **Nota Técnica:** Todas as projeções seguem as normas da LRF (Lei Complementar 101/2000) e consideram cenários macroeconômicos do BACEN.
```

### Destaques Específicos:

1. **Conformidade Legal**: Verificação automática dos limites da LRF
2. **Modelos Setoriais**: Elasticidades específicas por tipo de receita
3. **Stress Testing**: Cenários extremos para preparação a crises
4. **Integração com Siconfi**: Dados reais de execução orçamentária
5. **Transparência**: Relatórios prontos para divulgação à sociedade

### Para Implementação:

1. Conectar com API do Tesouro Transparente
2. Carregar dados históricos do ente governamental
3. Configurar parâmetros locais (alíquotas, elasticidades)
4. Integrar com sistemas de planejamento existentes
5. Realizar calibragem dos modelos com dados reais