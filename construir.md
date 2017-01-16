---
title: contribuir
layout: default
---
__nota: esta página não é pensada para o público em geral (mas se você quiser aprender algo, fique e divirta-se).__

aqui vou mostrar como foi feita a conversão do arquivo-texto markdown em que há o conteúdo integral de Ursula para os diversos formatos disponíveis.

### .md -> .md

o arquivo original de Ursula está em [markdown](https://daringfireball.net/projects/markdown/syntax). um único arquivo contém todos os capítulos do livro. para dividí-lo em vários arquivos (um para cada capítulo), usei o código python abaixo. se você tiver python no seu computador (instale [aqui](https://www.python.org/)), basta abrir o bloco de notas, copiar e colar o código abaixo, e salvá-lo com a terminação `.py`, e.g. `capitulos.py`. abra o terminal (command prompt no windows: Win + R, cmd, enter), e digite `python capitulos.py` (ou o nome que você tenha dado).

trabalhar com o texto todo em um arquivo é útil para edições do tipo 'search and replace'. por exemplo: decidi deixar o nome 'Ursula' como no original, sem acento. se desejasse mudar 'Ursula' para 'Úrsula', é mais fácil modificar um arquivo só do que vários.

```python
#!/usr/bin/python
import os
'''essa função toma uma arquivo único .md em que está o conteúdo integral de ursula, e depois divide seu conteúdo em 22 capítulos, cada um em um arquivo diferente.'''
def ursula_to_chaps(file_loc):
    with open(file_loc, mode='r+', encoding='utf-8') as input_data:
        livro = input_data.read()
        chaps = livro.split('___')
        nr_chap = 0
        dest_dir = file_loc.rsplit('/', 1)[0] + '/chaps-ursula/'
        if not os.path.isdir(dest_dir):
                os.mkdir(dest_dir)
                
        for chap in chaps:
            base_nome = chap.split('## ', 1)[1].split('\n', 1)[0].replace(' ', '-').lower() #pega o nome do capítulo, sempre depois de '##' e terminando em '\n'
            base_nr = '{:0>2}'.format(nr_chap) #formata numero dos capitulos para poder coloca-los em ordem
            nome_chap = dest_dir + base_nr + '-' + base_nome + '.md'
            with open(nome_chap, mode='w', encoding='utf-8') as output_data:
                output_data.write(chap)
            nr_chap += 1

file_loc = input('onde está o arquivo com ursula?')

ursula_to_chaps(file_loc)
```

se as edições forem feitas em um arquivo só, entretanto, é preciso ter uma forma de unir vários arquivos .md (correspondentes a cada capítulo) em um só. para isso (e outras conversões), usei o [pandoc](http://pandoc.org/).
