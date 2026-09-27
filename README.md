# Painel de Mercado de Trabalho Formal (CAGED) - Guaxupé/MG

## Descrição do Projeto

Este projeto tem como objetivo realizar a extração, tratamento e análise dos microdados do Novo CAGED (Cadastro Geral de Empregados e Desempregados), com foco no mercado de trabalho formal de Guaxupé/MG, além de desenvolver um painel interativo em Power BI para visualização dos indicadores de emprego, perfil do trabalhador e ocupações da região.

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

<p align="center">
  <img width="660" height="531" alt="image" src="https://github.com/user-attachments/assets/69e397a8-cc18-414b-ab05-be83a582ebc0" />
</p>



O download dos microdados do Novo CAGED é automatizado diretamente do servidor FTP oficial do PDET (Ministério do Trabalho) pelo script baixar_caged.py: ele conecta, localiza o arquivo .7z de cada competência (mês/ano) e faz o download, já organizando em pastas no padrão AAAAMM.

Cada arquivo mensal pode conter milhões de registros de movimentação em todo o Brasil, o que exige um processamento cuidadoso da memória. Por isso, a leitura é feita em pedaços (chunks) — o script processa o arquivo em blocos de 200 mil linhas por vez, em vez de carregar tudo de uma só vez, evitando travamentos mesmo em máquinas com recursos limitados.

Depois do download, o processo continua automatizado: os arquivos .7z são extraídos, filtrados (ainda durante a leitura em chunks) só pelas linhas do município de interesse, e tratados com Pandas (mapeamento de códigos para descrições, tipos, cálculo de saldo etc). A base final é enriquecida com dados do IPEA (INPC e salário mínimo) para permitir análise de salário em valores reais, não só nominais.

**Próximo passo:** automatizar também o download direto do FTP
oficial, eliminando a etapa manual.


### 2 - Modelagem no Power Query

Nesta primeira versão exploratória do projeto, algumas manipulações adicionais foram feitas diretamente no Power Query (dentro do próprio Power BI), como: remoção de colunas redundantes, criação de colunas de ordenação (para manter a sequência lógica de grau de instrução nos gráficos), agrupamento  do grau de instrução em categorias mais amplas (Fundamental, Médio, Superior, Pós-graduação) e simplificação de textos longos de algumas categorias. 

O objetivo é, nas próximas versões, migrar todas essas manipulações para o script Python (`consolidar_caged.py`), centralizando todo o tratamento de dados numa única etapa e deixando o Power Query responsável só pela leitura da base já pronta.


### 3 - Aba Visão Geral (Power BI)

<p align="center">
<img width="1449" height="816" alt="aba_geral" src="https://github.com/user-attachments/assets/bde1f646-9de3-4c3b-88c8-7be02d766b19" />
</p>

Essa aba traz um resumo do mercado de trabalho formal de Guaxupé/MG: cartões com admissões, desligamentos, saldo do período, taxa de rotatividade e salário médio, além de gráficos de saldo por setor de atividade, tipos de desligamento, top 5 ocupações com maior movimentação (admissão e desligamento) e a evolução mensal de admissões x desligamentos. Um painel de filtros permite explorar os dados de forma interativa, com botões de navegação para as demais abas do painel.


### 4 - Aba Perfil do Trabalhador (Power BI)

<p align="center">
<img width="1453" height="808" alt="perfil_trbalhador" src="https://github.com/user-attachments/assets/b9a07c39-6347-400b-bae6-ef95bcbd2d6c" />
</p>

Essa aba detalha o perfil de quem foi admitido/desligado em Guaxupé/MG no período: distribuição por sexo, raça/cor, grau de escolaridade, faixa etária e tipo de deficiência. Todos os valores de salário exibidos usam o salário real, já deflacionado pelo INPC via API do IPEA. Isso garante uma comparação justa ao longo do tempo: sem esse ajuste, o salário pareceria sempre "crescer" apenas por efeito da inflação, mesmo que o poder de compra real das pessoas estivesse estagnado.

### 5 - Aba Ocupações (Power BI)

<p align="center">
<img width="1457" height="810" alt="ocupacao" src="https://github.com/user-attachments/assets/51e59028-5f90-449b-8776-281dbed546e9" />
</p>

Essa aba apresenta o detalhamento por ocupação (CBO): uma tabela completa com admissões, desligamentos e salário médio por ocupação, e um treemap que representa visualmente o volume de movimentação de cada ocupação, colorido conforme o saldo (positivo ou negativo).

Como são centenas de ocupações diferentes, a tabela conta com uma busca por nome, que filtra o resultado em tempo real:

<p align="center">
<img width="559" height="158" alt="ocupacao2" src="https://github.com/user-attachments/assets/366a2b98-a2e1-416d-8ab0-2f3d99a24060" />
</p>
<img width="570" height="240" alt="ocupacao1" src="https://github.com/user-attachments/assets/48347c2c-e5ed-437c-ad7a-7258d3e347a3" />  
</p>

### 6 - Medidas DAX

O painel utiliza medidas DAX customizadas para os principais indicadores, entre elas:

- **Admissões / Desligamentos**: contagem de movimentações filtradas por tipo (admissão ou desligamento), com o saldo de desligamentos  ajustado para valor positivo.
- **Saldo do Período**: soma do saldo ajustado (já considerando as exclusões declaradas fora do prazo).
- **Taxa de Rotatividade**: proporção entre desligamentos e admissões no período.
- **Cor Saldo**: medida de formatação condicional que retorna verde ou vermelho conforme o saldo é positivo ou negativo, usada em cartões, gráficos de barras e no treemap.
- **Média Salarial (com sigilo)**: aplica sigilo estatístico, suprimindo a média salarial de categorias com poucos registros, para evitar identificar trabalhadores individualmente.

## Fonte dos dados

Os microdados são públicos, disponibilizados pelo Ministério do Trabalho e Emprego através do PDET: http://pdet.mte.gov.br/novo-caged

## Autor

- **Matheus Bonfim do Prado**: @bonfimdoprado (<https://github.com/bonfimdoprado>)
