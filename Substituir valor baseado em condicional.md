# Substituir valor baseado em condicional

-   Código e Fonte
    
    ```jsx
    Table.ReplaceValue("#PassoAnterior",
    	each [ColunaSubstituição],
    		each if [OutraColuna] = "ValorBuscado" then "ValorNovo"
    			else [ColunaSubstituição],
    				Replacer.ReplaceText, {"ColunaSubstituição"})