### Anotações 

size_t é utilizado para representar índices/valores em memória como o tamanho de um objeto ou array.

<pre>
int arr[] = {1,2,3};
size_t v = sizeof(arr);
</pre>

v assume o valor de 12 (bytes) porque cada valor inteiro tem 4 bytes (32 bits) por padrão. 

size_t funcionado como se fosse um "alias" (não exatamente) de sizeof(unsigned int)

<hr>

Você deve evitar o uso de new/delete que utiliza dynamic memory allocation. 

new/delete seria como utilizar malloc/free em C de certa forma. 

C++ tem um jeito mais eficiente instanciando diretamente objetos e utilizando o RAII. 

<hr>

Em C++ chamamos de *statement* o tipo de instrução que causa o programa executar alguma ação, normalmente terminam com ;

Enquanto *expressions* são utilizadas para calcular "evaluate" uma instrução como 2 + 3, call("foo")

<hr>

C++ tem várias formas de instanciar objetos e variáveis etc. Uma forma introduzida é a de {} chamada de brace initialization. 

Por boas práticas na declaração de literais, smart pointers, wrappers (std::vector, std::map), struct initialization e copy construction usamos o "*=*"

<pre>
int x = 10;
std::string foo = "bar";
std::unique_ptr<Matrix> matrix = NewMatrix(rows, cols);
MyStruct x{true, 5.0};
</pre>

Utilizamos a forma tradicional de constructor "*()*" quando a inicialização do objeto contém alguma lógica.

<pre>
Frobber frobber(size, &bazzer_to_duplicate);
</pre>

Em vez de:
<pre>
Frobber frobber{size, &bazzer_to_duplicate};
</pre>

A segunda pode invocar um construtor com dois parâmetros ou intializer list constructor.

<hr>

Undefined behavior é quando um trecho de código não tem um comportamento padrão para o compilador lidar como iniciar uma variável int x; vai ser alocado algum endereço de memória, mas não sabemos qual. Cada execução pode retornar um valor diferente.

<hr>

Aritmética de ponteiros (pointer arithmatic) em C/C++ são as operações fundamentais realizadas em cima de pointeiros onde basicamente você avançar ou regredir pra um endereõ de memória a partir do tamanho e tipo de um ponteiro. 

<pre>

int *ptr; // int 4 bytes (32 bits) e.g. position in memory 2001
int *newAddr = ptr + 1; // sum 4 bytes in memory position so its 2005

int arr[] = {1,2,3,4};
int *firstElementPtr = &arr[0];

int *nextElementPtr = firstElementPtr + 1;
int nextElement = *nextElementPtr;

</pre>

É possível visualizar em alguns trechos de código <pre>(char*)ptr</pre> fazendo o casting pra char* para fazer a artimética de ponteiros a nível de bytes.

<hr>

Header files são utilizados para indicar que uma definição de função, struct, tipo etc existe dentro do código e está definido durante o tempo de compilação. Para evitar dependências cíclicas e definição duplicadas usamos header guards como `#pragma once` ou antigamento `#ifndef <opt> #endif`

<hr>

Código em C++ é compilado sequencialmente então as funções utilizadas devem estar em order de execução para serem encontradas ou utilizar `function forward` que é basicamente declarar o nome e assinatura função sem o corpo. 

<hr>

Você utilizar multiline string usando R"(
    str1
    str2
)

<hr>

Você pode passar funções como parâmetro passando um ponteiro dela ou usando `std::function. `

<pre>

void call(void (*func)(int)) {
    func(0);
}

call(&myFunc);

</pre>

<hr>

Namespaces são uma forma desambiguação de código, após declarado um namespace todo seu conteúdo é específico a esse escopo permitindo por exemplo que uma mesma função seja declarada em namespaces diferentes.

Normalmente o namespace o identificador do namespace fica antes da anotação `<namespace_name>::value` por exemplo `std::string` std é o namespace. 

Para não ter que usar std:: em todo lugar poder utilizar using namespace, mas deve ser evitado. 