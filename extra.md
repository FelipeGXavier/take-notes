### O que é um Mangler?

### Implementar subnet mask/CIDR usando bitwise

### Como binários são distrubuídos? apt, rpm, dev etc

Bit masking

Como funções como std::hex funcionam e são declaradas

(Output operators)

Por que não é possível chamar `auto x = 10 << std::hex`

É possível utilizar um tipo parametrizado T para alocar std::bitset de forma dinâmica em runtime?

Como typedef e typename são utilizado?

No trecho: 

<pre>
template<> std::istream &std::getline<char, std::char_traits<char>, std::allocator<char>>(std::istream &__is, std::string &__str, char __delim)
</pre>

O que significa?

<pre>
template<> std::istream &std::getline<char, std::char_traits<char>, std::allocator<char>>
</pre>



<hr>

Em C++ como melhor prática valores que sabemos que são constantes em tempo de compilação devem ser anotados como `constexpr` pois permite que o valor seja otimizado em tempo de compilação. Para valores que não conseguimos saber em tempo de compilação usamos `const`. 

<pre>
constexpr int radius = constants::pi * 10;
const int radius = getValue();
</pre>

<hr>

Em C++ const pode ser utilizado na assinatura de funções como retorno e parâmetro.

<pre>
std::string tp(const std::string& str) {
    return str;
}
</pre>

Nesse trecho o `const` indica que o valor de `str` não pode ser alterado, enquanto o `&` indica que o valor será passado por referência. Se não utilizar o `&` apesar de que não será possível alterar o valor de `str` o valor será copiado por valor em vez de referência.

O uso de `const` no retorno em tipos primitivos int, double, char, bool não permite que o valor retornado da função seja utilizado. O `const` no retorno define uma "visualização" de apenas leitura sem fazer uma cópia. Isso é útil no uso de ponteiros e atributos de classes.

<pre>
    const int* ptr(const int& value) {
        return &value;
    }

    int myFirstValue = 10;

    const int* v = ptr(myFirstValue);
    int newValue = -1;
    v = &newValue; // Fine
    *v = 10; // Error, trying to change const value
</pre>

`int const* const p;`

Signfica que não é possível alterar o valor para onde p aponta na memória nem mudar o seu endereço para outro.



### Ponteiros vs referência? *int vs int&

Em C++ há pelo duas formas de trabalhar com memória. Pointeiros apontam para um endereço de memória que armazena um valor, para alterar o valor que a ponteiro aponta precisamos "derefenciar" ele como `*p = 10;` ponteiros podem ser nulos `nullptr` e permitem ponteiros de ponteiros assim como aritmética. 

Referências funcionam de forma semelhante a ponteiros, mas com algumas mudanças. A sintaxe de referência é `&<T>` e funciona como ponteiros, alterar um valor passado como referência altera o valor da variável passada como argumento, por exemplo. Entretanto não é possível fazer aritmética como é feita como ponteiros assim como não é possível assumir um valor nulo ou ter uma referência de uma referência. 

Uma observação:

Uma causa comum de undefined behavior (UB) é retornar um ponteiro (ou referência) de uma variável local de uma função ou método. Assim que uma função é chamada as suas variáveis locais de escopo são removidas da memória então o ponteiro ou referência não tem mais essa informação.


<pre>
int* ub_ptr() {
    int i = 10;
    return &i;
}

int& ub_ref() {
    int i = 10;
    return i;
}

int x1 = *ub_ptr();
int x2 = ub_ref();
</pre>

Nesse trecho ambas chamadas causam undefined behaviour.

Uma forma de retornar um ponteiro de uma variável local sem ter UB é alocando dinamicante com new/delete (não recomendado) ou utilizando smart pointers. 

Uma forma de retornar uma referência de uma variável local sem ter UB é retornando uma variável global ou estática.

<hr>

Em C++ é comum separar a definição de uma classe/struct de sua implementação. 

É comum a definição da classe ser definida no arquivo de header da classe e sua implementação estar no arquivo .cpp dessa forma:

<pre>
// MyClass.H
#pragma once
class MyClass {
    public:
        MyClass(); // Constructor declaration
        void print();

    private:
        int data;
}

// MyClass.cpp
MyClass::MyClass() : data(42) {

}

void MyClass::print() {
    std::cout << this->data << std::endl;
}
</pre>

<hr>

Anteriormente vimos que variávies com escopo local tem um ciclo de vida definido pelo escopo. Uma função que cria uma variável local dentro do seu escopo e retorno levará a UB. Entretanto se essa variável local for criada como `static` não ocorrerá UB pois o ciclo de vida segue com o programa.

<hr>

`extern` é uma forma de utilizar métodos/variáveis definidos em outros arquivos sem ter que importar o header ou a definição inteira (um dos cenários de uso).

`inline` tem vários usos, sendo um a utilização para declarar variáveis globais sem duplicar a instância ao incluir um header file. Em funções o `inline` pode servir como uma otimização para em vez do valor ser computado pela chamada da função o código ser substituido com a implementação da função em si.

<hr>

Variáveis com `static` em funções fazem que a função tenha um estado interno. Se a função declara uma variável estática no seu corpo ela é criada apenas uma vez mesmo que a função seja chamada várias vezes.

<hr>

Em C++ `this` funciona como um ponteiro especial que aponta pro próprio objeto. A notação `->` é utilizada em conjunto com o `this` para interagir com o objeto internamente. A anotação `.` é utilizada para acessar quando temos um objeto ou referência. Como `this` é um ponteiro poderíamos derefenciar ele com `*` para utilizar a anotação de `.` dentro da classe.

<pre>
int Math::Maybe::get() {
    auto m = (*this).value;
    auto m2 = this->value;
    return *m;
}
</pre>

Mesmo utilizando smart pointers deve ser utilizado `->` para acessar os valores do objeto que o ponteiro aponta, o mesmo para ponteiros normais.

<pre>
    Math::Maybe* m = new Math::Maybe();
    delete m;
    // UB
    auto d = m->get();
</pre>


<hr>

Mesmo que não declaro em um header podemos ter apenas a forward declaration de uma função e ele será possível utilizar. Para ter encapsulamento temos que usar `static` ou `unamed namespaces` ou atribuir a um namespace indiretamente.

Ou seja, `unamed namespaces` é uma forma de encapsular o seu conteúdo de outras `translation units`. 

<hr>

Entende-se por `translation unit` o arquivo .cpp com a inclusão dos pré-processadores.

<hr>

Mesmo que em `translation units` diferentes, identificadores/funções devem ser únicos em C++ mesmo que as `translation units` não interagem diretamente.

<hr>

`inline namespaces` podem ser utilizados para definir um método padrão por precedência. 

<hr>

C++ permite generics com parâmetros qe não tipos como por exemplo, literais.

Algns tipos são permitidos como int, float e funcionam como um constexpr.

Por exemplo: 

<pre>
template <int N>
void all() {
    std::cout << N << std::endl;
}
all<5>();
</pre>

<hr>

C++ permite restringir, excluir possibilidade de tipos, com a sintaxe de `delete`.

<pre>
template <typename T>
T get(T t) {
    return t;
}

template<>
int get(int x) = delete;
</pre>

<hr>

Utilizando `type_traits` é possível definir regras para quais tipos atendem o parâmetro:

<pre>
template <typename T, class U>
typename std::enable_if<std::is_arithmetic<T>::value, T>::type
max(T x, U y) {
    return (x < y) ? x : y;
}
</pre>

<hr>

C++ faz a distinção entre categoria de seus tipos, inicialmente haviam `lvalues` e `rvalues`, com versões mais modernas outras categorias surgiram.

Expressões do tipo `lvalue` são aquelas que podem ser atribuídas a variáveis ou outros objetos que fornceçam uma identificação em que o ciclo de vida seja além da expressão executada.

Expressões do tipo `rvalue` são aquelas que atribuídas como literais ou retornadas em funções ou operadores, estas expressões são descartadas após sua execução e não são "identificáveis".

Por via de regra: se conseguimos usar o operator `&` para pegar o endereço de memória é um `lvalue`

<pre>
int x = 10; // x é um lvalue, 10 é um rvalue
10 = x; // Essa atribuição não funciona pois o lado esquerdo do operando espera
// um lvalue e não um rvalue
x = 10 + x; // Isso funciona porque por mais que o x seja um lvalue ele é convertido implicitamente para um rvalue
</pre>

Referência em variáveis são chamadas de `lvalue references` 

<pre>
int x = 10; // lvalue
int& y = x; // lvalue reference
</pre>

*Lembrando que referência não podem ser nulas, não podem apontar para outras referências. Referências não estão diretamente ligadas aos tipos de gerenciamento de memória. Elas servem como um "alias" para um objeto. Se criar uma referência que aponta para um ponteiro a referência será válida enquanto o ponteiro for.

O processo de atribuir uma referência de um tipo como `int& y = x` é normalmente chamado de `reference binding`.

Para dar bind utilizando `constexpr` o valor referenciado deve ser global ou estático pois o compilador sabe qual vai ser o endereço de memória previamente e consegue tratar como uma constante.

<pre>
static int s_x = 6;
constexpr int& ref = {s_x};
</pre>

<hr>

Em C++ quando passamos um ponteiro por parâmetro a função não pode alterar para onde ele ponta. A não ser que seja passado uma referência de um ponteiro `int*& x`.

<hr>

Usar `auto` dropa const e referência de um valor.

<hr>

<pre>
Fraction f = {1, 2};
Fraction f2 = f;
f.num1 = 10;
</pre>

Alterar f altera f2.

<hr>

Structs/classes tem pelo menos o tamanho em bytes somado de seus atributos, mas por conta de otimizações pode ser que seja além disso. Esse processo é chamado de padding, onde por vezes o programa adiciona um gap para facilitar certas operações. Um struct com atributos somados igual a 14 bytes pode ter um tamanho real de 16 bytes por exemplo.

<hr>

A partir do C++17 é possível usar dedução de tipos ao instanciar templates (CTAD). Isso signfifica que podemos instanciar implicitamente: 

<pre>
template <typename T>
struct Pair {
    T first;
    T second;
}

template <typename T>
Pair(T, T) -> Pair<T>

pair{1, 1};
</pre>

A partir do C++20 não é preciso declarar a dedução do template na maioria dos casos.