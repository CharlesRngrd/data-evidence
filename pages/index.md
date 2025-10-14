---
title: Que se passe-t-il en France ?
sidebar: never
hide_toc: true
hide_beadcrumbs: true
---

<Image
    url="https://raw.githubusercontent.com/CharlesRngrd/data-evidence/refs/heads/master/assets/french-flag.png"
    description="Drapeau Français"
/>

## Préambule

<div style="text-align: justify">
    <br>
    Que se passe-t-il en France ? Cette question est sur toutes les lèvres par les temps qui courent.
    Depuis la réélection d'Emmanuel Macron en 2022, l'histoire de la Ve république nous plonge dans un récit inédit.
    <br>
    <br>
    Qui dit inédit, dit aussi records.
    C'est là que le second mandat de notre président nous propose un spectacle époustouflant.
    <br>
    <br>
    Certains records nous entrainent dans l'instabilité comme les <b>5 premiers ministres</b> dans un seul mandat.
    D'autres records en découlent comme les <b>130 ministres nommés</b> depuis 2022.
    D'autres encore, ont été battus à plusieurs reprises. C'est le cas du premier ministre le plus éphémère.
    Michel Barnier semblait indétrônables avec ses maigres <b>91 jours</b>.
    Finalement dans un élan matinal, Sébastien Lecornu a su repousser cette limite avec seulement <b>28 jours</b> en poste.
    <br>
    <br>
    A défaut d'avoir pu retenter sa chance, Michel Barnier peut malgré tout se conforter avec un autre record.
    Celui du premier ministre le plus vieux avec ses <b>73 ans</b> lors de sa nomination.
    Ne pouvant rivaliser sur ce terrain, Gabriel Attal détient le record inverse.
    Celui du premier ministre le plus jeune avec seulement <b>34 ans</b>.
    <br>
    <br>
    Finalement, il y a un record qui nous inquiète tous.
    Il s'agit des <b>3400 milliards</b> de dette qui ne cesse de monter comme un courtisan à l'escalier du pouvoir.
    <br>
    <br>
    Bref, quand je regarde ces chiffres, je me dis que la politique est une affaire de data.
    <br>
    <br>
</div>

## Composition du parlement

<div style="text-align: justify">
    <br>
    Pour démarrarer, petit rappel sur la composition de l'assemblée nationale.<br>
    Comme vous pouvez le vérifier le camps présendentiel a perdu des députés suite à la dissolution de 2024.
    <br>
    <br>
    Cela oblige le gouvernement à faire des compromis pour avancer.
    <br>
    <br>
</div>

<ButtonGroup name=selected_annee>
    <ButtonGroupItem valueLabel="Législatives 2024" value=2024 default />
    <ButtonGroupItem valueLabel="Législatives 2022" value=2022 />
</ButtonGroup>

<ECharts
    config={{
        tooltip: {
            trigger: 'item',
            formatter: '{b}: {c} députés ({d}%)'
        },
        series: [
            {
                type: 'pie',
                radius: ['120%', '180%'],
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
                    borderWidth: 2,
                    color: function(params) {
                        const colors = {
                            "Rassemblement National": "#030553",
                            "Les Républicains": "#2D47D9",
                            "Ensemble": "#FCCC34",
                            "Nouveau Front Populaire": "#FC0103",
                        };
                        return colors[params.name] || "#95a5a6";
                    }
                }
            }
        ]
    }}
/>

<Details title="D'où vient la data ?">
    Le data vient de <u><a href="https://www.data.gouv.fr/datasets/elections-legislatives-des-30-juin-et-7-juillet-2024-resultats-definitifs-du-2nd-tour">data.gouv.fr</a></u>
    <br>
    <br>
    Points de vigilance :
    <br>- Pour des raisons de clareté, les partis ont été regroupés en 4 grandes forces politiques.
    <br>- Les données du second tour ne contiennent pas les résultats des circonsriptions remportées dès le permier tour.
</Details>

## Répartition géographique

<Grid cols=2>
    {#each [
        [liste_candidat_nfp, "#FC0103"],
        [liste_candidat_ens, "#FCCC34"],
        [liste_candidat_lr, "#2D47D9"],
        [liste_candidat_rn, "#030553"]
    ] as parti}
        <AreaMap
            data={parti[0]}
            areaCol=circonscription_code
            geoJsonUrl='https://raw.githubusercontent.com/CharlesRngrd/data_demo/refs/heads/main/assets/map.geojson'
            geoId=codeCirconscription
            value=candidat_parti
            height=400
            borderWidth=0.5
            borderColor=#fff
            opacity=1
            selectedColor=warning
            colorPalette={[parti[1]]}
            basemap={"https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}"}
            tooltip={[
                {id: 'departement_nom', title: 'Département', showColumnName: true},
                {id: 'circonscription_nom', title: 'Circonscription', showColumnName: true},
                {id: 'candidat_denomination', title: 'Candidat', showColumnName: true},
                {id: 'candidat_voix_pourcentage', fmt: '#,##0 "%"', title: 'Voix', showColumnName: true},
            ]}
        />
    {/each}
</Grid>

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
    tooltip={[
        {id: 'departement_nom', title: 'Département', showColumnName: true},
        {id: 'circonscription_nom', title: 'Circonscription', showColumnName: true},
        {id: 'candidat_denomination', title: 'Candidat', showColumnName: true},
        {id: 'candidat_voix_pourcentage', fmt: '#,##0 "%"', title: 'Voix du candidat RN', showColumnName: true},
        {id: 'victoire_voix_poucentage', fmt: '#,##0 "%"', title: 'Voix du candidat élu', showColumnName: true}
    ]}
/>

```sql liste_candidat_rang_1
    select *
    from data_legislative.data_legislative
    where candidat_rang = 1
    and annee = ${inputs.selected_annee}
```

```sql liste_candidat_rn
    select *
    from ${liste_candidat_rang_1}
    where candidat_parti = 'Rassemblement National'
```

```sql liste_candidat_lr
    select *
    from ${liste_candidat_rang_1}
    where candidat_parti = 'Les Républicains'
```

```sql liste_candidat_ens
    select *
    from ${liste_candidat_rang_1}
    where candidat_parti = 'Ensemble'
```

```sql liste_candidat_nfp
    select *
    from ${liste_candidat_rang_1}
    where candidat_parti = 'Nouveau Front Populaire'
```

```sql classement_candidat_parti
    select
        candidat_parti, count() as siege_nombre
    from data_legislative.data_legislative
    where candidat_rang = 1
    and annee = ${inputs.selected_annee}
    group by candidat_parti
    order by
    CASE
        WHEN candidat_parti = 'Nouveau Front Populaire' THEN 1
        WHEN candidat_parti = 'Ensemble' THEN 2
        WHEN candidat_parti = 'Les Républicains' THEN 3
        WHEN candidat_parti = 'Rassemblement National' THEN 4
        ELSE 10
    END ASC
```

```sql simulation_candidat_rn
    select
        *,
        if(
            candidat_rang = 1, 'Victoire 2024', 'Victoire potentielle'
        ) as victoire_category,
        candidat_voix_pourcentage - candidat_rang_1_ecart as victoire_voix_poucentage
    from data_legislative.data_legislative
    where candidat_parti = 'Rassemblement National'
    and annee = 2024
    and (
        candidat_rang = 1
        or candidat_rang_1_ecart >= ${inputs.gain_point_vote} * -1.5
        or candidat_voix_pourcentage + ${inputs.gain_point_vote} >= 50
    )
```

```sql simulation_candidat_rn_total
    select
        count() as total
    from ${simulation_candidat_rn}
```
