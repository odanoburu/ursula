---
title: contribuir
layout: default
---
###### nota: esta página não é pensada para o público em geral (mas se você quiser aprender algo, fique e divirta-se).

aqui vou mostrar como foi feita a conversão do arquivo-texto markdown em que há o conteúdo integral de Ursula para os diversos formatos disponíveis.

## .md -> .md

o arquivo original de Ursula está em [markdown](https://daringfireball.net/projects/markdown/syntax). um único arquivo contém todos os capítulos do livro, separados por `___`. para dividí-lo em vários arquivos (um para cada capítulo), usei o código python abaixo (python3 ou maior). se você tiver python no seu computador (instale [aqui](https://www.python.org/)), basta abrir o bloco de notas, copiar e colar o código abaixo, e salvá-lo com a terminação `.py`, e.g. `capitulos.py`. abra o terminal (command prompt no windows: Win + R, cmd, enter), e digite `python capitulos.py` (ou o nome que você tenha dado).

trabalhar com o texto todo em um arquivo é útil para edições do tipo 'search and replace'. por exemplo: decidi deixar o nome 'Ursula' como no original, sem acento. se desejasse mudar 'Ursula' para 'Úrsula', é mais fácil modificar um arquivo só do que vários.

```python3
#!/usr/bin/python3.5
import os

def ursula_to_chaps(file_loc):
'''
essa função toma uma arquivo único .md em que está o conteúdo integral de ursula, e depois divide seu conteúdo em 22 capítulos, cada um em um arquivo diferente.
'''
    with open(file_loc, mode='r+', encoding='utf-8') as input_data:
        livro = input_data.read()
        livro.replace('\n\n', '\n') # para impedir que a função não cria espaços demais se for chamada muitas vezes.
        chaps = livro.split('___')
        nr_chap = 0
        sep = '\n___\n' # para que possamos juntar os capítulos em um arquivo só depois
        dest_dir = file_loc.rsplit('/', 1)[0] + '/chaps-ursula/'
        if not os.path.isdir(dest_dir):
                os.mkdir(dest_dir)
                
        for chap in chaps:
            if chap != chaps[len(chaps) - 1]:
                chap += sep # para que o último cap. não tenha separador
            else:
                base_nome = chap.split('## ', 1)[1].split('\n', 1)[0].replace(' ', '-').lower() #pega o nome do capítulo, sempre depois de '##' e terminando em '\n'
                base_nr = '{:0>2}'.format(nr_chap) #formata numero dos capitulos para poder coloca-los em ordem
                nome_chap = dest_dir + base_nr + '-' + base_nome + '.md'
                with open(nome_chap, mode='w', encoding='utf-8') as output_data:
                    output_data.write(chap)
            nr_chap += 1

file_loc = input('onde está o arquivo com ursula?')

ursula_to_chaps(file_loc)
```

se as edições forem feitas em um arquivo só, entretanto, é preciso ter uma forma de unir vários arquivos .md (correspondentes a cada capítulo) em um só. para isso, usei o terminal linux (se você usa windows, você precisa descobrir como fazer isso em windows e me contar). basta ter uma pasta com todos os capítulos de Ursula em formato .md (nada mais, nada menos), e rodar

```bash
cat *.md > ursula.md
```
esse comando concatena todos os arquivos .md da pasta em que você estiver e os salva em um novo arquivo `ursula.md` (por isso, não rode o comando se existir algum arquivo .md diferente dos capítulos).

## .md -> .epub

para transformar o arquivo .md em .epub, precisamos usar o programa [pandoc](http://pandoc.org/). há instruções de download e uso [aqui](http://pandoc.org/getting-started.html). é preciso abrir o terminal (ou command prompt) na pasta em que o arquivo .md original de ursula está, e rodar:

```bash
pandoc ursula.md -s -o ursula.epub
```
`pandoc` chama o programa, o comando `-s` demanda como resultado um arquivo _standalone_ e o comando `-o` diz onde deve ser guardado o resultado, nesse caso, `ursula.epub.`

podemos fazer essa conversão a partir dos vários arquivos de capítulos. abra o terminal em uma pasta em que todos os arquivos .md se referem à capítulos de Ursula (eles devem estar em ordem), e rode:

```bash
pandoc *.md -s -o ursula.epub
```
o resultado será o mesmo.

## .epub -> .mobi, .azw

para fazer essa conversão, uso o programa [calibre](https://calibre-ebook.com/). o programa dispõe de uma interface gráfica, então deve ser simples descobrir como usá-lo.

## .md -> .pdf

essa conversão foi feita manualmente. a partir do template disponível [aqui](https://www.overleaf.com/docs?snip_uri=http://www.latextemplates.com/templates/books/4/ebook.zip), colei o texto do .md em que havia o conteúdo integral de Ursula e adaptei o código latex.

se você quiser uma versão pdf mais simples (funcionará bem para computadores, mas não para e-readers), uma opção é usar o pandoc:

```bash
pandoc *.md -s -o ursula.pdf
```

para que o comando rode adequadamente, é preciso instalar o LaTeX, como está escrito na [página do pandoc](http://pandoc.org/getting-started.html).
