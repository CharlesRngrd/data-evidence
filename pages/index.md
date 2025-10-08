---
title: Que se passe-t'il en France ?
---

```sql classement_parti
  select
      candidat_parti, count() as siege_nombre
  from data_legislative.data_legislative
  where candidat_rang = 1
  group by candidat_parti
```

<BarChart
    data={classement_parti}
    title="Nombre de sièges par parti"
    x=candidat_parti
    y=siege_nombre
    swapXY=true
/>

