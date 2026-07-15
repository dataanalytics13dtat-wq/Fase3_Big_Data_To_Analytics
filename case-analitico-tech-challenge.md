# Case Analítico — Tech Challenge Fase 3

## 1. Contexto de negócio

Você atua como Especialista em Big Data & Analytics em uma consultoria estratégica de dados. O cliente é uma Instituição Financeira de grande porte que planeja expandir sua área de Dados, Analytics e IA e precisa de evidências sobre o mercado brasileiro de profissionais de dados para decidir onde investir em contratação, capacitação e tecnologia.

A fonte de evidência é a pesquisa **State of Data Brasil** (Data Hackers + Bain), que cobre formação, experiência, remuneração, tecnologias, modelo de trabalho e adoção de IA desde 2021.

## 2. Objetivo do case

Transformar as bases brutas da pesquisa em um material executivo que oriente a decisão de investimento do cliente em Dados/Analytics/IA — cobrindo estrutura do mercado, perfis valorizados, diversidade, tecnologias, adoção de IA, diferenças regionais/senioridade/modelo de trabalho, e oportunidades estratégicas.

## 3. Dados disponíveis

| Arquivo | Ano da pesquisa | Linhas (aprox.) | Colunas |
|---|---|---|---|
| State_of_data_2022.csv | 2022 | 4.270 | 353 |
| State_of_data_BR_2023_Kaggle | 2023 | 5.292 | 399 |
| Final Dataset - State of Data 2024 | 2024 | 5.216 | 403 |
| Final Dataset - State of Data 2025-2026 | 2025/26 | 3.494 | 388 |

**Ponto em aberto:** o desafio pede as "3 últimas pesquisas disponíveis". Estou assumindo **2023, 2024 e 2025-2026** como o recorte principal (mais recentes), com **2022 como referência histórica opcional** para reforçar tendência ao longo do tempo. Me avise se quiser outro recorte.

**Risco conhecido:** cada ano usa um esquema de colunas diferente (códigos tipo `P1_a`, `P2_h` mudam de posição/nome ano a ano). Isso significa que a camada Silver vai precisar de um dicionário de mapeamento por ano antes de unificar os dados — não dá para simplesmente empilhar os CSVs.

## 4. Perguntas de negócio → estrutura analítica

O desafio exige respostas a 7 perguntas. Decompus cada uma em sub-perguntas e no bloco temático correspondente:

**Bloco A — Estrutura do mercado**
- Como está estruturado o mercado brasileiro de Dados?
  - Quantos profissionais atuam em dados, por porte de empresa e setor?
  - Como o mercado evoluiu entre os anos analisados (crescimento, contração, layoffs)?
  - Distribuição de cargos (Analista, Engenheiro, Cientista, Analytics Engineer, BI, DBA etc.)

**Bloco B — Perfis mais valorizados**
- Quais perfis profissionais são mais valorizados pelo mercado?
  - Cargos com maior remuneração média e maior demanda declarada pelas empresas
  - Relação entre formação/área de estudo e cargo ocupado
  - Nível de senioridade mais procurado

**Bloco C — Diversidade**
- Qual é o cenário de diversidade de gênero nas carreiras de dados?
  - Distribuição por gênero geral e por cargo/senioridade/faixa salarial
  - Gap salarial entre gêneros, se existir
  - Evolução da diversidade entre os anos da pesquisa

**Bloco D — Tecnologias**
- Quais tecnologias apresentam maior adoção entre os profissionais?
  - Linguagens, bancos de dados, ferramentas de cloud/BI mais citadas
  - Diferenças de stack por senioridade e por cargo

**Bloco E — Inteligência Artificial**
- Qual é o índice de adoção de IA e seu impacto?
  - % de profissionais/empresas usando IA no dia a dia
  - Percepção de impacto na produtividade/carreira
  - Evolução ano a ano (essa é provavelmente a variável mais nova em 2024/2025-26)

**Bloco F — Diferenças regionais e modelo de trabalho**
- Existem diferenças relevantes entre regiões, senioridades ou modelos de trabalho?
  - Remuneração e cargos por região (Sudeste vs. demais)
  - Remoto vs. híbrido vs. presencial — preferência declarada vs. realidade
  - Satisfação e intenção de troca de emprego por modelo de trabalho

**Bloco G — Oportunidades estratégicas**
- Quais oportunidades e desafios para empresas que querem investir em Dados e IA?
  - Síntese cruzando os blocos anteriores em recomendações (onde contratar, quais skills priorizar, como reter talento, como acelerar adoção de IA)

## 5. Hipóteses de trabalho (a validar com os dados)

- A remuneração em dados segue concentrada no Sudeste, com prêmio salarial para cargos de Engenharia/Ciência de Dados sênior.
- A adoção de IA cresce de forma acentuada entre 2023 e 2025-26, mudando o mix de skills mais citados.
- Persiste uma sub-representação de mulheres em cargos técnicos sênior e de liderança, com gap salarial associado.
- Modelo de trabalho remoto/híbrido segue predominante, mas com tensão declarada em relação a políticas de retorno ao presencial.

## 6. Abordagem metodológica (conceitual, sem execução ainda)

- **Bronze**: ingestão dos CSVs originais no S3, sem alteração de schema, particionado por ano de pesquisa.
- **Silver**: padronização — dicionário de mapeamento de colunas por ano, tradução de códigos de resposta, tratamento de nulos/duplicidade, harmonização de categorias (ex.: níveis de senioridade, faixas salariais) para permitir comparação entre anos.
- **Gold**: tabelas agregadas por bloco temático (A–G acima), prontas para consulta via Athena e para alimentar os gráficos do material executivo.
- Processamento via Glue Jobs/Glue Notebook com Spark; catalogação via Glue Data Catalog; consulta via Athena.

## 7. Formato do entregável executivo

Narrativa sugerida para o PPT/PDF final: abrir com panorama do mercado (Bloco A) → perfil e remuneração (Blocos B, C) → tecnologia e IA (Blocos D, E) → segmentações (Bloco F) → fechar com recomendações acionáveis para o cliente (Bloco G). Cada bloco = 1–2 slides com gráfico + 2-3 insights escritos como texto de apoio.

## 8. Checklist de entrega (retomando as exigências do desafio)

- [ ] Material executivo (PPT/PDF) com DataViz e storytelling cobrindo os 7 blocos
- [ ] Diagrama de arquitetura AWS (Draw.io) embutido no material executivo
- [ ] Notebooks/scripts (PySpark e/ou SQL) documentando ingestão → tratamento → transformação → catalogação → consultas

## 9. Próximos passos

1. Confirmar recorte de anos (3 últimas pesquisas) e eventual uso do 2022 como histórico.
2. Explorar de fato as colunas de cada ano e montar o dicionário de mapeamento (Silver).
3. Só então partir para ingestão/tratamento em AWS ou prototipagem local, quando você der sinal verde para execução.
