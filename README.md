<p align="center"><img src="/images/logo.png" alt=""></p>
<h1 align="center">What the f*ck Golang! 😱</h1>
<p align="center">Exploring and understanding Golang through surprising snippets.</p>

> The idea and design for this collection were inspired by satwikkansal's awesome project [wtfpython](https://github.com/satwikkansal/wtfpython). 

Golang, one of the most facinating language, carrying out programming for scalable servers and large software systems. 
<!-- 
The Golang programming language was built to fill in the gaps of C++ and Java that Google came across while working with its servers and distributed systems. -->

Here's a fun project attempting to explain what exactly is happening under the hood for some counter-intuitive snippets and lesser-known features in Golang.

While some of the examples you see below may not be WTFs in the truest sense, but they'll reveal some of the interesting parts of Golang that you might be unaware of. I find it a nice way to learn the internals of a programming language, and I believe that you'll find it interesting too!

If you're an experienced Golang programmer, you can take it as a challenge to get most of them right in the first attempt. You may have already experienced some of them before, and I might be able to revive sweet old memories of yours! :sweat_smile:


So, here we go...



# Usage

A nice way to get the most out of these examples, in my opinion, is to read them in sequential order, and for every example:
- Carefully read the initial code for setting up the example. If you're an experienced Python programmer, you'll successfully anticipate what's going to happen next most of the time.
- Read the output snippets and,
  + Check if the outputs are the same as you'd expect.
  + Make sure if you know the exact reason behind the output being the way it is.
    - If the answer is no (which is perfectly okay), take a deep breath, and read the explanation (and if you still don't understand, shout out! and create an issue [here](https://github.com/dhamodaran-pandiyan/wtfgolang/issues)).
    - If yes, give a gentle pat on your back, and you may skip to the next example.




# 👀 Examples

## Section: Strain your brain!

<br>

### ▶ Careful with the braces!

<br>

1\.

```go
package main

import "fmt"

func main()  
{ 
    fmt.Println("hello there!")
}
```

**Output:**

```go
main.go:6: syntax error: unexpected semicolon or newline before {
```



#### 💡 Explanation:
+ In most other languages that use braces you get to choose where you place them. But, Go is different. 
+ You can thank automatic semicolon injection (without lookahead) for this behavior. Yes, Go does have semicolons :-)
+ Working program, 
      
    ```go
    package main

    import "fmt"

    func main() {  
        fmt.Println("works!")
    }
    ```



---


<br>

### ▶ Strings are immutable!

<br>

1\.

```go
package main

import "fmt"

func main() {
    x := "text"
    x[0] = 'T'

    fmt.Println(x)
}
```

**Output:**

```go
./prog.go:7:2: cannot assign to x[0] (value of type byte)

Go build failed.
```


#### 💡 Explanation:

- The immutability of strings is not the same as immutability of variables.

- Immutability of strings means that the characters in the string cannot be changed. This holds true for Go. 

- Variables in Go are always mutable. When a string variable is changed, the internal fields of the variable (pointer and length) are changed. The address of variable never changes.

- In Go, string is its own data type. At its core, it’s still a sequence of bytes, but:
  - It’s a fixed length. It doesn’t just continue on until a zero appears.
  - It comes with extra information: its length.
  - “Characters” or runes may span multiple bytes.
  - It’s immutable.

+ See the article on internals of string in Go http://research.swtch.com/godata.

+ Working program, 
      
    ```go
    package main

    import "fmt"

    func main() {
        x := "text"
        xbytes := []byte(x)
        xbytes[0] = 'T'

        fmt.Println(string(xbytes)) //prints Text
    }
    ```



---

# Contributing

A few ways in which you can contribute to wtfgolang,

- Suggesting new examples
- Minor corrections like pointing out outdated snippets, typos, formatting errors, etc.
- Identifying gaps (things like inadequate explanation, redundant examples, etc.)
- Any creative suggestions to make this project more fun and useful

Please see [CONTRIBUTING.md](/CONTRIBUTING.md) for more details. Feel free to create a new [issue](https://github.com/dhamodaran-pandiyan/wtfgolang/issues/new/choose) to discuss things.


# Acknowledgements

The idea and design for this collection were initially inspired by satwikkansal's awesome project [wtfpython](https://github.com/satwikkansal/wtfpython). 

#### Some nice Links!
* Link 1
* Link 2


# 🎓 License

[![WTFPL 2.0][license-image]][license-url]

&copy; [Dhamodaran pandiyan](https://github.com/dhamodaran-pandiyan)

[license-url]: http://www.wtfpl.net
[license-image]: https://img.shields.io/badge/License-WTFPL%202.0-lightgrey.svg?style=flat-square

## Surprise your friends as well!
