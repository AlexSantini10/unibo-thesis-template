# Template tesi UNIBO

Template LaTeX pronto per scrivere una tesi dell'Universita di Bologna.

Non serve conoscere GitHub Actions o i comandi LaTeX: dopo la configurazione
iniziale, Visual Studio Code compila automaticamente la tesi ogni volta che
salvi un file.

## Cosa offre

- frontespizio UNIBO personalizzabile;
- abstract, tre capitoli di esempio, conclusioni e bibliografia;
- cartelle separate per testi, immagini e configurazione;
- compilazione automatica al salvataggio;
- PDF generato in `build/main.pdf`;
- rimozione automatica dei file temporanei dopo una compilazione riuscita;
- pubblicazione automatica del PDF su GitHub dopo ogni push su `main`.

## Installazione iniziale

Servono:

1. [Visual Studio Code](https://code.visualstudio.com/);
2. l'estensione
   [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop);
3. una distribuzione LaTeX contenente `latexmk` e `biber`.

### Windows

1. Scarica e installa [MiKTeX](https://miktex.org/download).
2. Durante l'installazione, scegli di installare automaticamente i pacchetti
   mancanti.
3. Riavvia Visual Studio Code.

In alternativa, puoi installare
[TeX Live per Windows](https://tug.org/texlive/acquire-netinstall.html).

### Ubuntu e Debian

Apri il terminale ed esegui:

```bash
sudo apt update
sudo apt install latexmk texlive-latex-extra texlive-lang-italian biber
```

Per altre distribuzioni Linux, usa il relativo package manager oppure segui
l'[installazione ufficiale di TeX Live](https://tug.org/texlive/quickinstall.html).

### macOS

Scarica e installa [MacTeX](https://tug.org/mactex/mactex-download.html).

In alternativa, con Homebrew:

```bash
brew install --cask mactex
```

Al termine, riavvia Visual Studio Code.

## Primo avvio

### Ottenere il progetto

Puoi scegliere uno di questi metodi:

- **Nuova tesi su GitHub:** premi **Use this template** nella pagina del
  repository, quindi **Create a new repository**.
- **Senza usare Git:** premi **Code > Download ZIP** nella pagina GitHub ed
  estrai il file ZIP.
- **Con GitHub Desktop:** installa
  [GitHub Desktop](https://desktop.github.com/) e clona il repository.

### Aprire e compilare

1. Ottieni il progetto usando uno dei metodi precedenti.
2. Apri Visual Studio Code.
3. Seleziona **File > Open Folder** e apri l'intera cartella del progetto.
4. Quando VS Code propone l'estensione LaTeX Workshop, premi **Install**.
5. Apri `main.tex` e salvalo con `Ctrl+S` su Windows/Linux oppure `Cmd+S` su
   macOS.
6. Attendi la fine della compilazione.

Il PDF si trova qui:

```text
build/main.pdf
```

Per visualizzarlo dentro VS Code, apri la sezione LaTeX nella barra laterale e
seleziona **View LaTeX PDF**.

## Scrivere la tesi

### Modificare i dati del frontespizio

Apri `config/metadata.tex` e sostituisci i valori di esempio:

```latex
\newcommand{\ThesisTitle}{Titolo della tesi}
\newcommand{\ThesisAuthor}{Nome Cognome}
\newcommand{\ThesisSupervisor}{Chiar.mo Prof.\ Nome Cognome}
\newcommand{\DegreeCourse}{Corso di Laurea in Nome del corso}
```

Il frontespizio usa automaticamente questi dati.

### Modificare abstract, capitoli e conclusioni

I testi della tesi sono organizzati in:

```text
content/
|-- frontmatter/
|   |-- title-page.tex
|   `-- abstract.tex
|-- chapters/
|   |-- 01-introduction.tex
|   |-- 02-development.tex
|   `-- 03-results.tex
`-- backmatter/
    `-- conclusions.tex
```

Puoi rinominare i file dei capitoli. Dopo averli rinominati, aggiorna i
rispettivi percorsi dentro `main.tex`.

Per aggiungere un nuovo capitolo:

1. crea, per esempio, `content/chapters/04-new-chapter.tex`;
2. inizia il file con `\chapter{Titolo del capitolo}`;
3. aggiungi in `main.tex`:

```latex
\input{content/chapters/04-new-chapter}
```

### Aggiungere un'immagine

1. Copia l'immagine dentro `assets/images/`.
2. Inserisci questo codice nel capitolo desiderato:

```latex
\begin{figure}[ht]
  \centering
  \includegraphics[width=0.8\textwidth]{nome-immagine.png}
  \caption{Descrizione dell'immagine}
  \label{fig:nome-immagine}
\end{figure}
```

### Aggiungere una fonte bibliografica

Apri `assets/bibliography/references.bib` e aggiungi una voce:

```bibtex
@book{lamport1994latex,
  author    = {Leslie Lamport},
  title     = {LaTeX: A Document Preparation System},
  publisher = {Addison-Wesley},
  year      = {1994}
}
```

Usa l'identificativo della voce per citarla nel testo:

```latex
\cite{lamport1994latex}
```

La bibliografia viene generata automaticamente alla fine della tesi.

## Compilazione automatica

La configurazione inclusa in `.vscode/settings.json` usa LaTeX Workshop e
`latexmk`.

Ogni volta che salvi un file `.tex`:

1. viene ricompilata l'intera tesi;
2. viene aggiornato `build/main.pdf`;
3. dopo una compilazione riuscita, vengono eliminati i file temporanei come
   `.aux`, `.bbl`, `.bcf` e `.log`.

Se la compilazione fallisce, i file temporanei vengono conservati per aiutare a
individuare l'errore.

## Risolvere problemi comuni

### Il PDF non viene generato

Apri un terminale e verifica che `latexmk` sia installato:

```bash
latexmk --version
```

Se il comando non esiste, reinstalla la distribuzione LaTeX seguendo la sezione
relativa al tuo sistema operativo.

### VS Code segnala un tool non definito

1. Apri la Command Palette con `Ctrl+Shift+P` o `Cmd+Shift+P`.
2. Esegui **Developer: Reload Window**.
3. Salva nuovamente `main.tex`.

### La compilazione segnala un errore

Apri la sezione LaTeX nella barra laterale di VS Code e consulta i messaggi
mostrati da LaTeX Workshop. Generalmente viene indicato il file e la riga che
contengono l'errore.

### Eliminare manualmente i file temporanei

Apri la Command Palette ed esegui:

```text
LaTeX Workshop: Clean up auxiliary files
```

Il comando conserva il PDF.

## Comandi da terminale

Questa sezione e' facoltativa. Non serve per il normale utilizzo con VS Code.

Compilare:

```bash
latexmk -pdf main.tex
```

Eliminare i file temporanei mantenendo il PDF:

```bash
latexmk -c -bibtex-cond1 main.tex
```

Eliminare anche il PDF:

```bash
latexmk -C main.tex
```

## Pubblicazione automatica su GitHub

Il file `.github/workflows/release-pdf.yml` configura GitHub Actions.

A ogni push sul branch `main`, GitHub:

1. installa LaTeX;
2. compila la tesi;
3. salva il PDF come artifact della workflow;
4. crea o aggiorna la release `latest`;
5. allega `main.pdf` alla release;
6. elimina i file generati dal runner dopo la pubblicazione riuscita.

Per scaricare il PDF pubblicato:

1. apri la pagina GitHub del repository;
2. apri la sezione **Releases**;
3. seleziona **Latest thesis PDF**;
4. scarica `main.pdf`.

La workflow puo anche essere avviata manualmente dalla sezione **Actions** del
repository.

### Pubblicare senza usare il terminale

Con GitHub Desktop:

1. apri il repository;
2. scrivi una breve descrizione nel campo **Summary**;
3. premi **Commit to main**;
4. premi **Push origin**.

Terminato il push, GitHub Actions compilera e pubblichera automaticamente il
nuovo PDF. Puoi controllare lo stato dalla sezione **Actions** del repository.

## Struttura del progetto

```text
.
|-- .github/workflows/       # compilazione e pubblicazione su GitHub
|-- .vscode/                 # compilazione automatica al salvataggio
|-- assets/
|   |-- bibliography/        # fonti bibliografiche
|   `-- images/              # immagini e logo
|-- config/
|   |-- metadata.tex         # dati del frontespizio
|   |-- packages.tex         # pacchetti LaTeX
|   `-- style.tex            # stile e impaginazione
|-- content/
|   |-- frontmatter/         # frontespizio e abstract
|   |-- chapters/            # capitoli
|   `-- backmatter/          # conclusioni
|-- build/                   # PDF generato, ignorato da Git
|-- .latexmkrc               # configurazione di latexmk
`-- main.tex                 # ordine delle pagine della tesi
```

Per iniziare, nella maggior parte dei casi basta modificare
`config/metadata.tex` e i file dentro `content/`.
