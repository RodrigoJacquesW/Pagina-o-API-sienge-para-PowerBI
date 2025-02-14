Sistema de paginação para PowerBI com PowerQuery
# Integração SIENGE com Power BI

Este repositório contém um exemplo de como integrar a API do SIENGE com o Power BI, utilizando paginação via offset.

## Como usar

1. Substitua o URL da API no código pela URL da sua instância do SIENGE.
2. Use a função `GETAPIDATA` para puxar os dados da API.
3. Crie uma tabela auxiliar de offset para lidar com a paginação.

## Exemplo de código

```powerquery
= (Offset as number) as table =>
let
    Fonte = Json.Document(Web.Contents(
        "https://api.sienge.com.br/{SeuDomínio}/public/api/v1/building-projects/{project_id}/sheets/{sheet_id}/tasks", 
        [Query=[offset=Text.From(Offset), limit="200"]]
    )),
    Dados = Fonte[results],
    Tabela = Table.FromRecords(Dados)
in
    Tabela
