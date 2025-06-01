<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Morgan Estampados</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .parpadea {
      animation: blink 1s infinite;
    }
    @keyframes blink {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }
  </style>
</head>
<body class="text-gray-900">
  <!-- Barra de navegación -->
  <header class="bg-red-700 text-white p-4 shadow-md flex justify-between items-center">
    <h1 class="text-2xl font-bold">Morgan Estampados</h1>
    <nav class="space-x-4">
      <a href="#catalogo" class="hover:underline">Inicio</a>
      <a href="#personaliza" class="hover:underline">Personaliza</a>
      <a href="#contacto" class="hover:underline">Contacto</a>
      <a href="#pagos" class="hover:underline">Pagos</a>
      <a href="carro.html" class="hover:underline">Carrito</a>
      <a href="#compras" class="hover:underline">Mis Compras</a>
      <button id="loginBtn" class="bg-white text-red-700 px-2 py-1 rounded">Login Google</button>
    </nav>
  </header>

  <!-- Frase Pirata -->
  <section class="bg-black text-white text-center p-2 text-lg italic">
    <p>“¡Arrrr! Que tu estilo navegue con nosotros, marinero del diseño.”</p>
  </section>

  <!-- Catálogo de productos -->
  <section id="catalogo" class="p-6">
    <h2 class="text-3xl font-semibold text-center mb-6">Catálogo de Productos</h2>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6" id="catalogo-grid">
      <!-- Los productos se agregarán por JS -->
    </div>
    <script>
      let carrito = JSON.parse(localStorage.getItem('carrito')) || [];

      function agregarAlCarrito(nombre, precio, index) {
        const itemExistente = carrito.find(item => item.nombre === nombre);
        if (itemExistente) {
          itemExistente.cantidad++;
        } else {
          carrito.push({ nombre, precio, cantidad: 1 });
        }
        localStorage.setItem('carrito', JSON.stringify(carrito));
        document.getElementById(`contador-${index}`).textContent = carrito.find(item => item.nombre === nombre).cantidad;
      }

      document.addEventListener("DOMContentLoaded", () => {
        const catalogo = document.getElementById("catalogo-grid");
        for (let i = 1; i <= 20; i++) {
          const nombre = i === 1 ? "Camiseta Pirata" : `Producto ${i}`;
          const precio = i === 1 ? 35000 : 20000 + i * 500;
          const div = document.createElement('div');
          div.className = "bg-white p-4 rounded shadow flex flex-col items-center";
          div.innerHTML = `
            <img src="p${i}.jpeg" alt="${nombre}" class="w-full mb-2">
            <h3 class="font-bold">${nombre}</h3>
            <p>$${precio}</p>
            <div class="flex flex-col mt-2 w-full justify-center">
              <a href="p${i}.html" class="bg-purple-500 text-white px-4 py-2 rounded text-center mb-2">Ver detalles</a>
              <button onclick="agregarAlCarrito('${nombre}', ${precio}, ${i})" class="bg-pink-500 text-white px-4 py-2 rounded flex items-center justify-center gap-2">
                Añadir al carro <span>❤️</span> <span id="contador-${i}" class="ml-2 bg-white text-pink-600 px-2 rounded">0</span>
              </button>
            </div>
          `;
          catalogo.appendChild(div);
        }
      });
    </script>
  </section>

  <!-- Sección Mis Compras -->
  <section id="compras" class="p-6 bg-white">
    <h2 class="text-2xl font-bold text-center mb-4">Compras Realizadas</h2>
    <p class="text-center">Esta sección estará disponible próximamente con seguimiento de pedidos.</p>
  </section>

  <!-- Personaliza tu prenda -->
  <section id="personaliza" class="p-6 bg-white">
    <h2 class="text-2xl font-bold text-center mb-4">Personaliza tu prenda</h2>
    <form class="max-w-xl mx-auto space-y-4">
      <input type="text" placeholder="Nombre completo" class="w-full border p-2 rounded">
      <input type="email" placeholder="Correo electrónico" class="w-full border p-2 rounded">
      <input type="file" class="w-full border p-2 rounded">
      <select class="w-full border p-2 rounded">
        <option>Camiseta</option>
        <option>Gorra</option>
        <option>Tote Bag</option>
      </select>
      <textarea placeholder="Detalles del diseño" class="w-full border p-2 rounded"></textarea>
      <a href="https://wa.link/ru46tm" target="_blank" class="block bg-red-700 text-white px-4 py-2 text-center rounded">Enviar por WhatsApp</a>
    </form>
  </section>

  <!-- Contacto -->
  <section id="contacto" class="p-6">
    <h2 class="text-2xl font-bold text-center mb-4">Contacto</h2>
    <form class="max-w-xl mx-auto space-y-4">
      <input type="text" placeholder="Nombre" class="w-full border p-2 rounded">
      <input type="email" placeholder="Correo" class="w-full border p-2 rounded">
      <textarea placeholder="Mensaje" class="w-full border p-2 rounded"></textarea>
      <button class="bg-red-700 text-white px-4 py-2 rounded">Enviar</button>
    </form>
  </section>

  <!-- Pagos en línea -->
  <section id="pagos" class="p-6 bg-white">
    <h2 class="text-2xl font-bold text-center mb-4">Pagos en Línea</h2>
    <p class="text-center mb-2">Realiza tu pago a través de nuestros métodos autorizados.</p>
    <div class="flex flex-wrap justify-center gap-4">
      <a href="#" class="bg-green-600 text-white px-4 py-2 rounded">Nequi</a>
      <a href="#" class="bg-yellow-500 text-white px-4 py-2 rounded">Bancolombia</a>
      <a href="#" class="bg-blue-700 text-white px-4 py-2 rounded">Daviplata</a>
    </div>
  </section>

  <!-- Botón flotante de WhatsApp parpadeante -->
  <a href="https://wa.link/ru46tm" target="_blank" class="fixed bottom-6 right-6 bg-green-500 text-white p-4 rounded-full shadow-lg parpadea text-xl font-bold">
    📩 Escríbenos por WhatsApp
  </a>

  <!-- Script de redirección al login -->
  <script>
    const loginBtn = document.getElementById("loginBtn");
    loginBtn.addEventListener("click", () => {
      window.location.href = "login.html";
    });
  </script>
</body>
</html>
