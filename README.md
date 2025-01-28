# DioHeinekeinModelandoDoZero(OS)
Desafio - Dio X Heinekein X MySQL Workbench (modelando o processo de gerenciamento de ordens de serviços para uma oficina mecânica).

# Roteiro de leitura README.md e Diagrama Entidade-Relacionamento:

A) Definição dos objetivos.

B) Narrativa original.

C) Desenvolvimento do Modelo ERR.

## A) Objetivos
Utilizando MySQL Workbench, conhecimentos adquiridos em aula e experiência, modelar do zero um sistema de controle e gerenciamento de execução de ordens de serviço em uma oficina mecânica, criando o esquema conceitual.

Temos como objetivo final dar ao usuário - mecânico, que na abertura das ordens de serviços, ele irá ouvir o cliente e listar as suas necessidades. Assim, enquanto inspeciona o veículo, ele abre a OS e víncula ali as peças e serviços que julgar necessários, bem como faz a ligação ao(s) mecânico(s) que irá(ão) executar os serviços, determinando também datas previstas de início e fim para cada serviço a ser executado.

Ao término, ele apresenta ao cliente as ordens de serviço ainda como orçamento, para aprovação ou rejeição.

## B) Narrativa original

- Sistema de controle e gerenciamento de execução de ordens de serviço em uma oficina mecânica.
- Clientes levam seus veículos (um cliente pode levar mais de um veículo) à oficina mecânica para serem consertados ou para passarem por revisões periódicas (tipos: conserto e revisão).
- Cada veículo é designado a uma equipe de mecânicos que identifica os serviços a serem executados e preenche uma OS com data de entrega.
- A partir da OS, calcula-se o valor de cada serviço, consultando-se uma tabela de referência de mão-de-obra (tabela de preços).
- O valor de cada peça também irá compor a OS (valor = peças + mão-de-obra).
- O cliente autoriza a execução dos serviços propostos pela oficina mecânica (orçamento).
- A mesma equipe avalia e executa os serviços.
- Os mecânicos possuem código, nome, endereço e especialidade.
- Cada OS possui: nº, data de emissão, um valor (peças e mão-de-obra), status e uma data para conclusão dos trabalhos.
- Uma OS pode ser composta por vários serviços e um mesmo serviço pode estar contido em mais de uma OS;
- Uma OS pode ter vários tipos de peça e uma peça pode estar presente em mais de uma OS.

## C) Desenvolvimento do modelo ERR

A seguir iremos listar o desenvolvimento do esquema conceitual para o contexto da oficina com base na narrativa fornecida.

## 1. Clientes:
- Criamos a tabela CLIENTE para armazenar os dados de identificação dos diversos clientes.
- A tabela clientes terá vínculos com as tabelas VEICULO e OS (ordens de serviço).
- Criamos a tabela VEICULOSPORCLIENTE para armazenar a narrativa do cliente quanto as suas necessidades com relação ao veículo em questão na data e assim, podemos manter um histórico das considerações do cliente quanto ao veículo, pois o cliente pode, eventualmente decidir por não avançar com a execução dos serviços da OS e a oficina perderia assim o histórico do caso.
- Um cliente poderá ter mais de um veículo.
- Um cliente poderá ter mais de uma ordem de serviço.
  
## 2. Veículos:
- Criamos a tabela VEICULO para armazenar os dados de identificação dos veículos por cliente.
- A sua vinculação com a tabela CLIENTE gerada em VEICULOSPORCLIENTE, como já colocamos, irá manter a eventual narrativa histórica do que o cliente entende que precisa ser consertado (ou revisado) em seu veículo.
- Um cliente poderá ter mais de um veículo.
- Um veículo poderá ter mais de uma ordem de serviço.

## 3. Peças:
- Criamos a tabela PECA para armazenar os dados de identificação das diversas peças disponíveis para a oficina mecânica, bem como o seu preço para o cliente.
- Consideramos que a codificação das peças obedece o critério "part number".
- Consideramos que o estoque de peças encontra-se a disposição imediata para início dos serviços, seja isso em formato interno ou externo.
- A tabela PECA vincula-se a tabela de ordens de serviço - OS, através de PECASPOROS, que mantém o histórico de requisição de peças por ordem de serviços, com os dados de quantidade, valor e data de utilização das peças, tanto a nivel de previsão (orçamento), como da realização dos serviços.
- As peças podem ser utilizadas em diferentes ordens de serviço.
  
## 4. Serviços:
- Criamos a tabela SERVICO para definir os diversos serviços prestados pela oficina mecânica, a qual está categorizada através da tabela CATEGORIAS, a qual é vinculada aos mecânicos através de CATEGORIASPORMECANICO enquanto definição de especialidade ou capacidade laboral.
- A tabela SERVICO será a tabela de preços de serviços do negócio, pois também armazena os preços dos diversos serviços oferecidos aos clientes, porém, levando em conta agora a sua complexidade.
- Como critério de complexidade estamos definindo uma descrição crítica, bem como marca, modelo e ano do veículo do cliente.
- Consideramos que uma revisão periódica ou espontânea é um tipo de serviço oferecido pela mecânica.

## 5. Mecânicos:
- Criamos a tabela MECANICO para armazenar os dados de identificação dos mecânicos.
- Vinculada a tabela MECANICO, temos a tabela CATEGORIA, que irá trazer a especialização necessária para cada indivíduo da equipe de mecânicos.
- Por REGRA DO NEGÓCIO, está sendo definida a seguinte tabela inicial de categorias de serviço, as quais abrigarão os diferentes tipos de serviços em níveis JÚNIOR, PLENO E SENIOR:
  - Lanternagem: serviços de pintura, chapeação, polimento, amassamentos e manutenções diversas na lataria, para-choques, vidros e rodas.
  - Elétrica: serviços no sistema elétrico e eletrônico.
  - Hidráulica: serviços no sistema hidráulico.
  - Mecânica: que envolvam a macânica.
  - Suspensão: serviços que envolvam pneus, suspensão, amortecedores, molas e estabilidade.
  - Geral: que nao se enquadram em nenhum dos ítens acima. 

## 6. OS - Ordens de Serviço:
- Criamos a tabela OS para armazenar as diversas ordens de serviço abertas para os veículos dos clientes, suas datas globais previstas e realizadas de início e fim, seus custos totais (por cliente e veículo), bem como campos para registro de observações.
- Vinculadas a tabela OS, temos:
  - PECASPOROS: para fazer a armazenagem das diversas peças utilizadas em cada OS, tanto a nível previsional (orçamento) como o efetivamente realizado.
  - SERVICOSPOROS: para fazer a armazenagem dos diversos serviços executados em cada OS, tanto a nível previsional (orçamento) como o efetivamente realizado.
  - CLIENTE: trazer os dados dos clientes para as Ordens de Serviços.
  - VEICULO: trazer os dados dos veículos para as Ordens de Serviços.
  - OSPORMECANICO: disponibilizar dados dos mecânicos para programação e alocação dos serviços consoante as suas habilidades.
- Na abertura das ordens de serviços, o responsável irá ouvir o cliente e listar as suas necessidades enquanto inspeciona o veículo, ele abre a OS e víncula ali as peças e serviços que julgar necessários, bem como faz a ligação ao mecânico que irá executar os serviços, determinando também datas previstas de início e fim para cada serviço a ser executado.
- As ordens de serviço poderão ter os seguintes status iniciais por REGRA DO NEGÓCIO:
  - Aguardando aprovação de cliente
  - Aprovada para execução
  - Aguardando peças
  - Aguardando liberação de box de serviço
  - Em execução
  - Aguardando revisão dos serviços
  - Em revisão dos serviços
  - Concluída e aguardando pagamento
  - Paga
  - Entregue e aprovada pelo cliente

## Projeto Lógico:

Consta anexo neste repositório os arquivos PNG e MWB com o projeto lógico.

## Ferramentas:

- MySQL Workbench
- Git / Github
