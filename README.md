# Desafio Dio Criando um Carrinho de Compras em Rust usando o Rocket



**Como criar um carrinho de compras em Rust usando o Rocket**



O Rocket é um framework web para Rust que facilita a criação de APIs e serviços web. Para criar um carrinho de compras usando o Rocket, você pode seguir estes passos:



1. Crie um novo projeto Rust usando o comando `cargo new`.

   

2. Adicione a dependência do Rocket ao seu projeto adicionando a seguinte linha ao seu arquivo `Cargo.toml`:





```plaintext
[dependencies]
rocket = "0.5.0"
```



1. #### Crie um novo arquivo chamado `main.rs` e adicione o seguinte código:

rust



```rust
use rocket::*;

#[get("/carrinho")]
fn carrinho() -> String {
    "Seu carrinho está vazio."
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![carrinho])
}
```



1. #### Execute seu aplicativo usando o comando `cargo run`.

Este código cria uma rota simples do Rocket que retorna uma mensagem informando que o carrinho está vazio. Você pode adicionar mais funcionalidade ao seu carrinho de compras, como adicionar itens, remover itens e calcular o total.

Aqui estão alguns recursos adicionais que podem ser úteis:

- Documentação do Rocket
- Tutorial do Rocket
- Exemplo de carrinho de compras do Rocket



## Código Completo e Abrangente para Criar um Carrinho de Compras em Rust usando o Rocket



**Definição de cada parte do projeto para criar um carrinho de compras em Rust usando o Rocket:**

#### **Rocket:**

- É um framework web para Rust que facilita a criação de APIs e serviços web.

- Ele fornece um conjunto de macros e atributos que podem ser usados para definir rotas, manipuladores de rotas e outros componentes de um aplicativo web.

  

#### **Carrinho de compras:**

- É um objeto que armazena os itens que um usuário deseja comprar.

- Ele normalmente contém uma lista de itens, cada um com uma quantidade e um preço.

  

#### **Item:**

- É um objeto que representa um único item em um carrinho de compras.

- Ele normalmente contém um ID, nome, preço e quantidade.

  

#### **Rota:**

- É um caminho que mapeia uma solicitação HTTP para um manipulador de rota específico.

- No Rocket, as rotas são definidas usando o atributo `#[get]` ou `#[post]`.

  

#### **Manipulador de rota:**

- É uma função que processa uma solicitação HTTP e retorna uma resposta.

- No Rocket, os manipuladores de rota são definidos usando o atributo `fn`.

  

#### **Estado do Rocket:**

- É um objeto que pode ser compartilhado entre diferentes manipuladores de rota.
- Ele pode ser usado para armazenar dados que precisam ser acessados por vários manipuladores de rota.



rust



```rust
use rocket::*;

#[derive(Serialize, Deserialize)]
struct Item {
    id: i32,
    name: String,
    price: f32,
    quantity: i32,
}

#[derive(Serialize, Deserialize)]
struct Cart {
    items: Vec<Item>,
}

#[get("/carrinho")]
fn carrinho(carrinho: State<Cart>) -> Json<Cart> {
    Json(carrinho.inner())
}

#[post("/carrinho/adicionar")]
fn adicionar_item(carrinho: State<Cart>, item: Json<Item>) -> Json<Cart> {
    carrinho.inner_mut().items.push(item.into_inner());
    Json(carrinho.inner())
}

#[post("/carrinho/remover")]
fn remover_item(carrinho: State<Cart>, id: Json<i32>) -> Json<Cart> {
    carrinho.inner_mut().items.retain(|item| item.id != id.into_inner());
    Json(carrinho.inner())
}

#[post("/carrinho/atualizar")]
fn atualizar_item(carrinho: State<Cart>, item: Json<Item>) -> Json<Cart> {
    let index = carrinho.inner_mut().items.iter().position(|item| item.id == id.into_inner()).unwrap();
    carrinho.inner_mut().items[index] = item.into_inner();
    Json(carrinho.inner())
}

#[launch]
fn rocket() -> _ {
    rocket::build().manage(Cart { items: vec![] }).mount("/", routes![carrinho, adicionar_item, remover_item, atualizar_item])
}
```



Este código cria um carrinho de compras simples usando o Rocket. O carrinho de compras é representado por uma estrutura `Cart` que contém uma lista de itens. Os itens são representados por uma estrutura `Item` que contém um ID, nome, preço e quantidade.

As rotas do Rocket são usadas para gerenciar o carrinho de compras. A rota `/carrinho` retorna o conteúdo do carrinho de compras. A rota `/carrinho/adicionar` adiciona um item ao carrinho de compras. A rota `/carrinho/remover` remove um item do carrinho de compras. A rota `/carrinho/atualizar` atualiza um item no carrinho de compras.

O estado do carrinho de compras é gerenciado usando o estado do Rocket. O estado do Rocket é um objeto que pode ser compartilhado entre diferentes manipuladores de rotas. Neste caso, o estado do carrinho de compras é compartilhado entre todos os manipuladores de rotas que precisam acessar o carrinho de compras.
