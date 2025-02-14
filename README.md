Sistema de paginação para PowerBI com PowerQuery
# Integração SIENGE com Power BI

Este repositório contém um exemplo de como integrar a API do SIENGE com o Power BI, utilizando paginação via offset.

## Como usar

1. Abra o PowerQuery.
2. Crei uma função nula e use a função `GETAPIDATA` para puxar os dados da API.
3. Substitua o URL da API no código pela URL da sua instância do SIENGE. (De exemplo, usarei uma api que puxa o planejamento.)
4. Crie outra tabela usando a consulta nula e utilizando a função OffsetList.
5. Por fim, na tabela Offset, crie uma coluna personalizada com a formula de puxar a API

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
