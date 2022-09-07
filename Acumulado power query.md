``` Power Query
Personalizar1 = Table.Buffer(#"Outras Colunas Removidas"),
    #"Linhas Filtradas1" = Table.SelectRows(Personalizar1, each ([Depreciação normal lançada do ano] <> null)),
    #"Tipo Alterado2" = Table.TransformColumnTypes(#"Linhas Filtradas1",{{"Nº principal do imobilizado", Int64.Type}, {"Exercício", Int64.Type}, {"Período contábil da depreciação", Int64.Type}}),
    #"Índice Adicionado" = Table.AddIndexColumn(#"Tipo Alterado2", "Índice", 1, 1, Int64.Type),
    #"Linhas Classificadas" = Table.Sort(#"Índice Adicionado",{{"Nº principal do imobilizado", Order.Ascending}, {"Exercício", Order.Ascending}, {"Período contábil da depreciação", Order.Ascending}}),
    #"Linhas Agrupadas" = Table.Group(#"Linhas Classificadas", {"Nº principal do imobilizado"}, {{"LinhaMin", each List.Min([Índice]), type number}}),
    #"Consultas Mescladas1" = Table.NestedJoin(#"Índice Adicionado", {"Nº principal do imobilizado"}, #"Linhas Agrupadas", {"Nº principal do imobilizado"}, "Linhas Agrupadas", JoinKind.LeftOuter),
    #"Linhas Agrupadas Expandido" = Table.ExpandTableColumn(#"Consultas Mescladas1", "Linhas Agrupadas", {"LinhaMin"}, {"LinhaMin"}),
    #"Personalização Adicionada2" = Table.AddColumn(#"Linhas Agrupadas Expandido", "RangeLista", each List.Range(
    #"Linhas Agrupadas Expandido"[Depreciação normal lançada do ano],
    [LinhaMin] -1,
    [Índice] - [LinhaMin] + 1
```