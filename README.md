<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Loja Online</title>
  <link rel="stylesheet" href="style.css"/>
</head>
<body>
  <header>
    <h1>Minha Loja</h1>
    <div id="cart">
      🛒 Carrinho: <span id="cart-count">0</span> itens
    </div>
  </header>

  <main>
    <div class="product">
      <h2>Camisa Estilosa</h2>
      <p>R$ 59,90</p>
      <button onclick="addToCart('Camisa Estilosa', 59.90)">Comprar</button>
    </div>

    <div class="product">
      <h2>Tênis Esportivo</h2>
      <p>R$ 199,90</p>
      <button onclick="addToCart('Tênis Esportivo', 199.90)">Comprar</button>
    </div>

    <div class="product">
      <h2>Mochila Moderna</h2>
      <p>R$ 89,90</p>
      <button onclick="addToCart('Mochila Moderna', 89.90)">Comprar</button>
    </div>

    <section id="cart-items">
      <h2>Itens no Carrinho</h2>
      <ul id="cart-list"></ul>
      <p>Total: R$ <span id="cart-total">0.00</span></p>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>

