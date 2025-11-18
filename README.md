# Query21 - Análise Completa de Vulnerabilidades do Tomcat

## Descrição

A **Query21** é um script Python que combina as funcionalidades das queries 7 e 10 para realizar uma análise abrangente das vulnerabilidades do projeto **Apache Tomcat**, integrando dados de documentação com informações detalhadas dos commits do GitHub.

## Sobre o Apache Tomcat

Apache Tomcat é um servidor web e container de servlets Java amplamente utilizado em ambientes de produção. Este projeto analisa as vulnerabilidades documentadas do Tomcat, incluindo:
- 47 CVEs documentados
- Commits de correção (fix) e introdução de vulnerabilidades (VCC)
- Arquivos de código Java modificados
- Análise automatizada usando modelos de linguagem (LLMs)

## Funcionalidades

### Tarefas Executadas

#### 1. Contagem de Vulnerabilidades por Projeto
- Conta o total de vulnerabilidades documentadas
- Identifica vulnerabilidades curadas (com descrição completa)
- **Filtro**: Apenas projeto Tomcat
- **Arquivo gerado**: `1_2_vulnerabilidades_por_projeto.xlsx`

#### 2. Análise de Vulnerabilidades por Tipo
- Classifica vulnerabilidades por tags/tipos
- Agrupa por projeto
- **Filtro**: Apenas projeto Tomcat
- **Arquivo gerado**: `3_vulnerabilidades_por_tipo.xlsx`

#### 3. Análise de Vulnerabilidades por Lição
- Identifica lições aprendidas (tags começando com "Lesson:")
- Conta ocorrências por projeto
- **Filtro**: Apenas projeto Tomcat
- **Arquivo gerado**: `4_vulnerabilidades_por_licao.xlsx`

#### 4. Análise Completa de Tokens (Documentação + GitHub)
Esta é a **funcionalidade principal** que integra dados de múltiplas fontes:

**Dados da Documentação (Query 7):**
- Caracteres totais da documentação
- Tokens (palavras) da descrição e erros

**Dados do GitHub (Query 10):**
- Tokens extraídos dos diffs dos commits
- Linhas adicionadas e deletadas
- Número de arquivos modificados
- Tamanho total dos arquivos modificados (em bytes)

**Arquivo gerado**: `5_analise_completa_tokens.xlsx`

### Colunas do Arquivo Principal (Tarefa 4)

O arquivo `5_analise_completa_tokens.xlsx` contém:

| Coluna | Descrição |
|--------|-----------|
| Projeto | Nome do projeto (Tomcat) |
| CVE | Identificador da vulnerabilidade |
| Repositório GitHub | URL do repositório no GitHub (apache/tomcat) |
| Caracteres Totais (Documentação) | Total de caracteres na descrição |
| Tokens Documentação | Palavras na documentação |
| Tokens GitHub (Diff) | Palavras extraídas dos diffs |
| Total de Tokens | Soma de tokens da documentação e GitHub |
| Linhas Adicionadas (GitHub) | Linhas de código adicionadas |
| Linhas Deletadas (GitHub) | Linhas de código removidas |
| Total de Linhas Modificadas | Soma de adições e deleções |
| Arquivos Modificados (GitHub) | Quantidade de arquivos alterados |
| Tamanho Total dos Arquivos (bytes) | Tamanho real dos arquivos via API |

## Requisitos

### Dependências Python
```bash
pip install requests pandas openpyxl
```

### Token do GitHub
É necessário um token de acesso pessoal do GitHub para fazer requisições à API:

```python
GITHUB_TOKEN = "seu_token_aqui"
```

**Limites da API:**
- Sem token: 60 requisições/hora
- Com token: 5000 requisições/hora

## Como Usar

```bash
python query13.py
```

## Fluxo de Execução

1. **Busca dados da API** do Vulnerability History
   - Vulnerabilidades
   - Tags/Tipos
   - Informações dos projetos

2. **Filtra vulnerabilidades** do Tomcat

3. **Executa análises**:
   - Tarefas 1-2: Contagem de vulnerabilidades
   - Tarefas 3-4: Classificação por tipo e lição
   - Tarefa 5: Análise completa com dados do GitHub

4. **Para cada vulnerabilidade**:
   - Extrai commits relacionados via eventos (fix/vcc)
   - Busca dados completos de cada commit na API do GitHub
   - Calcula tokens dos diffs
   - Obtém tamanho real dos arquivos via `contents_url`

5. **Gera arquivos Excel** com resultados consolidados

## Estatísticas Geradas

Ao final da execução, o script exibe:
- Total de tokens (documentação)
- Total de tokens (GitHub)
- Total combinado de tokens
- Total de linhas modificadas
- Total de arquivos modificados
- Tamanho total dos arquivos processados

## Diferencial da Query13

A Query13 se destaca por:
- ✅ **Integração completa** entre documentação textual e código fonte
- ✅ **Análise focada** no projeto Apache Tomcat
- ✅ **Métricas detalhadas** de tamanho e complexidade
- ✅ **Dados reais** via API do GitHub (não aproximações)
- ✅ **Visão 360°** de cada vulnerabilidade
- ✅ **Download de arquivos** completos dos commits relacionados
- ✅ **Geração de gráficos** comparativos de análise por LLMs

## Arquivos Gerados

### Arquivos Excel
- `1_2_vulnerabilidades_por_projeto.xlsx` - Contagens gerais
- `3_vulnerabilidades_por_tipo.xlsx` - Classificação por tipo
- `4_vulnerabilidades_por_licao.xlsx` - Lições aprendidas
- `5_analise_completa_tokens.xlsx` - **Análise completa integrada**
- `Relatorio_Analise_CVEs_LLM.xlsx` - Análise de CVEs por modelos LLM

### Pastas e Arquivos de Código
- `CVEs/` - Pasta com subpastas para cada CVE contendo:
  - `README.md` - Informações do CVE
  - `[hash_commit]/` - Arquivos completos de cada commit relacionado

### Gráficos
- `comparativo_llms_tomcat.png` - Comparativo de detecções por LLM
- `charts/` - Pasta com gráficos detalhados:
  - `llm_stacked_counts.png` - Contagens empilhadas por status
  - `llm_stacked_percent.png` - Percentuais empilhados por status
  - `llm_yes_no_counts.png` - Comparativo YES vs NO
  - `llm_heatmap_status.png` - Heatmap de proporções por LLM

## Scripts Auxiliares

### `criar_pastas_cves.py`
Cria estrutura de pastas para cada CVE e baixa os arquivos completos dos commits relacionados:
```bash
python criar_pastas_cves.py
```

### `generate_llm_charts.py`
Gera múltiplos gráficos detalhados de análise dos modelos LLM:
```bash
python generate_llm_charts.py
```

### `plot_compare_llms_tomcat.py`
Gera gráfico comparativo das detecções por LLM:
```bash
python plot_compare_llms_tomcat.py
```

### `analisar_cves_com_llm.py`
Analisa CVEs usando modelos de linguagem (LLM) para detectar vulnerabilidades:
```bash
python analisar_cves_com_llm.py
```