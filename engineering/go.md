## Em Golang existe um diferença entre *buffered channels* e *unbuffered channels*. 

A diferença está forma que cada um sincroniza suas operações. 

Unbuffered channels tem uma operação estrita entre quem envia e quem recebe o valor do canal. 

Quando utilizamos unbuffered channel e enviamos um valor ao canal essa operação bloqueia até algum recebedor esteja pronto para recebr o valor escrito no canal. Veja o seguinte trecho de código: 

<pre>
func write(ch chan int) {
	fmt.Println("Start sending");
	ch <- 10;
	fmt.Println("Finished");
}

func receive(ch chan int) {
	time.Sleep(1 * time.Second);
	x := <- ch;
	fmt.Println("Received value from channel", x);
}

func main() {
   ch := make(chan int);
   go write(ch);
   go receive(ch);
   time.Sleep(5 * time.Second);
}
</pre>

Assim que é iniciado uma goroutine para *write* e é escrito o valor de 10 no canal a goroutine fica suspensa "bloqueia" até a função *receive* receba o valor escrito no canal e somente depois disso que conseguiremos ver o texto "Finished" escrito na tela.

Agora em buffered channels a escrita num canal não vai ser bloqueante a não ser que seu buffer esteja cheio, enquanto em quem recebe, não será bloqueante desde que o canal não esteja vazio. 

<pre>
func write(ch chan int) {
	fmt.Println("Start sending");
	ch <- 10;
	fmt.Println("Finished");
}

func receive(ch chan int) {
	time.Sleep(3 * time.Second);
	x := <- ch;
	fmt.Println("Received value from channel", x);
}

func main() {
   ch := make(chan int, 1);
   go write(ch);
   go write(ch);
   go receive(ch);
   go receive(ch);
   time.Sleep(5 * time.Second);
}
</pre>

É mandado dois comandos de *write* num canal com capacity de 1. A primeira escrita passa e vemos "Finished" na tela de imediado. Na segunda escrita o buffer do canal já está cheio, então a ação é bloqueada e só vemos o "Finished" depois que um dos valores do canal é recebido na função *receive*.