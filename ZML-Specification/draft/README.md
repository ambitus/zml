# Work with asciidoc

## Install for editing
Visual studio code has an extension to preview and edit asciidoc. Just search for it:
https://marketplace.visualstudio.com/items?itemName=asciidoctor.asciidoctor-vscode

## Install for PDF creation

1. Install [Ruby](https://www.ruby-lang.org/en/downloads/)
2. Install asciidoctor: `[sudo] gem install asciidoctor asciidoctor-pdf rouge`

## Create PDF

`asciidoctor -r asciidoctor-pdf -b pdf spec/spec.adoc`