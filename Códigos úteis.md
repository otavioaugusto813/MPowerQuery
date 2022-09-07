# Função Switch em M
## Código 
 ```jsx
    let
        // Created by Daniil Maslyuk
        Switch =
            let
                Function = (input as list) as any =>
                    let
                        Source = List.Buffer(input),
                        Expression = List.First(Source),
                        SkipExpression = List.Skip(Source),
                        HasElse = Number.IsEven(List.Count(Source)),
                        ElseValue = if HasElse then List.Last(Source) else null,
                        ValuesResults = List.RemoveLastN(SkipExpression, Number.From(HasElse)),
                        Values = List.Alternate(ValuesResults, 1, 1, 1),
                        Results = List.Alternate(ValuesResults, 1, 1, 0),
                        FirstResult = List.PositionOf(Values, Expression),
                        FunctionResult = if FirstResult = -1 then ElseValue else Results{FirstResult}
                    in
                        FunctionResult,
                FunctionType = type function (input as list) as any
                    meta [
                        Documentation.Name = "Switch",
                        Documentation.LongDescription = "Evaluates an expression against a list of values and returns one of multiple possible result expressions.",
                        Documentation.Examples = {
                            [Description = "Simple Switch expression", Code = "Switch({2, 1, ""A"", 2, ""B""})", Result = """B"""],
                            [Description = "An equivalent of SWITCH(TRUE... in DAX", Code = "Switch({true, 1 > 2, ""A"", 1 < 2, ""B"", ""No result""})", Result = """B"""]
                        }
                    ],
                TypedFunction = Value.ReplaceType(Function, FunctionType)
            in
                TypedFunction
    in
        Switch
    ```
    
   #### Fonte
    [](https://xxlbi.com/blog/switch-true-in-power-query/)[https://xxlbi.com/blog/switch-true-in-power-query/](https://xxlbi.com/blog/switch-true-in-power-query/)
    

# Substituir valor baseado em condicional

-   Código e Fonte
    
    ```jsx
    Table.ReplaceValue("#PassoAnterior",
    	each [ColunaSubstituição],
    		each if [OutraColuna] = "ValorBuscado" then "ValorNovo"
    			else [ColunaSubstituição],
    				Replacer.ReplaceText, {"ColunaSubstituição})
    ```
    

# Filtrar valor baseado em valores que não estão na lista

-   Código e Fonte
    
    ```jsx
    Table.AddColumn("PassoAnterior", "Check" each if [Payment] = 1 and not(List.Contains([paycode],"BADDEBT","WRITEOF")) then "Paid" else "N/A")
    ```
    

# Adicionar texto a uma coluna existente

-   Código
    
    ```jsx
    StepName = Table.TransformColumns(
      Source,
      {
        {
          "name", 
          each 
            Text.Combine({"ABC",(_)}),
          type text
        }
      }
    )
    ```
    
    [Eliminar dígitos de um texto sem adicionar nova coluna](Eliminar%20dígitos%20de%20um%20texto%20sem%20adicionar%20nova%20coluna.md)    ```