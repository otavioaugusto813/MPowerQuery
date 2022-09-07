```markup
List.Accumulate(
                {0..(Date.Year([award_end_date])-Date.Year([award_begin_date]))*12+(Date.Month([award_end_date])-Date.Month([award_begin_date]))},
                {},
                (s,c) => s&{Date.AddMonths(begin,c)}
            )
```
