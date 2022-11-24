+++
title = "Code Plagiarism Detector Presentation"
description = "TODO"
outputs = ["Reveal"]
+++

<section data-noprocess>
    <h4>Alma Mater Studiorum $\cdot$ Università di Bologna</h4>
    <h6 style="font-size:0.8em;">Campus di Cesena</h6>
    <hr/>
    <p style="font-size:0.8em;">Dipartimento di informatica $-$ Scienza e Ingegneria</p>
    <p style="font-size:0.8em;">Corso di Laurea in Ingegneria e Scienze Informatiche</p>
    <h2 style="margin:1em 0"><a href="https://github.com/tassiLuca/bachelor-thesis/releases/latest">Progettazione e sviluppo di uno strumento per la scansione di progetti software alla ricerca di potenziali segni di plagio</a></h2>
    <p style="font-size:0.8em; margin-bottom:0">Elaborato in</p>
    <h6>PROGRAMMAZIONE A OGGETTI</h6>
    <div style="margin-top:1em;">
        <div style="width:50%; border-box:none; float: left">
            <p style="font-size:0.7em">Relatore</p>
            <p style="font-size:0.8em">Prof. Danilo Pianini</p>
        </div>
        <div style="width:50%; border-box:none; float: right">
            <p style="font-size:0.7em;">Presentata da:</p>
            <p style="font-size:0.8em;">Luca Tassinari</p>
        </div>
    </div>
</section>

---

## Plagiarismo e antiplagiarismo nel software

- problema annoso e in continua crescita
- ha visto, nel corso degli anni, numerosi scontri legali, i.e. Oracle vs. Google per Android 
- farlo manualmente è impraticabile $\Rightarrow$ è necessario un tool _automatico_
- sono pochi i progetti _open source_ di facile utilizzo

**MA**

Creare un software antiplagio è _complesso_!

---

### Sistemi antiplagio automatici e loro caratteristiche

{{< figure src="techniques.svg" height="400" width="90%" >}}

---

## Requisiti di un tool antiplagio moderno

<div class="container">
<div class="col" style="width: 50%;">

- sia in grado di essere insensibile a rifattorizzazioni operate per offuscare la copiature

{{% fragment %}}
- effettui il confronto su più progetti
  - ⚠️ la misura della similarità tra sorgenti è un'operazione onerosa in termini computazionali!
{{% /fragment %}}

</div>

<div class="col" style="width: 50%; font-size: 0.8em; vertical-align: center">

{{< figure src="levels-of-plagiarism.svg" width="95%" >}}

{{% smaller %}}
**Tassonomia dei livelli di plagio di Faidhi & Robinson (1987)**

</div>
</div>

---

# Gli stadi logici di un tool antiplagio

---

<div style="width: 27%; padding: 0 36%;">

```mermaid
flowchart TB
    sources[(Program Sources)] -- strings --> analysis[1. Analysis]
    analysis -- token sequences --> filtering[2. Filtering]
    filtering -- filtered token sequences --> Detection[6.Detection]
    Detection -- matches --> result(((Reports)))
```

</div>

---

### Fase 1: Analisi

```mermaid
flowchart TB
    subgraph analysis
    direction LR
        Parsing[1.Parsing] -- AST --> Cleaning[2.Cleaning]
        Cleaning -- Cleaned AST --> Tokenization[3.Tokenization]
    end
```

---

---

---

---

---

---
