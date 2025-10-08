---
title: Que se passe-t'il en France ?
---

```sql classement_candidat_parti
    select
        candidat_parti, count() as siege_nombre
    from data_legislative.data_legislative
    where candidat_rang = 1
    and annee = 2024
    group by candidat_parti
```

```sql liste_candidat_rang_1
    select
        *
    from data_legislative.data_legislative
    where candidat_rang = 1
    and candidat_parti in ${inputs.selected_candidat_parti.value}
    and annee = ${inputs.selected_annee}
```

```sql liste_candidat_parti
    select
        distinct candidat_parti
    from data_legislative.data_legislative
    where candidat_rang = 1
    order by candidat_parti
```

![Drapeau Français](https://raw.githubusercontent.com/CharlesRngrd/data-evidence/refs/heads/master/french-flag.png)

<BarChart
    data={classement_candidat_parti}
    title="Nombre de sièges par parti"
    x=candidat_parti
    y=siege_nombre
    swapXY=true
/>

<Dropdown
    name=selected_candidat_parti
    title="Choisissez des partis"
    data={liste_candidat_parti}
    value=candidat_parti
    multiple=true
    selectAllByDefault=true
/>

<ButtonGroup name=selected_annee>
    <ButtonGroupItem valueLabel="Législatives 2024" value=2024 default />
    <ButtonGroupItem valueLabel="Législatives 2022" value=2022 />
</ButtonGroup>

<AreaMap
    data={liste_candidat_rang_1}
    areaCol=circonscription_code
    geoJsonUrl='https://raw.githubusercontent.com/CharlesRngrd/data_demo/refs/heads/main/assets/map.geojson'
    geoId=codeCirconscription
    value=candidat_parti
    height=700
    borderWidth=0.5
    borderColor=#fff
    opacity=1
    selectedColor=warning
    basemap={"https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}"}
/>
