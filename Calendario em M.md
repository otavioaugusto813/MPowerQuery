```js

let
    Fonte = #date(2020,1,1),
    Personalizar1 = List.Dates(Fonte, Number.From(Date.EndOfYear(Date.From(DateTime.LocalNow())))-Number.From(Fonte), #duration(1,0,0,0)),
    #"Convertido para Tabela" = Table.FromList(Personalizar1, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Tipo Alterado4" = Table.TransformColumnTypes(#"Convertido para Tabela",{{"Column1", type date}}),
    #"Data Max" = Table.AddColumn(#"Tipo Alterado4", "Data Max", each #date(2040,1,1)),
    #"Tipo Alterado3" = Table.TransformColumnTypes(#"Data Max",{{"Data Max", type date}, {"Column1", type date}}),
    #"Colunas Renomeadas" = Table.RenameColumns(#"Tipo Alterado3",{{"Column1", "Data"}}),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Colunas Renomeadas",{{"Data", type date}}),
    #"Ano Inserido" = Table.AddColumn(#"Tipo Alterado", "Ano", each Date.Year([Data]), Int64.Type),
    #"Mês Inserido" = Table.AddColumn(#"Ano Inserido", "Mês", each Date.Month([Data]), Int64.Type),
    #"Personalização Adicionada" = Table.AddColumn(#"Mês Inserido", "Ano Mês Ref", each ([Ano]*100)+[Mês]),
    #"Tipo Alterado1" = Table.TransformColumnTypes(#"Personalização Adicionada",{{"Ano Mês Ref", Int64.Type}}),
    #"Nome do Mês Inserido" = Table.AddColumn(#"Tipo Alterado1", "Nome do Mês", each Date.MonthName([Data]), type text),
    #"Personalização Adicionada1" = Table.AddColumn(#"Nome do Mês Inserido", "Mês Abrev", each Date.ToText([Data], "MMM")),
    #"Coluna Mesclada Inserida" = Table.AddColumn(#"Personalização Adicionada1", "Mês e Ano", each Text.Combine({[Mês Abrev], "-", Text.From([Ano], "pt-BR")}), type text),
    #"Tipo Alterado2" = Table.TransformColumnTypes(#"Coluna Mesclada Inserida",{{"Ano", type text}, {"Mês", type text}, {"Ano Mês Ref", type text}}),
    #"Linhas Classificadas" = Table.Sort(#"Tipo Alterado2",{{"Ano", Order.Ascending}}),
    #"Coluna Mesclada Inserida1" = Table.AddColumn(#"Linhas Classificadas", "Num Mês Abrev", each Text.Combine({[Mês], "-", [Mês Abrev]}), type text),
    #"Coluna Duplicada" = Table.AddColumn(#"Coluna Mesclada Inserida1", "Num Mês Abrev - Copiar", each [Num Mês Abrev], type text),
    #"Colunas Removidas" = Table.RemoveColumns(#"Coluna Duplicada",{"Num Mês Abrev - Copiar", "Num Mês Abrev"}),
    #"Tipo Alterado5" = Table.TransformColumnTypes(#"Colunas Removidas",{{"Mês", Int64.Type}, {"Mês Abrev", type text}}),
    #"Nome do Dia Inserido" = Table.AddColumn(#"Tipo Alterado5", "Nome do Dia", each Date.DayOfWeekName([Data]), type text),
    #"Tipo Alterado6" = Table.TransformColumnTypes(#"Nome do Dia Inserido",{{"Mês e Ano", type date}}),
    #"Colocar Cada Palavra Em Maiúscula" = Table.TransformColumns(#"Tipo Alterado6",{{"Nome do Mês", Text.Proper, type text}, {"Mês Abrev", Text.Proper, type text}, {"Nome do Dia", Text.Proper, type text}})
in
    #"Colocar Cada Palavra Em Maiúscula"
```