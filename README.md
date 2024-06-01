# Ping-Pong-com-Go

Vou criar um artigo sobre como implementar um jogo de Ping Pong no terminal usando Go, com foco no fluxo de impressão e registro de mensagens. O artigo será dividido em módulos para facilitar a compreensão.

---

## Introdução

Neste artigo, vamos desenvolver um simples jogo de Ping Pong no terminal. O objetivo é entender como funciona o fluxo de impressão e registro de mensagens em uma aplicação Go. Vamos criar um padrão de mensagens que será usado durante o jogo.

## Módulo 1: Configuração do Projeto

Primeiro, crie um novo diretório para o seu projeto e inicialize um novo módulo Go:

```bash
mkdir pingpong-go
cd pingpong-go
go mod init pingpong-go
```

## Módulo 2: Estrutura Básica do Jogo

Vamos começar com a estrutura básica do nosso jogo de Ping Pong.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c := make(chan string)

    go func() {
        for {
            c <- "Ping"
            time.Sleep(time.Second * 1)
        }
    }()

    go func() {
        for {
            msg := <-c
            fmt.Println(msg)
            c <- "Pong"
        }
    }()

    var input string
    fmt.Scanln(&input)
}
```

## Módulo 3: Fluxo de Mensagens

Agora, vamos implementar o fluxo de mensagens que será exibido no terminal.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c := make(chan string)

    go ping(c)
    go pong(c)

    var input string
    fmt.Scanln(&input)
}

func ping(c chan string) {
    for {
        c <- "Ping"
        fmt.Println("Ping enviado!")
        time.Sleep(time.Second * 1)
    }
}

func pong(c chan string) {
    for {
        msg := <-c
        fmt.Println(msg, "recebido!")
        c <- "Pong"
        fmt.Println("Pong enviado!")
    }
}
```

## Módulo 4: Registro de Mensagens

Para fins de depuração, podemos adicionar um registro de mensagens.

```go
package main

import (
    "fmt"
    "log"
    "os"
    "time"
)

func main() {
    c := make(chan string)
    file, err := os.OpenFile("pingpong.log", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
    if err != nil {
        log.Fatal(err)
    }
    log.SetOutput(file)

    go ping(c)
    go pong(c)

    var input string
    fmt.Scanln(&input)
}

func ping(c chan string) {
    for {
        c <- "Ping"
        log.Println("Ping enviado!")
        time.Sleep(time.Second * 1)
    }
}

func pong(c chan string) {
    for {
        msg := <-c
        log.Println(msg, "recebido!")
        c <- "Pong"
        log.Println("Pong enviado!")
    }
}
```

## Conclusão

Este artigo apresentou como criar um jogo de Ping Pong no terminal com Go, focando no fluxo de impressão e registro de mensagens. Experimente adicionar mais funcionalidades e personalizar o jogo conforme desejar.

---

Espero que este artigo tenha sido útil para entenderem como trabalhar com impressão e registro de mensagens em Go. Divirtam-se codificando e aprimorando suas habilidades de programação!
