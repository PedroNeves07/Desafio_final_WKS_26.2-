
# ClínicaCare — Sistema Integrado de Gestão de Saúde

# Desafio Final — Workshop de Dados 26.2 | Fábrica de Software**
Modelagem de Dados · SQL · Python & Machine Learning · Power BI

---

## 📋 Descrição

Projeto completo e integrado de gestão de dados para a **ClínicaCare**, clínica fictícia de médio porte localizada em João Pessoa (PB). O projeto cobre todo o pipeline de dados de ponta a ponta: modelagem conceitual e lógica do banco, implementação em SQL (MySQL/MariaDB), análise exploratória e Machine Learning em Python, e um dashboard interativo em Power BI — todos consumindo **a mesma fonte de dados**, garantindo coerência total entre os módulos.

## 🎯 Objetivo

Substituir o controle manual (planilhas e papel) da ClínicaCare por um sistema de dados estruturado, capaz de:
- Armazenar pacientes, médicos, especialidades, consultas, prontuários, prescrições e pagamentos de forma consistente e normalizada;
- Gerar análises e relatórios de faturamento, ocupação e atendimento;
- Prever o risco de falta dos pacientes às consultas (no-show);
- Disponibilizar um dashboard interativo para apoiar decisões da diretoria.

## 🛠️ Tecnologias utilizadas

| Camada | Ferramentas |
|---|---|
| Banco de dados | MySQL / MariaDB 10.11 |
| Modelagem | Graphviz (diagrama E-R), normalização até 3FN |
| Linguagem de análise | Python 3 — pandas, numpy, matplotlib, scikit-learn, pymysql, SQLAlchemy |
| Machine Learning | scikit-learn (Árvore de Decisão, Leave-One-Out Cross-Validation) |
| Business Intelligence | Power BI Desktop (DAX, Power Query) |
| Documentação | Markdown, Word (.docx) |

## 📁 Estrutura do projeto

```
Desafio_Final_WKS_26.2/
│
├── 1_Modelagem/
│   ├── Modelo_Conceitual_ER.pdf      → Diagrama entidade-relacionamento (9 entidades)
│   └── Modelo_Logico.txt             → Modelo lógico normalizado, com tipos e constraints
│
├── 2_SQL/
│   ├── clinica_care.sql              → Script completo: DDL + DML + DQL (testado em MariaDB real)
│   └── Analise_Consultas.docx        → Explicação e insights das 8 consultas SQL
│
├── 3_Python/
│   ├── analise_clinica.ipynb         → Extração, EDA, features, ML (Previsão de No-Show)
│   └── dados_limpos.csv              → Dataset tratado, gerado pelo notebook
│
├── 4_Power_BI/
│   ├── Dashboard_ClinicaCare.pbix    → Dashboard interativo (5+ visuais, KPIs, slicers)
│   ├── dados.csv                     → Dataset flat para importação no Power BI
│   └── Insights_Dashboard.docx       → Guia de construção + medidas DAX + insights
│
└── README.md                         → Este arquivo
```

## 🧩 Como os módulos se integram

```
Modelagem (E-R + lógico)
        ↓
SQL (clinica_care.sql cria e popula o banco MySQL)
        ↓
Python (conecta no MESMO banco via SQLAlchemy/pymysql, extrai os dados reais)
        ↓
Power BI (importa dados.csv, extraído do MESMO banco)
```
Não há dados "inventados" isoladamente em nenhum módulo — todos os números, gráficos e insights derivam da mesma base populada no Módulo 2.

## 📖 Resumo de cada módulo

### 1. Modelagem de Dados
9 entidades (Pacientes, Médicos, Especialidades, Convênios, Consultas, Prontuários, Prescrições, Pagamentos e a associativa Médico_Especialidade), com PKs/FKs, cardinalidades e o relacionamento N:N entre Médicos e Especialidades resolvido via tabela associativa. Modelo normalizado até a 3FN (ex.: `prontuarios` e `pagamentos` não duplicam `id_paciente`/`id_medico`, obtidos via JOIN com `consultas`).

### 2. SQL
Banco `clinica_care` em MySQL/MariaDB, com todas as tabelas, constraints (`NOT NULL`, `UNIQUE`, `CHECK`, `PK`, `FK`), 12–20 registros realistas por tabela, 3 `UPDATE`s, 4 consultas de agregação (`COUNT`, `SUM`, `AVG`, `MIN`/`MAX`) e 4 `JOIN`s (incluindo uma window function `DENSE_RANK`). Script **testado do início ao fim em um MariaDB real**, sem erros.

### 3. Python + Machine Learning
Tema escolhido: **Previsão de No-Show**. O notebook conecta diretamente no banco MySQL, faz limpeza e engenharia de atributos (`dias_antecedencia`, `historico_faltas`, `idade_paciente`), gera os 3 gráficos exigidos (barras, pizza, linha) e treina uma Árvore de Decisão. Dado o volume pequeno de dados (exigência do próprio desafio, 12–20 registros por tabela), o notebook é transparente sobre as limitações estatísticas do modelo, comparando-o com um baseline trivial.

### 4. Power BI
Dashboard com os 5 visuais obrigatórios (barras, colunas, pizza, linha, KPIs/cards), filtros interativos (período, especialidade, médico, plano, status), medidas DAX documentadas e paleta de cores profissional. Construído com base no `dados.csv` extraído do mesmo banco `clinica_care`.

## 📊 Principais resultados

- Banco de dados com 9 tabelas relacionais, todas dentro do intervalo de 12–20 registros exigido.
- Receita total confirmada (pagamentos "Pago"): **R$ 2.803,00**.
- Taxa de ocupação geral: **75%** das consultas agendadas foram efetivamente realizadas.
- Taxa de no-show geral: **18,8%**, com queda de 25% (janeiro) para 12,5% (fevereiro/2026).
- Modelo de Machine Learning funcional de ponta a ponta (extração → features → treino → avaliação → interpretação), com avaliação honesta de suas limitações estatísticas dado o volume de dados.

## 💡 Principais insights

- **Cardiologia, Neurologia e Psiquiatria** têm os maiores tickets médios pagos — boas candidatas a priorização de agenda.
- **Particular e Unimed JP** concentram a maior parte da base de pacientes — relevante para negociação de tabelas com convênios.
- A queda na taxa de no-show entre janeiro e fevereiro sugere que ações informais de lembrete já vêm funcionando, reforçando o valor de formalizar esse processo (ex.: usando o próprio modelo de ML para priorizar contatos).
- Com um histórico maior de dados reais, a mesma pipeline (SQL → Python → Power BI) já está pronta para escalar sem nenhuma mudança estrutural.

## ▶️ Instruções para executar o projeto

1. **Banco de dados**: instale o MySQL/MariaDB, rode `2_SQL/clinica_care.sql` (`mysql -u root < clinica_care.sql`) para criar e popular o schema `clinica_care`.
2. **Python**: abra `3_Python/analise_clinica.ipynb` no Jupyter ou Google Colab, ajuste a string de conexão do banco (usuário/senha/host) na primeira célula, e execute todas as células em ordem.
3. **Power BI**: abra `4_Power_BI/Dashboard_ClinicaCare.pbix` no Power BI Desktop. Caso o arquivo não abra (por incompatibilidade de versão do preview PBIR), siga o guia em `Insights_Dashboard.docx` para reconstruir o dashboard em poucos minutos a partir de `dados.csv`.
4. **Modelagem**: consulte `1_Modelagem/Modelo_Conceitual_ER.pdf` e `Modelo_Logico.txt` para reproduzir o diagrama em Draw.io, Lucidchart ou MySQL Workbench, se necessário.

---
*Squad Líder: Gabriel Lucas de Araujo Bandeira — Fábrica de Software, Workshop de Dados 26.2*
