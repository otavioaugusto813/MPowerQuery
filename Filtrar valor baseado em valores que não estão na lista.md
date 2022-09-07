# Filtrar valor baseado em valores que não estão na lista

-   Código e Fonte
    
    ```jsx
    Table.AddColumn("PassoAnterior", "Check" each if [Payment] = 1 and not(List.Contains([paycode],"BADDEBT","WRITEOF")) then "Paid" else "N/A")
