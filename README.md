Sistema de paginação para PowerBI com PowerQuery
# Integração SIENGE com Power BI

Este repositório contém um exemplo de como integrar a API do SIENGE com o Power BI, utilizando paginação via offset.

## Como usar

1. Substitua o URL da API no código pela URL da sua instância do SIENGE. (De exemplo, usarei uma api que puxa o planejamento.)
2. Use a função `GETAPIDATA` para puxar os dados da API.
3. Crie uma tabela auxiliar de offset para lidar com a paginação.

## Exemplo de código
```powerquery
Função GetAPIData
= (Offset as number) as table =>
let
    Fonte = Json.Document(Web.Contents(
        "https://api.sienge.com.br/{SeuDomínio}/public/api/v1/building-projects/{NúmeroDoProjeto}/sheets/2/tasks?offset=0&limit=200", 
        [Query=[offset=Text.From(Offset), limit="200"]]
    )),
    Dados = Fonte[results],
    Tabela = Table.FromRecords(Dados)
in
    Tabela
```
```
Função OffsetList
= List.Generate(() => 0, each _ < 1600, each _ + 200)
```
```
Função Coluna para puxar a API
= try GetAPIData([Offset]) otherwise null
```
