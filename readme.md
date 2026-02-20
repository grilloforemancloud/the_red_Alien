# Boas Práticas e Padrões Idiomáticos em Go e C

## 🎯 Introdução
Nem sempre boas práticas são consideradas *design patterns* formais. Porém, quando uma técnica se torna **a única forma eficaz de resolver problemas recorrentes**, ela deixa de ser apenas "higiene de código" e passa a funcionar como um **padrão idiomático da linguagem**.

Este documento explora como **structs, composição e interfaces** em Go (e até em C) podem ser vistos como soluções de design, aplicando princípios como **SRP** e **OCP** do SOLID.

---

## 🔑 Questões que não são bem resolvidas sem `struct`, composição e interfaces

1. **Representação de entidades complexas**  
   - Sem `structs`, dados relacionados ficam dispersos em variáveis soltas ou mapas.  
   - `structs` permitem modelar entidades como `User`, `Order`, `Config`.

2. **Extensibilidade sem modificar código existente (OCP)**  
   - Interfaces permitem definir contratos e criar múltiplas implementações.  
   - Sem interfaces, seria necessário usar condicionais extensos (`if/else` ou `switch`).

3. **Polimorfismo sem herança**  
   - Go não possui herança clássica, mas interfaces + composição permitem polimorfismo.  
   - Exemplo: qualquer tipo que implemente `io.Reader` pode ser usado em funções que esperam um `Reader`.

4. **Separação de responsabilidades (SRP)**  
   - Composição de `structs` divide responsabilidades em partes menores.  
   - Exemplo: um `Logger` pode ser embutido em diferentes structs sem duplicação.

5. **Testabilidade e desacoplamento**  
   - Interfaces permitem criar *mocks* em testes.  
   - Sem elas, o código ficaria preso a implementações concretas.

---

## 🧩 Exemplos em Go

### Interface pequena (SRP + ISP)
```go
type Reader interface {
    Read(p []byte) (n int, err error)
}


•   Qualquer tipo que implemente  pode ser usado como .
• 	Isso reduz acoplamento e aumenta flexibilidade.

```
### Composição de structs (OCP)

``` type Logger struct {}

func (l Logger) Log(msg string) {
    fmt.Println(msg)
}

type Service struct {
    Logger
}

func (s Service) DoWork() {
    s.Log("Executando tarefa...")
}
```

- Service reutiliza Logger sem herança.
- É possível adicionar novos comportamentos sem alterar Logger.

### Paralelo em C
Em C, não há interfaces, mas é possível simular comportamentos semelhantes com structs + ponteiros de função:
``` typedef struct {
    int (*operation)(int, int);
} Strategy;

int add(int a, int b) { return a + b; }
int multiply(int a, int b) { return a * b; }

Strategy s;
s.operation = add;
printf("%d\n", s.operation(2, 3)); // 5
```

- Aqui, Strategy funciona como uma interface.
- Diferentes funções podem ser atribuídas sem mudar o código que usa Strategy.


### Conclusão
- Boas práticas viram padrões quando são a solução recorrente para problemas de design.
- Em Go, structs, composição e interfaces não são apenas conveniência: são necessários para resolver problemas de extensibilidade, polimorfismo e testabilidade.
- Em C, o uso de structs e ponteiros de função cumpre papel semelhante.
👉 Assim, podemos dizer que padrões idiomáticos emergem naturalmente das boas práticas, mesmo em linguagens que não são puramente orientadas a objetos.

---

Esse documento já está pronto para ser publicado em um repositório GitHub como artigo técnico.  

Quer que eu monte também uma **estrutura de README.md** com seções típicas (Introdução, Exemplos, Conclusão, Referências) para deixar ainda mais no formato de projeto open source?


---

Esse documento já está pronto para ser publicado em um repositório GitHub como artigo técnico.  

Quer que eu monte também uma **estrutura de README.md** com seções típicas (Introdução, Exemplos, Conclusão, Referências) para deixar ainda mais no formato de projeto open source?


# Boas Práticas e Padrões Idiomáticos em Go e C

## 🎯 Introdução
Nem sempre boas práticas são consideradas *design patterns* formais. Porém, quando uma técnica se torna **a única forma eficaz de resolver problemas recorrentes**, ela deixa de ser apenas "higiene de código" e passa a funcionar como um **padrão idiomático da linguagem**.

Este documento explora como **structs, composição e interfaces** em Go (e até em C) podem ser vistos como soluções de design, aplicando princípios como **SRP** e **OCP** do SOLID.

---

## 🔑 Questões que não são bem resolvidas sem `struct`, composição e interfaces

1. **Representação de entidades complexas**  
2. **Extensibilidade sem modificar código existente (OCP)**  
3. **Polimorfismo sem herança**  
4. **Separação de responsabilidades (SRP)**  
5. **Testabilidade e desacoplamento**

---

## ✅ Isso resolve
- **Agrupar dados relacionados**: `structs` evitam variáveis soltas e tornam entidades mais claras.  
- **Flexibilidade com interfaces**: contratos permitem múltiplas implementações sem alterar código existente.  
- **Reuso com composição**: embutir comportamentos (`Logger`, `Config`) em diferentes structs sem duplicação.  
- **Testes mais simples**: interfaces permitem *mocks* e reduzem acoplamento.  
- **Polimorfismo idiomático**: qualquer tipo que implemente uma interface pode ser usado em funções genéricas.

---

## ❌ Isso não resolve
- **Complexidade desnecessária**: usar interfaces para algo que nunca terá mais de uma implementação só adiciona ruído.  
- **Herança clássica**: Go não tem, e tentar simular herança com composição pode gerar confusão.  
- **Gerenciamento de memória avançado**: structs não substituem técnicas de otimização de baixo nível (como em C).  
- **Problemas de concorrência**: interfaces e composição não resolvem *race conditions*; é preciso usar *goroutines* e *channels*.  
- **Validação de dados complexa**: structs ajudam a organizar, mas não substituem lógica de validação robusta.

---

## 🧩 Exemplos em Go

### Interface pequena (SRP + ISP)
```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

###Composição de structs (OCP)

``` type Logger struct {}

func (l Logger) Log(msg string) {
    fmt.Println(msg)
}

type Service struct {
    Logger
}

func (s Service) DoWork() {
    s.Log("Executando tarefa...")
}
```
### Paralelo em 
```typedef struct {
    int (*operation)(int, int);
} Strategy;

int add(int a, int b) { return a + b; }
int multiply(int a, int b) { return a * b; }

Strategy s;
s.operation = add;
printf("%d\n", s.operation(2, 3)); // 5

```
Conclusão
- Boas práticas viram padrões quando são a solução recorrente para problemas de design.
- Em Go, structs, composição e interfaces não são apenas conveniência: são necessários para resolver problemas de extensibilidade, polimorfismo e testabilidade.
- Em C, o uso de structs e ponteiros de função cumpre papel semelhante.
- Mas é importante saber quando aplicar: nem todo problema precisa de interface ou composição.
👉 Assim, podemos dizer que padrões idiomáticos emergem naturalmente das boas práticas, mesmo em linguagens que não são puramente orientadas a objetos.

---

Esse “isso faz / isso não faz” ajuda a mostrar que não é só sobre aplicar técnicas, mas também sobre **saber quando não aplicar**.  

Quer que eu monte esse documento já no formato de **README.md de projeto open source**, com seções típicas como *Instalação*, *Uso*, *Exemplos* e *Contribuição* para ficar pronto para subir no GitHub?



