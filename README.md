# Desafio Técnico: Engenharia e Análise de Dados Orçamentários (Senado Federal)

Repositório dedicado ao pipeline de extração, tratamento, higienização e visualização da evolução histórica de gastos com assistência à saúde de parlamentares, ex-parlamentares e seus dependentes no Senado Federal.

---

## Diferenciais Técnicos Implementados

Para garantir os mais altos padrões de engenharia de software e análise de dados exigidos no desafio, o projeto foi desenvolvido aplicando as seguintes boas práticas:
*   **Modularização e Clean Code:** Código estruturado em funções com responsabilidade única e clara separação entre a camada de lógica de dados e a camada de renderização visual.
*   **Uso Rigoroso de Type Hinting:** Todas as funções possuem indicações explícitas de tipos primitivos e estruturados de dados (`List`, `Dict`, `Optional`), aumentando a manutenibilidade e legibilidade do código.
*   **Tratamento Robusto de Exceções:** Implementação de blocos `try-except` para capturar falhas de IO (como arquivos ausentes) ou problemas consistentes de decodificação na leitura dos conjuntos de dados.
*   **Engenharia de Dados (Data Cleaning):** Desenvolvimento de algoritmos baseados em Expressões Regulares (RegEx) para limpar ruídos gerenciais estruturais comuns em planilhas do governo brasileiro.

---

## 🛠️ Tecnologias e Dependências Utilizadas

*   **Linguagem Principal:** Python 3
*   **Pandas:** Engenharia de dados, tratamento de matrizes e exportação tabular.
*   **Matplotlib e Seaborn:** Camada gráfica profissional de alta qualidade estética para Storytelling.
*   **Typing:** Biblioteca nativa para tipagem estática assistida.

---

## Relato de Desafios Técnicos e Tratamento de Erros

### 1. Abordagem Inicial e Falha de Requisição Direta
A estratégia original do pipeline previa o consumo automatizado dos dados em tempo real diretamente do portal do governo via requisições HTTP (`requests.get`). No entanto, os servidores do Senado Federal e da Câmara possuem barreiras de segurança (*Firewalls* e proteções DDoS) que bloqueiam requisições vindas de faixas de IP de servidores em nuvem públicos (como os da Google). 

A tentativa de consumo direto gerou o erro de parseamento de JSON: `ValueError: Expecting value: line 1 column 1 (char 0)`.

### Solução Adotada (Pipeline Híbrido)
Para contornar a restrição física de infraestrutura de rede, implementamos um pipeline híbrido: download manual do arquivo consolidado em formato `.csv` e carga assistida de arquivos locais no script através da biblioteca nativa do Google Colab (`from google.colab import files`).

### Limpeza do Relatório Gerencial com Pandas
Como a planilha baixada foi projetada para leitura humana e não para processamento computacional puro, aplicamos os seguintes tratamentos para remover ruídos na base:
*   **Ajuste de Cabeçalho:** O título institucional fixado na linha superior foi descartado utilizando o parâmetro `skiprows=1` para alinhar os anos cronológicos como colunas exatas do DataFrame.
*   **Limpeza Dinâmica de Rodapé:** Notas de rodapé informativas foram removidas programaticamente varrendo a primeira coluna e descartando linhas através do padrão RegEx `^\d+\)` (dígitos numéricos seguidos de parênteses).
*   **Padronização Textual:** Aplicação de `.str.upper()` e `.str.strip()` nas colunas textuais para remover espaços em branco residuais e unificar chaves de categorias em letras maiúsculas.

---

## Storytelling dos Dados e Explicação do Fenômeno

O estudo temporal (2014 a 2025) da evolução orçamentária do Senado revelou padrões altamente relevantes sobre a gestão pública de saúde:

### 1. Narrativa Coerente das Descobertas
*   **A Era da Estabilidade (2014-2020):** Durante sete anos consecutivos, os gastos de saúde do Senado operaram sob rígido controle e previsibilidade orçamentária, mantendo-se sempre abaixo do teto de R$ 15 milhões anuais.
*   **O Ponto de Inflexão (2021):** O ano de 2021 representou um choque orçamentário histórico sem precedentes. Em apenas 12 meses, as despesas totais mais que dobraram (**+112,5% de aumento**), saltando repentinamente para R$ 31,71 milhões.
*   **O Novo Patamar (2022-2025):** Longe de ser um pico temporário isolado, as despesas mantiveram sua tendência de alta exponencial, quebrando a barreira recorde de R$ 40,47 milhões no fechamento de 2025.

### 2. Análise e Explicação do Fenômeno Orçamentário
A quebra abrupta de comportamento observada nos gráficos a partir de 2021 é justificada analiticamente por três fenômenos convergentes:
1.  **Inflação Médica Severa Pós-Pandemia:** O setor de saúde suplementar no Brasil registrou altas históricas nos custos de insumos médico-hospitalares, exames e diárias de internação no pós-COVID-19, inflando diretamente os contratos do setor público.
2.  **Migração Crítica para o Modelo de Livre Escolha:** Ao analisar isoladamente os gráficos por categoria, nota-se que a linha de *Indenizações e Restituições* (reembolso direto na conta do parlamentar) quadruplicou de volume. Isso prova estatisticamente uma mudança drástica de comportamento dos beneficiários, que passaram a preferir consultas e hospitais particulares de elite fora da rede credenciada do Senado.
3.  **Represamento Contábil de Passivos:** Os picos agudos observados nas categorias de *Despesas de Exercícios Anteriores* comprovam que contas médicas geradas durante os anos de crise sanitária ficaram retidas em auditoria e foram pagas acumuladas em lotes a partir de 2021 e 2022.

### 3. Sugestões de Melhorias Futuras (Gargalos Não Resolvidos)
*   **Abertura de Microdados Anonimizados:** O Senado fornece visões macro consolidadas anualmente. Para o desenvolvimento de modelos preditivos ou auditorias de conformidade com IA, recomenda-se a abertura de microdados por evento de saúde (anonimizados por ID) para identificar anomalias estatísticas (*outliers*).
*   **Provisões Contábeis de Governança:** Recomenda-se a criação de mecanismos contábeis para suavizar a volatilidade de despesas atrasadas (exercícios anteriores), mitigando o impacto de picos inesperados na peça orçamentária anual do órgão.
