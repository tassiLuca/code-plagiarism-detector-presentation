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

<div class="container">
<div class="col" style="width: 50%; text-align: right">

### Stadi logici di un tool antiplagio

</div>
<div class="col" style="width: 50%; padding: 0 23%">

```mermaid
flowchart TB
    sources[(Program Sources)] -- strings --> analysis[1. Analysis]
    analysis -- representations --> filtering[2. Filtering]
    filtering -- filtered representations --> Detection[6.Detection]
    Detection -- matches --> result(((Reports)))
```

</div>
</div>

---

# Il tool sviluppato
- permette di reperire i progetti da _repository_ _GitHub_ e _Bitbcuket_
- effettua l'analisi dei sorgenti utilizzando una tecnica _structure based_
- genera un report testuale con la stima di similarità repo-to-repo e con le parti di codici individuate come simili

---

### Fase 1: Analisi

**Scopo:** trasformare i sorgenti in rappresentazioni confrontabili che astraggano dai dettagli del linguaggio

```mermaid
flowchart TB
    subgraph analysis
    direction LR
        Parsing[1.Parsing] -- AST --> Preprocess[2.Preprocessing]
        Preprocess -- Cleaned AST --> Tokenization[3.Tokenization]
    end
```

1. I file sono _parsati_ $\rightarrow$ è generato l'_Abstract Syntax Tree_
2. _Preprocessing_: l'AST viene "ripulito" da dettagli insignificanti a livello semantico (commenti, dichiarazioni di `import`, `package`, ...)
3. L'AST viene visitato e vengono "emessi" i _token_ corrispondenti ai costrutti del linguaggio da valorizzare

---

<div class="container">
<div class="col" style="margin-left: -7%">

**Prima dell'analisi**

```java
package org.examples;

import java.util.Arrays;

/**
 * This is a sample class to demonstrate the tokenization process.
 */
public class Main {
    public static void main(String[] args) {
        if (args.length > 0) {
            System.out.println("Program arguments: " + Arrays.toString(args));
        } else {
            System.out.println("Hello world from Java!");
        }
    }
}
```

</div>
<div class="col">

{{% fragment %}}

**Dopo la _tokenizzazione_**

```text
[class-interface-decl (line=8, column=1), 
method-decl (line=9, column=5), 
parameter (line=9, column=29), 
block-stmt (line=9, column=44), 
if-stmt (line=10, column=9), 
binary-expr (line=10, column=13), 
field-access-expr (line=10, column=13), 
name-expr (line=10, column=13), 
literal-expr (line=10, column=27), 
block-stmt (line=10, column=30), 
expression-stmt (line=11, column=13), 
method-call-expr (line=11, column=13), 
field-access-expr (line=11, column=13), 
name-expr (line=11, column=13), 
binary-expr (line=11, column=32), 
literal-expr (line=11, column=32), 
method-call-expr (line=11, column=56), 
name-expr (line=11, column=56), 
name-expr (line=11, column=72), 
block-stmt (line=12, column=16), 
expression-stmt (line=13, column=13),
method-call-expr (line=13, column=13), 
field-access-expr (line=13, column=13), 
name-expr (line=13, column=13), 
literal-expr (line=13, column=32)]
```

{{% /fragment %}}

</div>
</div>

---

### Fase 2: Filtraggio

**Scopo:** limitare il numero di confronti $\Rightarrow$ migliorare le prestazioni

<div style="padding: 0 20%">

```mermaid
flowchart TB
    subgraph filtering[filtering]
        direction LR
        Indexing[4.Indexing] -- Index DS --> Filtering[5.Filtering]
    end
```

</div>

1. Le rappresentazioni (sequenze di _token_) sono aggregate sotto forma di strutture dati dalla quali è possibile estrarre informazioni statistiche sulla base delle quali viene stimata la similarità

{{% fragment %}}

### Fase 3: Detection
Sono applicati algoritmi di _string matching_, riadattati per funzionare su sequenze di token.

{{% /fragment %}}

---

## Configurabilità del tool

- interfaccia a riga di comando che permette la configurazione:
  - lunghezza minima della sequenza di _token_ che dovrebbe essere riportata
  - soglia di similarità tra sorgenti ($ 0. \div 1.$)
  - soglia di filtraggio dei sorgenti ($ 0. \div 1.$)

---

## Esempio di output

---

# Validazione dei risultati

- **submission set**: 130 progetti universitari sviluppati in Java nel triennio 2019-21
- **corpus set**: +354 progetti sottomessi nello stesso corso (2015-) 
  - confronto a "prodotto cartesiano"

---

## Analisi di sensitività

<div class="smaller">

| **Progetto originale** | **Progetto copiato** | **Similarità** | **Ispezione manuale**      |
|------------------------|----------------------|----------------|----------------------------|
| 9b7266                 | 80fd2e               | 100\%          | corrispondenza totale      |
| f0caf3                 | c1e451               | 90\%           | corrispondenza totale      |
| ac8a48                 | 7d79ff               | 81\%           | corrispondenza elevata     |
| 7bc0ee                 | 2308d9               | 70\%           | corrispondenza medio-alta  |
| a5c39e                 | 2ed153               | 62\%           | corrispondenza medio-alta  |
| 005bc2                 | f67c20               | 55\%           | corrispondenza parziale    |
| 501b0f                 | c01302               | 49\%           | corrispondenza parziale    |
| 8f5d5a                 | afcd72               | 48\%           | falso positivo             |

</div>

---

# 🙏
### Grazie per l'attenzione!

---
