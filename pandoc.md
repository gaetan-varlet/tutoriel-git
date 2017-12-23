# Pandoc, convertisseur de document universel

Il faut installer commencer par installer [pandoc](http://pandoc.org/), puis installer un moteur LaTeX, par exemple [MiKTeX](https://miktex.org/), pour générer des PDF.



## Exemples de commandes :
`[...]` : code optionnel

### Markdown vers Formats divers
- convertir un fichier markdown en PDF (avec coloration syntaxique du code) : `pandoc input.md [--toc] -o output.pdf`
- convertir un fichier markdown en odt : `pandoc input.md -o output.odt`
- convertir un fichier markdown en docx : `pandoc input.md -o output.docx`
- convertir un fichier markdown en html :
	- produire un fragment HTML : `pandoc input.md -o output.html`
	- produire une page HTML autonome : `pandoc -s input.md -o output.html`
	- produire une page HTML avec une table des matières, un fichier CSS et un pied de page personnalisé : `pandoc -s --toc -c pandoc.css -A footer.html input.md -o output.html`
- convertir un fichier markdown en présentation beamer avec reveal.js :
`pandoc -t beamer input.md -o beamer.pdf`
- convertir un fichier markdown en présentation HTML/JavaScript avec reveal.js : `pandoc -t revealjs -s input.md -o presentation.html`

### Formats divers vers Markdown
- convertir un fichier ODT en markdown : `pandoc input.odt -o output.md`
- convertir un fichier DOCX en markdown : `pandoc input.docx -o output.md`
