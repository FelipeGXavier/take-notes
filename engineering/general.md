### Base64

O que base64 não é: uma forma de criptografia para proteger seus dados. 

Base64 foi criado para fornecer uma forma confiável de transportar código binário de um para outro sem a perda de informação. 

Uma forma de transformar imagens e outras estruturas em texto.

Ex.: 

Suponha que você queira adicionar algumas imagens a um documento XML. As imagens tem um formato binário enquanto XML é um texto. XML não tem suporte a integração de formatos binários. 

Solução: usar base64

<hr>

### Linker

O ato de "linkar" linking é o método onde um grupo de objetos são conectados junto em um único executável. A diferença entre o static linking e dynamic link é a etapa que ele ocorre.

## Static linking

Bibliotecas estáticas são binários carregados no tempo de compilação do código. Ou seja, quando o código está compiladora ele já tem 100% do que precisa pra rodar sem precisar carregar uma biblioteca em tempo de execução. Bibliotecas estáticas geralmente tem o formato .a (UNIX based) e .lib (Windows). A dificuldade em bibliotecas estáticas está em sua atualização visto que o código final já está compilado com ela então para atualizar a biblioteca obrigatoriamente temos que recompilar uma nova versão.


## Dynamic linking

Bibliotecas dinâmicas são binários carregados em runtime em uma linguagem. São arquivos .so (UNIX based) e .dll (Windows) que podem ser carregadas no código e executadas. A vantagem nelas está em poder atualizar uma biblioteca sem ter que recompilar o código fonte que a utiliza. Outra vantagem é que o código fonte alvo geralmente é mais leve visto que não tem as bibliotecas embutidas. Uma desvantagem é que para o código funcionar a máquina tem que possuir a biblioteca instalada.

## .dll

DLL (Dynamic Link Library)

## .so 

## .a

## .lib

## O que são arquivos ELF

### CMake

### Distribuindo bibliotecas em linguagens compiladas

Em C++ e C para distribuir executáveis e ou bibliotecas segue a seguinte forma: é disponibilizado o código fonte e a distribuição alvo faz o build pelo CMake ou outro ferramental e é utilizado o binário. Isso ocorre porque não é trivial fazer o build de um código em C++ em uma distro Linux e garantir que funcione em outra. 

Um passo a mais ainda seria disponibilizar bindings em C possibilitando o código em C++ por exemplo ser executado em C e facilitando integração com outras linguagens que geralmente fornecem um FFI com C e não C++.

### Bibliotecas em Linux

Maioria das bibliotecas em Linux são fornecidas como uma tarball .tar.gz contendo o binário. Feita a instalação geralmente cai em algum diretório como /lib e /usr/local/lib

Para utilizar uma biblioteca dinâmica tenha certeza que ela está no diretório que LD_LIBRARY_PATH aponta.

### Como código é compilado para uma arquitetura especifica de processador

## amd64

## x86

### Processadores de 8 bits

### Como uma imagem .jpg e .png é armazenada

As chamadas "raw images" são armazenadas sem qualquer tipo de compressão ou cabeçalho são representados por uma matriz com os bytes de cada canal variando 0 - 255. Quando abrimos esse tipo de imagem em um editor de texto conseguimos visualizar seu conteúdo pois pode ser interpretado diretamente como texto e decodificado para um caracter. 

Agora outros formatos de imagem mais complexos como jpg e png não armazenam diretamente o valor dos canais RGB de 0 255. São adicionadas outras informaçõs e metadados que tendem a não traduzir para um caracter legível dentro de um enconding como o ASCII por exemplo. 

Um guia como implementar um leitor de imagens png: 

https://handmade.network/forums/articles/t/2363-implementing_a_basic_png_reader_the_handmade_way

## Blob 

É um tipo especial de dado em banco de dados destinado ao armazenamento de dados binários de qualquer tamanho. 

Se fosse armazenar um audio por exemplo, seria lido o arquivo em stream e transformado em um array de bytes que é inserido no banco como um blob.

### Como FFI funciona

### Como LLVM funciona


### O que são headers pré-compilados


### Como stdin, stdout e stderr funciona


### Como POSIX e System Calls funcionam

### Quais são os comandos de kill e o que representam (SIGKILL etc)