1. O que essa sintaxe faz em Go?

<pre>
c := make(chan ObjType)
for d := range c {}
</pre>

R.: A sintaxe for range *channel* em Go é utilizada para iterar os valores que o canal recebeu. Então se eu tenho canal de int e envio 3 valores iterar sobre ele vai me retornar os 3 valores. 

Para conseguir iterar dessa forma o canal deve estar fechado com close.

<pre>
func main() {
	c := make(chan int, 2)

	go func() {
		time.Sleep(2 * time.Second)
		c <- 2
		c <- 3
		close(c)
	}()

	for d := range c {
		fmt.Println(d)
	}
}
</pre>

2. Como que conseguimos conectar em chamadas ponto a ponto como VOIP sendo que na maiorias das vezes estamos através de um NAT e uma rede interna não exposta?
