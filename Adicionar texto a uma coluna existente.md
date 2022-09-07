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
