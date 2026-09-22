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
- Power Query (Power BI) - modelagem e transformações adicionais

## Serviço usado:

- Github

## Bibliotecas Python

- Pandas
- py7zr
- Requests

## Informações do Projeto:

### 1 - Extração e Tratamento dos Dados (Python)

Hoje, o download dos microdados do Novo CAGED é feito manualmente no
portal oficial do PDET (Ministério do Trabalho), em arquivos `.7z`.
Cada arquivo mensal pode conter milhões de registros de movimentação
em todo o Brasil, o que exige um processamento cuidadoso da memória.
Por isso, a leitura é feita em **pedaços (chunks)** — o script
processa o arquivo em blocos de 200 mil linhas por vez, em vez de
carregar tudo de uma só vez, evitando travamentos mesmo em máquinas
com recursos limitados.

A partir do download manual, o processo é automatizado: o script
extrai os arquivos `.7z`, filtra (ainda durante a leitura em chunks)
só as linhas do município de interesse e trata os dados com Pandas
(mapeamento de códigos para descrições, tipos, cálculo de saldo etc).
A base final é enriquecida com dados do IPEA (INPC e salário mínimo)
para permitir análise de salário em valores reais, não só nominais.

**Próximo passo:** automatizar também o download direto do FTP
oficial, eliminando a etapa manual.

### 2 - Modelagem no Power Query

Nesta primeira versão exploratória do projeto, algumas manipulações
adicionais foram feitas diretamente no Power Query (dentro do próprio
Power BI), como: filtro para o município de Guaxupé, remoção de
colunas redundantes, criação de colunas de ordenação (para manter a
sequência lógica de grau de instrução nos gráficos), agrupamento do
grau de instrução em categorias mais amplas (Fundamental, Médio,
Superior, Pós-graduação) e simplificação de textos longos de algumas
categorias.

O objetivo é, nas próximas versões, migrar todas essas manipulações
para o script Python (`consolidar_caged.py`), centralizando todo o
tratamento de dados numa única etapa e deixando o Power Query
responsável só pela leitura da base já pronta.

### 3 - Aba Visão Geral (Power BI)

![visao geral](./imagens/aba1_visao_geral.png)

Essa aba traz um resumo do mercado de trabalho formal de Guaxupé/MG:
cartões com admissões, desligamentos, saldo do período, salário médio
e taxa de rotatividade, além de gráficos de saldo por setor de
atividade, tipos de desligamento, top ocupações com maior
movimentação e a evolução mensal de admissões x desligamentos.

### 4 - Aba Perfil do Trabalhador (Power BI)

![perfil trabalhador](./imagens/aba2_perfil_trabalhador.png)

Essa aba detalha o perfil de quem foi admitido/desligado em
Guaxupé/MG no período: distribuição por sexo, raça/cor, grau de
escolaridade, faixa etária, tipo de deficiência e salário médio
(nominal e real, deflacionado com o INPC via API do IPEA).

### 5 - Aba Ocupações (Power BI)

![ocupacoes](./imagens/aba3_ocupacoes.png)

Essa aba apresenta o detalhamento por ocupação (CBO): uma tabela
completa com admissões, desligamentos e salário médio por ocupação, e
um treemap que representa visualmente o volume de movimentação de cada
ocupação, colorido conforme o saldo (positivo ou negativo).

### 6 - Medidas DAX

O painel utiliza medidas DAX customizadas para os principais
indicadores, entre elas:

- **Admissões / Desligamentos**: contagem de movimentações filtradas
  por tipo (admissão ou desligamento), com o saldo de desligamentos
  ajustado para valor positivo.
- **Saldo do Período**: soma do saldo ajustado (já considerando as
  exclusões declaradas fora do prazo).
- **Taxa de Rotatividade**: proporção entre desligamentos e admissões
  no período.
- **Cor Saldo**: medida de formatação condicional que retorna verde
  ou vermelho conforme o saldo é positivo ou negativo, usada em
  cartões, gráficos de barras e no treemap.
- **Média Salarial (com sigilo)**: aplica sigilo estatístico,
  suprimindo a média salarial de categorias com poucos registros, para
  evitar identificar trabalhadores individualmente.

As medidas completas estão disponíveis em [`/dax/medidas.txt`](./dax/medidas.txt).

## Fonte dos dados

Os microdados são públicos, disponibilizados pelo Ministério do
Trabalho e Emprego através do PDET: http://pdet.mte.gov.br/novo-caged

## Autor

- **Matheus Bonfim do Prado**: @bonfimdoprado (<https://github.com/bonfimdoprado>)
