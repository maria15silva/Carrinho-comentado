const produtos = [
    {
        id: "1",
        nome: "Informática para Internet: Interfaces Web II",
        prof: "Prof. Kelly",
        preco_de: 80,
        preco_por: 50,
        descricao: "O melhor curso de JavaScript",
        imagem: "./assets/1.png",
    },
    {
        id: "2",
        nome: "Gestão de conteúdo Web II",
        prof: "Prof. Kelly",
        preco_de: 80,
        preco_por: 50,
        descricao: "O melhor curso de JavaScript",
        imagem: "./assets/3.png",  
    }
];
//Nesta parte, um array 'produtos' é definido, contendo objetos que representam produtos 
//com várias informações, como nome, preço, descrição e imagem.

function renderizaProdutos(){
    let html = "";
    for(let i = 0; i < produtos.length; i++){
        html = html + criarProduto(produtos[i], i);
    }
    return html;
}
//A função 'renderizaProdutos' cria uma string HTML que contém todos os produtos da 
//lista. Ela itera sobre o array 'produtos' e chama a função 'criarProduto' para criar 
//a representação HTML de cada produto.

function criarProduto(produto, index) {
    return `
    <div class = "curso">
    <img class = 'inicio' title="t" src="${produto.imagem}" />
    <div class = "curso-info">
      <h4>${produto.nome}</h4>
      <h4>${produto.prof}</h4>
      <h4>${produto.descricao}</h4>
      </div>
      <div class = "curso-preco">
      <span class="preco-de">R$${produto.preco_de}</span>
      <span class="preco-por">R$${produto.preco_por}</span>
      <button class="btncar btn-add" data-index="${index}"></button>
      </div>
      </div>
      `;
}
//A função 'criarProduto' gera uma representação HTML para um único produto. Ela 
//recebe um objeto 'produto' e um índice 'index' como argumentos e retorna uma string 
//HTML que exibe as informações do produto, incluindo nome, professor, descrição, preços e uma imagem.

const container = document.querySelector("#container")
container.innerHTML = renderizaProdutos();
//O código seleciona um elemento HTML com o id "container" e substitui seu conteúdo HTML 
//pelo resultado da função 'renderizaProdutos()'. Isso significa que a lista de produtos 
//será exibida dentro desse elemento na página.

const carrinhoItens = {};
//Nesta linha, é definido um objeto vazio 'carrinhoItens' que será usado para rastrear os 
//produtos adicionados ao carrinho de compras. Os produtos serão armazenados no objeto 
//usando o 'id' do produto como chave.

function renderizaCarrinho () {
    let html = '';
    for (let produtoId in carrinhoItens) {
        html = html + criaItemCarrinho(carrinhoItens[produtoId]);
    }
    document.querySelector('.carrinho_itens').innerHTML = html;
}
//A função 'renderizaCarrinho' cria uma representação HTML do carrinho de compras. Ela 
//itera pelos produtos no objeto 'carrinhoItens' e chama a função 'criaItemCarrinho' para 
//criar a representação HTML de cada item no carrinho. O HTML gerado é então inserido 
//dentro de um elemento com a classe "carrinho_itens" na página.

function criaItemCarrinho(produto){
    return  `
    <div class = "carrinho_compra">
    <h4>${produto.nome}</h4>
    <p>Preço unidade: ${produto.preco_por} | Quantidade: ${produto.quantidade}</p>
    <p>Valor: R$: ${produto.preco_por*produto.quantidade}</p>
    <button data-produto-id="${produto.id}" class="btn-remove"> </button>
    </div>
    `;
}
//A função 'criaItemCarrinho' recebe um objeto 'produto' do carrinho e cria uma 
//representação HTML para um item no carrinho. Ela exibe o nome do produto, 
//o preço unitário, a quantidade e o valor total desse item. Além disso, ela 
//inclui um botão com um atributo 'data-produto-id' que armazena o 'id' do produto 
//para identificação quando o usuário desejar remover um item do carrinho.

function criaCarrinhoTotal (){
    let total = 0;
    for (let produtoId in carrinhoItens) {
        total = total + carrinhoItens[produtoId].preco_por * carrinhoItens[produtoId].quantidade;
    }
    document.querySelector('.carrinho_total').innerHTML = `
    <h4>Total: <strong> R$${total} </strong</h4>
    <a href ="#" target="_blank">
    <ion-icon name="card-outline"></ion-icon>
    <strong>Comprar Agora</strong
    </a>
    `;}
    //A função 'criaCarrinhoTotal' calcula o valor total dos itens no carrinho somando o preço 
    //unitário multiplicado pela quantidade de cada item. Em seguida, ela atualiza o conteúdo 
    //de um elemento com a classe "carrinho_total" na página, exibindo o valor total e um link para "Comprar Agora."

    function adicionaItemNoCarrinho(produto){
        if (!carrinhoItens[produto.id]) {
            carrinhoItens[produto.id] = produto;
            carrinhoItens[produto.id].quantidade = 0;
        }++carrinhoItens[produto.id].quantidade;
        renderizaCarrinho();
        criaCarrinhoTotal();}
        //A função 'adicionaItemNoCarrinho' recebe um produto como argumento e adiciona esse 
        //produto ao carrinho de compras. Se o produto ainda não estiver no carrinho, ele 
        //é adicionado ao objeto 'carrinhoItens', e a quantidade é inicializada como zero. Em 
        //seguida, a quantidade é incrementada em 1. Depois disso, as funções 'renderizaCarrinho' 
        //e 'criaCarrinhoTotal' são chamadas para atualizar a exibição do carrinho.

        document.body.addEventListener('click' , function (event){
            const elemento = event.target;
            if(elemento.classList.contains('btn-add')) {
                const index = parseInt(elemento.getAttribute('data-index'), 10);
                const produto = produtos[index];

                adicionaItemNoCarrinho(produto);
            }
            if (elemento.classList.contains('btn-remove')) {
                const produtoId = elemento.getAttribute('data-produto-id');
                if(carrinhoItens[produtoId].quantidade <= 1) {
                    delete carrinhoItens[produtoId];
                } else {
                    --carrinhoItens[produtoId].quantidade;
                }
                renderizaCarrinho();
                criaCarrinhoTotal();
            }
        });
        //Nesse bloco de código, é adicionado um ouvinte de eventos ao elemento '<body>' da 
        //página para capturar cliques. Quando o usuário clica em algum lugar da página, 
        //esse código verifica se o elemento clicado possui a classe "btn-add" ou "btn-remove". 
        //Se o elemento clicado for um botão de adição (classe "btn-add"), ele obtém o índice 
        //do produto a partir do atributo 'data-index' do botão e adiciona o produto ao carrinho. 
        //Se for um botão de remoção (classe "btn-remove"), ele remove o produto do carrinho, 
        //desde que a quantidade seja maior que 1, ou exclui completamente o produto se a 
        //quantidade for igual a 1. Em seguida, as funções 'renderizaCarrinho' e 'criaCarrinhoTotal' 
        //são chamadas para atualizar a exibição do carrinho.
