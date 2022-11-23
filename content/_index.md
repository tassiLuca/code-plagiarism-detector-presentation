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

# Contesto e motivazioni

Questa tesi nasce dalla necessità di sviluppare uno strumento capace d'individuare potenziali *plagi* in progetti software.

Il plagiarismo nel software è pratica che, nel tempo, ha visto numerosi scontri legali (ad esempio, Oracle contro Google per Android), e per il quale sono pochi i progetti open source di facile utilizzo pratico.

L'elaborato, quindi, si addentra nelle tecniche di analisi e d'individuazione di possibili plagi, presentando il processo di progettazione e sviluppo dello strumento.

---

# Perché un sistema antiplagio **automatico**?

{{% fragment %}}
- La *complessità* dei progetti software *aumenta*;
{{% /fragment %}}

{{% fragment %}}
- La *quantità* di progetti software a disposizione cresce esponenzialmente;
{{% /fragment %}}

{{% fragment %}}
- Farlo manualmente è tedioso, impiega molto tempo e risorse!
{{% /fragment %}}

{{% fragment %}}
In generale, creare un software antiplagio è **_complesso_**. 

Sono due le principali sfide da affrontare durante la progettazione.
{{% /fragment %}}

---

## Prima sfida: creare un tool più "furbo" dello sviluppatore

Quando la copiatura è un atto deliberato, lo sviluppatore adotta un insieme di tecniche per cercare di _offuscarle_.

Le principali rifattorizzazioni adottate possono essere classificate in _lessicali_ e _strutturali_.

---

{{< slide transition="zoom" transition-speed="fast" >}}
- _lessicali:_
  - possono essere eseguite, in linea di principio, da un text editor
  - non richiedono la conoscenza del linguaggio di programmazione
  - Alcuni esempi:
    {{% smaller %}}
    - riformulazione di commenti, la loro aggiunta o rimozione;
    - riformattazione del testo, l'introduzione di spazi vuoti, di nuove linee o il cambio dell'ordine dei parametri nella definizione delle funzioni;
    -  cambiare il nome degli identificatori e delle funzioni o i tipi di dato: ad esempio da `int` a `Integer` o da `float` a `double`.
---

{{< slide transition="zoom" transition-speed="fast" >}}
- _strutturali:_
  - fortemente dipendenti dal linguaggio di programmazione $\Rightarrow$ maggior sforzo in termini di comprensione della logica del codice;
  - Alcuni esempi:
    <div class="smaller">
    {{< markdown >}}
    - aggiungere istruzioni ridondanti, come dichiarazioni, inizializzazioni, istruzioni di stampa;
    - sostituire i costrutti di loop con costrutti equivalenti: passare da `for` a `do/while`, o da un approccio iterativo a uno funzionale, ad esempio tramite l’uso degli Stream in Java;
    - sostituire istruzioni `if` nidificate con dichiarazioni equivalenti, ad esempio `switch-case` o `when` in Kotlin, e viceversa;
    - cambiare l’ordine d’istruzioni indipendenti;
    - cambiare l’ordine degli operandi: ad esempio `x < y` può essere cambiato in `y >= x`;
    - sostituire la chiamata a funzione con il corpo della stessa
    {{< /markdown >}}
    </div>

---

{{< figure src="01-levels-of-plagiarism.pdf" height="100%" width="400" >}}

{{% smaller %}}
**Tassonomia dei livelli di plagio di Faidhi & Robinson (1987)**: mappa le possibili rifattorizzazioni in sette livelli o categorie, sulla base della loro difficoltà: il più semplice è il livello zero che corrisponde a una copia letterale; il più impegnativo è il sesto livello che corrisponde a un cambiamento logico e può essere considerato plagio solo se si verifica in concomitanza con altri livelli.

---

## Seconda sfida: le prestazioni

Un sistema antiplagio che non si limita a confrontare la similarità tra una coppia di progetti, bensì effettui un controllo _uno a molti_ o _molti a molti_, in cui si testano _tutte le possibili coppie_, sono le prestazioni.

- la misurazione della somiglianza tra una coppia di sorgenti, nella gran parte degli algoritmi noti in letteratura, ha una complessità almeno quadratica nel numero delle istanze delle sue rappresentazioni
- per ogni valutazione, il numero di confronti da effettuare è tipicamente elevato
  <div class="smaller">
  {{< markdown >}}
  - detto $N$ il numero di progetti, volendo confrontare tutte le coppie di progetti tra loro, dovrebbero essere eseguite $N(N−1)$ comparazioni.
  </div>

---

Nel corso degli anni, è stato necessario porsi il problema di sviluppare strumenti di rilevamento automatici di plagi che fossero _efficienti_ ed _efficaci_.

Tre popolari famiglie di tecniche:

{{% fragment %}}
- **attribute-based**
{{% /fragment %}}

{{% fragment %}}
- **structure-based**
{{% /fragment %}}

{{% fragment %}}
- **tecniche ibride**
{{% /fragment %}}

---

---

```mermaid
classDiagram
    Animal <|-- Duck
    Animal <|-- Fish
    Animal <|-- Zebra
    Animal : +int age
    Animal : +String gender
    Animal: +isMammal()
    Animal: +mate()
    class Duck{
        +String beakColor
        +swim()
        +quack()
    }
    class Fish{
        -int sizeInFeet
        -canEat()
    }
    class Zebra{
        +bool is_wild
        +run()
    }
```
