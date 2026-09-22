# Painel de Mercado de Trabalho Formal (CAGED) - Guaxupé/MG

![banner](./imagens/banner.png)

## Descrição do Projeto

Este projeto tem como objetivo realizar a extração, tratamento e análise
dos microdados do Novo CAGED (Cadastro Geral de Empregados e
Desempregados), com foco no mercado de trabalho formal de Guaxupé/MG,
além de desenvolver um painel interativo em Power BI para visualização
dos indicadores de emprego, perfil do trabalhador e ocupações da região.

## Tecnologia

Os softwares utilizados neste projeto foram:

- Jupyter / Python 3
- Power BI

## Serviço usado:

- Github

## Bibliotecas Python

- Pandas
- py7zr
- Requests

## Informações do Projeto:

### 1 - Aba Visão Geral (Power BI)

![visao geral](./imagens/aba1_visao_geral.png)

Essa primeira aba traz um resumo executivo do mercado de trabalho local:
cartões com admissões, desligamentos, saldo do período, salário médio e
taxa de rotatividade, além de gráficos de saldo por setor de atividade,
tipos de desligamento, top ocupações com maior movimentação e a
evolução mensal de admissões x desligamentos.

### 2 - Aba Perfil do Trabalhador (Power BI)

![perfil trabalhador](./imagens/aba2_perfil_trabalhador.png)

Essa aba detalha o perfil de quem foi admitido/desligado no período:
distribuição por sexo, raça/cor, grau de escolaridade, faixa etária,
tipo de deficiência e salário médio (nominal e real, deflacionado com
o INPC via API do IPEA).

### 3 - Aba Ocupações (Power BI)

![ocupacoes](./imagens/aba3_ocupacoes.png)

Essa aba apresenta o detalhamento por ocupação (CBO): uma tabela
completa com admissões, desligamentos e salário médio por ocupação, e
um treemap que representa visualmente o volume de movimentação de cada
ocupação, colorido conforme o saldo (positivo ou negativo).

### 4 - Extração e Tratamento dos Dados (Python)

Os microdados são extraídos automaticamente do FTP oficial do
Ministério do Trabalho, filtrados por município de interesse e
tratados com Pandas (mapeamento de códigos para descrições, tipos,
cálculo de saldo etc). A base final é enriquecida com dados do IPEA
(INPC e salário mínimo) para permitir análise de salário em valores
reais, não só nominais.

## Fonte dos dados

Os microdados são públicos, disponibilizados pelo Ministério do
Trabalho e Emprego através do PDET: http://pdet.mte.gov.br/novo-caged

## Autor

- **Matheus Bonfim do Prado**: @bonfimdoprado (<https://github.com/bonfimdoprado>)
