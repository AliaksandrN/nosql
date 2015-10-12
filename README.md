# Technologie NoSQL

Terminarz rozliczania się z [zadań](http://wbzyl.inf.ug.edu.pl/nosql/zadania):

| zadanie |                      | stacjonarne | niestacjonarne |
|---------|--------------------- |-------------|----------------|
| 1       | EDA                  | 19.11.2015  | 27.11.2015     |
| 2.      | Aggregation Pipeline | 17.12.2015  |  8.01.2016     |
| 3.      | MapReduce            | dodatkowe   | dodatkowe      |
| 4.      | Neo4j                | dodatkowe   | dodatkowe      |


## Podręczne linki

[Dokumentacja MongoDB](https://docs.mongodb.org/manual/):

* [Query and Projection Operators](https://docs.mongodb.org/manual/reference/operator/query/)
* [Update Operators](https://docs.mongodb.org/manual/reference/operator/update/)


## Zaliczenie i Egzamin

> A good demo is disproportionately valuable in big data science.<br>
> — [Storytelling](http://en.wikipedia.org/wiki/Storytelling)

Link do **prywatnego** repozytorium z rozwiązaniami zadań należy wpisać odpowiednio
w plikach [Zaliczenie.md](Zaliczenie.md) i [Egzamin.md](Egzamin.md)
w jednym wierszu według schematu:

    1. [Nazwisko, Imię](link do prywatnego repo z rozwiązaniami zadań)


## Simple Rules for Reproducible Computations

Provide public access to scripts, runs, and results:

1. Version control all custom scripts:
  - avoid writing code
  - **write thin scripts** and use standard tools and use standard UNIX
    commands to chain things together.
1. Avoid manual data manipulation steps:
  - use a build system, for example [**make**](http://bost.ocks.org/mike/make/),
    and have all results produced automatically by build targets
  - if it’s not automated, it’s not part of the project,
    i.e. have an idea for a graph or an analysis?
    automate its generation
1. Use a markup, for example
   [**Markdown**](http://daringfireball.net/projects/markdown/syntax), or
   [**AsciiDoc**](http://asciidoctor.org)
   to create reports for analysis and presentation output products.

Plus two more rules:

1. Record all intermediate results, when possible in standardized formats.
1. Connect textual statements to underlying results.


## Bardzo duże pliki z danymi

Spakowany plik _RC_2015-01.bz2_ zajmuje na dysku 5_452_413_560 B,
czyli ok. 5.5 GB. Każda linijka pliku to jeden obiekt JSON, komentarz
z serwisu Reddit, z tekstem komentarza, autorem, itd.
Wszystkich komentarzy/JSON-ów powinno być 53_851_542.

```bash
bunzip2 --stdout RC_2015-01.bz2 | head -1 | jq .
time bunzip2 --stdout RC_2015-01.bz2 | rl --count 1000 > RC_2015-01_1000.json
# real	 ∞ s
# user	 ∞ s
# sys	0m12 s
time bunzip2 -c RC_2015-01.bz2 | mongoimport --drop --host 127.0.0.1 -d test -c reddit
# 2015-10-09T19:49:35.698+0200	test.reddit	29.5 GB
# 2015-10-09T19:49:35.698+0200	imported 53851542 documents

# real	50m31.021s
# user	82m21.155s
# sys	2m58.977s
```

Plik _primer-dataset.json_ informacje o restauracjach w Nowym Jorku.

```bash
wget https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/dataset.json
cat dataset.json | gzip --stdout > primer-dataset.json.gz
rm dataset.json

gunzip -c primer-dataset.json.gz | shuf -n 1
gunzip -c primer-dataset.json.gz | rl   -c 1
```
