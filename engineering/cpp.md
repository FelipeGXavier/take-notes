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

Somar um número como 1 a um ponteiro int de 4 bytes signfica pular 4 bytes e ir para o próximo endereço.

<hr>

Header files são utilizados para indicar que uma definição de função, struct, tipo etc existe dentro do código e está definido durante o tempo de compilação. Para evitar dependências cíclicas e definição duplicadas usamos header guards como `#pragma once` ou antigamente `#ifndef <opt> #endif`

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

<hr>

Fazer casting de um pointeiro para outro vai aumentar ou diminuir seu tamanhho, por exemplo:

<pre>
int a = 1025;
int *p = &a;
char *ch;
ch = (*char)p;
// 1025 -> 00000000 00000000 00000100 00000001 (32 bits/4bytes)
// char = 1 byte, o casting vai pegar o último byte 00000001, ou seja, o valor é igual 1
</pre>

<hr>

Value types e reference types (pointers) são armazenados de forma diferente. Em C/C++ value types quando atribuídos a um valor recebem um endereço de memória de forma estática durante o tempo de compilação então a instrução gerada já sabe diretamente o endereço de memória e lida diretamente com o valor literal sem ter que primeiro carregar o endereço de memória.

Enquanto references (pointers) quando declarados primeiro é gerado uma instrução que carrega o valor no endereço de memória pra depois ser utilizado.

O tamanho de um ponteiro não influencia o tamanho do valor que ele aponta, assim como os blocos de memória não tem valores fixos.

Geralmente a menor unidade de um bloco de memória é um byte, em sistemas 32 bits temos até 2^32 possíveis endereços de memória o que representa 4GB de RAM. Enquanto em sistemas de 64 bits temos até 2^64 possíveis endereços de memória o que resulta em exabytes. Em sistemas 32 bits endereços de memória tem 4 bytes (32 bits) de comprimentos e sistema de 64 bits tem 8 bytes (64 bits) de comprimento.

Quando você aloca 10000 bytes em um programa em C e aloca caracteres o programa vai aparentar como uma única unidade para um endereço de memória. Entretanto se o sistema operacional não conseguir encontrar um bloco contíguo de 10000 bytes ele pode armazenar em mais de um endereço de memória e dividir em outros pedaços, entretanto quem cuida disso é o compilador da linguagem e para o programador é tratado como uma única unidade.

Assembly com atribuição de um valor a um *int:
<pre>
main:
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-12], 99
        lea     rax, [rbp-12]
        mov     QWORD PTR [rbp-8], rax
        mov     rax, QWORD PTR [rbp-8]
        mov     DWORD PTR [rax], 10
        mov     eax, 0
        pop     rbp
        ret
</pre>

Assembly com atribuição de um valor a um int: 

<pre>
main:
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR [rbp-4], 99
        mov     eax, 0
        pop     rbp
        ret
</pre>

<hr>

void* pointer é um ponteiro genérico que pode apontar para qualquer tipo. 

<hr>

Ponteiros podem apontar para outros ponteiros com a anotação int** p;

Isso signfica que a variável p armazena o endereço de um ponteiro de um int.

<pre>
int x = 10;
int *p = &x;
int **q = &p;
cout << &x << " " << p << " " << &p << " " << &q << " " << *q << " " << **q;
</pre>

p e &x tem o mesmo endereço de memória. 
&p e &q tem seu próprio endereço de memória. 
*q e p e &x tem o mesmo endereço de memória.
**q tem o valor 10.

<hr>

Quando não passamos um valor como um ponteiro em funções é feito um cópia do seu valor (pass by value) então mesmo que o valor seja modificado ele fica limitado a aquele escopo pois ele tem um endereço de memória diferente do valor passado para a função. 

Quando é passado como um ponteiro ele aponta pro mesmo endereço de memória então quando ele é alterado o efeito colateral é mantido já que uso o mesmo endereço.

<hr>

Arrays em C/C++ são sempre passado por referência. Quando um array é passado como argumento para uma função o que é de fato passado é um ponteiro para o primeiro elemento do array e não o array inteiro.

Isso faz com que não seja possível verificar o tamanho do array passado como parâmetro pois o valor de sizeof vai retornar o tamanho em bytes do ponteiro e não do array.

<pre>
void callArr(int arr[]) {
    cout << sizeof(arr) << endl;
    cout << arr << endl;
}

int x[] = {1,2,3,4,5};
cout << sizeof(x) << endl;
cout << &x << " " << &x[0] << endl;
callArr(x);
</pre>

&x, &x[0] e arr dentro da função callArr tem o mesmo endereço de memória.

<hr>

Assim como arrays de uma dimensão arrays de duas dimensões também funcionam a partir de ponteiros.

<pre>
int z[2][2] = {
    {1,2},
    {2,3}
};

cout << z << " " << " " << &z[0] << " " << z[0] << " " << &z[0][0] << " " << z[1] << " " << &z[1] << endl;

</pre>

z e &z[0], z[0] e &z[0][0] tem o mesmo endereço de memória
&z[1] e z[1] tem o mesmo endereço

Ou seja podemos fazer aritmética de ponteiros para avançar e ler os elementos do array.

<hr>

Em C++ objetos são instanciados diretamente sem o uso de ponteiro e assim que saem do escopo são destruídos `Foo f = Foo();` ou são criados como ponteiros. Para criar um objeto como ponteiro utilizamos a keyword `new` onde temos que se preocupar em dar `delete` quando necessário ou utilizamos smart pointers que gerenciam o ciclo de vida do objeto automaticamente.

Quando instanciamos usando `new` e `delete` dentro de uma classe ainda precisamos nos preocupar em dar o `delete` mesmo que seja feito o `delete` do objeto em si.

<pre>
class Inner {
public:
    Inner(int value) : data(value) {}
private:
    int data;
};

class MyClass {
public:
    MyClass() {
        innerPtr = new Inner(42); // Dynamically allocate Inner object
    }

    ~MyClass() {
        delete innerPtr; // Delete the dynamically allocated Inner object
    }

private:
    Inner* innerPtr;
};
</pre>

<hr>

Para declarar namespace você utiliza a sintaxe `namespace <namespace_name> {}` e no header file igual, você declara as funções classes etc. dentro do bloco de namespace. 

Isso permite por exemplo duas funções com mesmo nome, mas em diferente namespaces.

<hr>

Em C++ podemos definir que queremos passar um parâmetro por referência utilizando a sintaxe `type &<variable_name>`

Isso indica que não queremos copiar o valor da variável e trabalhar diretamente com o parâmetro passado, isso funciona de forma similiar a passar um ponteiro, mas tem algumas diferenças em questão de gerenciamento de memória.

Entretanto essas duas funções atingem o mesmo objetivo, alterar o valor passado por parâmetro.

<pre>
void callStr(std::string &s) {
    s = "asd";
    cout << &s << endl;
}

void callStr(std::string *s) {
    *s = "asd";
    cout << s << endl;
}
</pre>

<hr>

Por que std::string tem o sizeof de 32 bytes? Na implementação de std::string normalmente são utilizados campos auxiliares além de apenas um ponteiro de char.

Algo assim: 

<pre>
template <typename T>
struct basic_string {
    char* begin_;
    size_t size_;
    union {
        size_t capacity_;
        char sso_buffer[16];
    };
};
</pre>

char* tem 8 bytes, size_t tem 4 bytes somando esses tipos com o buffer de char[16] resulta em 32 bytes.

Esses valores podem mudar dependendo do compilador.

/*Comparaing float points
Set precision 
String manipulation, split, join, concat, indexOf, replace
Casting between types*/