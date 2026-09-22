# Painel de Mercado de Trabalho Formal (CAGED) — Guaxupé/MG

Projeto de ponta a ponta para extração, tratamento e visualização dos
microdados do Novo CAGED (Cadastro Geral de Empregados e Desempregados),
com foco no mercado de trabalho formal de Guaxupé/MG.

> Projeto pessoal, desenvolvido de forma independente com dados públicos
> oficiais do governo brasileiro. Não utiliza nem representa dados de
> nenhuma empresa ou instituição.

## 🎯 Objetivo

Transformar os microdados brutos do CAGED — disponibilizados em arquivos
`.txt` compactados, sem tratamento — em um painel interativo que permita
acompanhar admissões, desligamentos, perfil dos trabalhadores e ocupações
mais relevantes do mercado de trabalho local.

## 🛠️ Tecnologias utilizadas

- **Python** (pandas, py7zr, requests) — extração, tratamento e
  enriquecimento dos dados
- **API do IPEA** — indicadores macroeconômicos (INPC, salário mínimo)
  para deflacionar salários e calcular poder de compra real
- **Power BI / DAX** — modelagem, medidas customizadas e painel
  interativo

## 📊 O painel

3 páginas:

1. **Visão Geral** — admissões, desligamentos, saldo do período, saldo
   por setor de atividade, tipos de desligamento e ocupações com maior
   movimentação.
2. **Perfil do Trabalhador** — distribuição por sexo, raça/cor,
   escolaridade, faixa etária, tipo de deficiência e salário médio
   (nominal e real).
3. **Ocupações** — tabela completa e mapa visual (treemap) das
   ocupações por volume de movimentação e saldo.

*(Prints do painel na pasta `/imagens`)*

## 📁 Estrutura do repositório

```
├── scripts/
│   ├── extrair_caged.py        # baixa/extrai os microdados do FTP oficial
│   ├── consolidar_caged.py     # trata e junta os arquivos MOV/EXC
│   └── ipea_indicadores.py     # busca INPC e salário mínimo na API do IPEA
├── dax/
│   └── medidas.txt             # medidas DAX usadas no painel
├── imagens/
│   └── ...                     # prints do painel
└── README.md
```

## ▶️ Como rodar

1. Instale as dependências:
   ```
   pip install pandas py7zr requests
   ```
2. Ajuste os caminhos de pasta no topo de cada script (`PASTA_RAIZ`,
   `PASTA_APOIO`) para o seu ambiente local.
3. Rode `extrair_caged.py` para baixar/extrair os microdados de uma
   competência.
4. Rode `consolidar_caged.py` para gerar a base tratada.
5. (Opcional) Rode `ipea_indicadores.py` para adicionar salário real e
   salário mínimo à base.
6. Conecte o Power BI ao arquivo `.parquet` gerado.

## 📌 Fonte dos dados

Os microdados são públicos, disponibilizados pelo Ministério do
Trabalho e Emprego através do PDET:
http://pdet.mte.gov.br/novo-caged

## ⚠️ Nota sobre dados sensíveis

Este repositório contém apenas **código**. Os dados extraídos (que
incluem informação individual, como salário por trabalhador) **não são
versionados** — veja `.gitignore`. Ao reproduzir este projeto, trate
qualquer resultado com poucos registros por categoria com cautela
(sigilo estatístico), para evitar identificar indivíduos.

## 📫 Contato

Sinta-se à vontade para abrir uma *issue* ou me chamar no LinkedIn.
