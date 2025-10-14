---
title: Que se passe-t'il en France ?
---

```sql classement_candidat_parti
    select
        candidat_parti, count() as siege_nombre
    from data_legislative.data_legislative
    where candidat_rang = 1
    and annee = ${inputs.selected_annee}
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

```sql simulation_candidat_rn
    select
        *,
        if(
            candidat_rang = 1, 'Victoire 2024', 'Victoire potentielle'
        ) as victoire_category
    from data_legislative.data_legislative
    where candidat_parti = 'Rassemblement National'
    and annee = 2024
    and (
        candidat_rang = 1
        or candidat_rang_1_ecart >= ${inputs.gain_point_vote} * -1
        or candidat_voix_pourcentage + ${inputs.gain_point_vote} >= 50
    )
```

```sql simulation_candidat_rn_total
    select
        count() as total
    from ${simulation_candidat_rn}
```

```sql liste_candidat_parti
    select
        distinct candidat_parti
    from data_legislative.data_legislative
    where candidat_rang = 1
    order by candidat_parti
```

![Drapeau Français](https://raw.githubusercontent.com/CharlesRngrd/data-evidence/refs/heads/master/french-flag.png)

<ECharts
    config={{
        title: {
            text: 'Composition du parlement',
            left: 'left',
        },
        tooltip: {
            trigger: 'item',
            formatter: '{b}: {c} députés ({d}%)'
        },
        series: [
            {
                type: 'pie',
                radius: ['100%', '160%'],
                center: ['50%', '100%'],
                startAngle: 180,
                endAngle: 360,
                data: classement_candidat_parti.map(d => ({
                    name: d.candidat_parti,
                    value: d.siege_nombre
                })),
                label: {
                    show: true,
                    position: 'outside'
                },
                labelLine: {
                    show: true
                },
                itemStyle: {
                    borderRadius: 6,
                    borderColor: '#fff',
                    borderWidth: 2
                }
            }
        ]
    }}
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

<Slider
    title="Gain de points" 
    name=gain_point_vote
    min=0
    max=10
/>

<BigValue 
  data={simulation_candidat_rn_total} 
  value=total
/>

<AreaMap
    data={simulation_candidat_rn}
    areaCol=circonscription_code
    geoJsonUrl='https://raw.githubusercontent.com/CharlesRngrd/data_demo/refs/heads/main/assets/map.geojson'
    geoId=codeCirconscription
    value=victoire_category
    height=700
    borderWidth=0.5
    borderColor=#fff
    opacity=1
    selectedColor=warning
    basemap={"https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}"}
/>
