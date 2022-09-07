# Eliminar dígitos de um texto sem adicionar nova coluna
    
    ```jsx
    Table.ReplaceValue(#"PassoAnterior", each [Coluna], each Text.Remove([Coluna], {"0".."9"}), Replacer.ReplaceValue,{"Coluna"})


