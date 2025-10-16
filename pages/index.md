---
title: Que se passe-t-il en France ?
sidebar: never
hide_toc: true
hide_beadcrumbs: true
---

<Note>
    Publié par Charles RANGHEARD le 15 octobre 2025.
</Note>

<Image
    url="https://raw.githubusercontent.com/CharlesRngrd/data-evidence/refs/heads/master/assets/french-flag.png"
    description="Drapeau Français"
/>

<div style="text-align: justify">
    <br>
    Que se passe-t-il en France ? Cette question est sur toutes les lèvres par les temps qui courent.
    Depuis la réélection d'Emmanuel Macron en 2022, l'histoire de la Ve république nous plonge dans un récit inédit.
    <br>
    <br>
    Qui dit inédit, dit aussi records.
    C'est là que le second mandat de notre président nous offre son spectacle.
    <br>
    <br>
    Certains records nous entrainent dans l'instabilité comme les <b>5 premiers ministres</b> dans un seul mandat.
    D'autres records en découlent comme les <b>130 ministres nommés</b> depuis 2022.
    D'autres encore, ont été battus à plusieurs reprises. C'est le cas du premier ministre le plus éphémère.
    Michel Barnier semblait indétrônable avec ses maigres <b>91 jours</b>.
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
    C'est ce que nous allons explorer au travers des résultats des élections législatives 2022 et 2024.
    <br>
    <br>
</div>

## Composition du parlement en <Value data={simulation_candidat_total} column=annee fmt="###0" />

<div style="text-align: justify">
    <br>
    Pour démarrarer, petit rappel sur la composition de l'assemblée nationale.<br>
    Comme vous le savez, le camp présendentiel a perdu des députés suite à la dissolution de 2024.
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
    Elles ont été rajoutées en utilisant le jeu de données du premier tour.
</Details>

## Répartition géographique en <Value data={simulation_candidat_total} column=annee fmt="###0" />

<div style="text-align: justify">
    <br>
    Lorsque l'on regarde par famille politique la carte de France des résultats, certaines régularités s'observent :
    <br>- Quelque soit le parti, la majorité des circonscriptions gagnées touchent une autre circonscription gagnée par ce même parti.
    Cela forme des tâches qui n'ont rien à voir avec les départements.
    <br>- Chaque parti est fortement représenté dans une zone distincte de la France : le Nouveau Front Populaire est très présent dans le sud-ouest, Ensemble dans le nord-ouest, Les Républicains dans l'Est et le Rassemblement National au nord-est et au sud-est. A croire que les citoyens qui habitent sur la partie droite de la France votent à droite.
    <br>
    <br>
    Attention, au premier abord, on a l'impression qu'il y a plus de députés Rassemblement National que Nouveau Front Populaire. En réalité le Nouveau Front Populaire est très présent dans les grandes villes. Vous pouvez le vérifier avec un zoom sur Paris.
    <br>
    <br>
</div>

<Details title="Où sont les DOM-TOM ?">
    Oui, désolé, les DOM-TOM ne s'affichent pas sur la carte. C'est en cours de correction.
</Details>

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
            value="Parti politique"
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

## Simulation d'une augmentation des voix en <Value data={simulation_candidat_total} column=annee fmt="###0" />

<div style="text-align: justify">
    <br>
    Une question reste en suspens :
    <br>Que se passerait-il en cas de nouvelle disolution ?
    Est-ce que le Nouveau Front Populaire ou le Rassemblement National pourrait obtenir une majorité ?
    <br>Vous pouvez simuler le nombre de députés qu'ils pourraient obtenir si leur score grimpait lors des prochaines législatives.
    <br>
    <br>
</div>

Si le **{inputs.selected_candidat_parti}** augmente son score de **{inputs.gain_point_vote} point{inputs.gain_point_vote == 1 ? "" : "s"}**,
le parti aura **<Value data={simulation_candidat_total} column=total /> députés**.
Soit **+<Value data={simulation_candidat_total} column=ecart /> députés** par rapport à <Value data={simulation_candidat_total} column=annee fmt="###0." />

<Details title="Comment sont fait les calculs ?">
    En cas de duel, 1 point permet de gommer un écart de 2 points car l'adversaire perdrait 1 point également.
    <br>En cas de triangulaire, 1 point permet de gommer un écart de 1,5 points car les 2 adversaires perdraient 0,5 point.
</Details>

<Grid cols=2>
    <ButtonGroup name=selected_candidat_parti>
        <ButtonGroupItem valueLabel="Nouveau Front Populaire" value="Nouveau Front Populaire" default />
        <ButtonGroupItem valueLabel="Rassemblement National" value="Rassemblement National" />
    </ButtonGroup>

    <Slider
        title="Gain de points"
        name=gain_point_vote
        min=1
        max=10
    />
</Grid>

<AreaMap
    data={simulation_candidat}
    areaCol=circonscription_code
    geoJsonUrl='https://raw.githubusercontent.com/CharlesRngrd/data_demo/refs/heads/main/assets/map.geojson'
    geoId=codeCirconscription
    value="Catégorie d'élus"
    height=700
    borderWidth=0.5
    borderColor=#fff
    opacity=1
    selectedColor=warning
    colorPalette={
        inputs.selected_candidat_parti=="Nouveau Front Populaire" ? ["#FC0103", "#FE8A8A"] : ["#030553", "#BABBFD"]
    }
    basemap={"https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}"}
    tooltip={[
        {id: 'departement_nom', title: 'Département', showColumnName: true},
        {id: 'circonscription_nom', title: 'Circonscription', showColumnName: true},
        {id: 'candidat_denomination', title: 'Candidat', showColumnName: true},
        {id: 'candidat_voix_pourcentage', fmt: '#,##0 "%"', title: 'Voix du candidat RN', showColumnName: true},
        {id: 'candidat_rang_1_score', fmt: '#,##0 "%"', title: 'Voix du candidat élu', showColumnName: true}
    ]}
/>

```sql liste_candidat_rang_1
    select *, candidat_parti AS "Parti politique"
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

```sql simulation_candidat
    select
        *,
        if(
            candidat_rang = 1, 'Elu 2024', 'Elu potentiel'
        ) as "Catégorie d'élus"
    from data_legislative.data_legislative
    where candidat_parti = '${inputs.selected_candidat_parti}'
    and annee = ${inputs.selected_annee}
    and (
        candidat_rang = 1
        or candidat_rang_1_ecart >= ${inputs.gain_point_vote} * -1.5
        or candidat_voix_pourcentage + ${inputs.gain_point_vote} >= 50
    )
```

```sql simulation_candidat_total
    select
        max(annee) as annee, count() as total, count_if(candidat_rang != 1) as ecart
    from ${simulation_candidat}
```
