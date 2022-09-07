```js
let
    months=Record.FromList(List.Repeat({0},12), {"1".."9", "10","11","12"}),
    Origine = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45WMjQyVtJRMjIwMgRSIKahgVKsDpq4GUjcFFPcEsTEIm5oBDIMzSCQmDkWC0DiFggLjIxNkQyCqo8FAA==", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [account = _t, year = _t, month = _t, amount = _t]),
    #"Modificato tipo" = Table.TransformColumnTypes(Origine,{{"account", Int64.Type}, {"year", Int64.Type}, {"month", type text}, {"amount", Int64.Type}}),
    #"Raggruppate righe" = Table.Group(#"Modificato tipo", {"account", "year"}, {"all", each Record.ToTable(months&Record.FromList([amount],[month]))}),
    #"Personalização Adicionada" = Table.AddColumn(#"Raggruppate righe", "Teste", each Table.ReplaceValue([all], 0, null, Replacer.ReplaceValue,{"Value"})),
    #"Personalização Adicionada1" = Table.AddColumn(#"Personalização Adicionada", "FillDown", each Table.FillDown([Teste], {"Value"})),
    #"Colunas Removidas" = Table.RemoveColumns(#"Personalização Adicionada1",{"all", "Teste"}),
    #"FillDown Expandido" = Table.ExpandTableColumn(#"Colunas Removidas", "FillDown", {"Name", "Value"}, {"Name", "Value"})
in
    #"FillDown Expandido"
```



let
    AvaliaçãoMaterialHistórico  = PowerBI.Dataflows(null),
    #"d887e6c5-aa5a-4efb-a7fd-9de6cc441f9c" = Fonte{[workspaceId="d887e6c5-aa5a-4efb-a7fd-9de6cc441f9c"]}[Data],
    #"6ea55456-f8ed-4b31-9db6-c641ff0b494e" = #"d887e6c5-aa5a-4efb-a7fd-9de6cc441f9c"{[dataflowId="6ea55456-f8ed-4b31-9db6-c641ff0b494e"]}[Data],
    AvaliaçãoMaterialHistórico1 = #"6ea55456-f8ed-4b31-9db6-c641ff0b494e"{[entity="AvaliaçãoMaterialHistórico"]}[Data],
    months=Record.FromList(List.Repeat({0},12), {"1".."9", "10","11","12"}),
    #"Linhas Classificadas" = Table.Sort(AvaliaçãoMaterialHistórico,{{"Nº do material", Order.Ascending}, {"Área de avaliação", Order.Ascending}, {"Tipo de avaliação", Order.Ascending}, {"Exercício do período atual", Order.Ascending}, {"Período corrente período contábil", Order.Ascending}}),
   TabelaAgrupada = Table.Group( #"Linhas Classificadas", {"Nº do material", "Área de avaliação", "Tipo de avaliação"}, {"all", each Record.ToTable(months&Record.FromList([Preço médio móvel],[months]))})

in
   TabelaAgrupada
   