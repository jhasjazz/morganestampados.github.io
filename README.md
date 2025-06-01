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
      <a href="personaliza.html" class="hover:underline">Personaliza</a>
      <a href="contacto.html" class="hover:underline">Contacto</a>
      <a href="pagos.html" class="hover:underline">Pagos</a>
      <a href="compras.html" class="hover:underline">Mis Compras</a>
      <a href="reels.html" class="hover:underline">Reels</a>
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

      function guardarCarrito() {
        localStorage.setItem('carrito', JSON.stringify(carrito));
      }

      function obtenerCantidad(nombre) {
        const item = carrito.find(item => item.nombre === nombre);
        return item ? item.cantidad : 0;
      }

      function agregarAlCarrito(nombre, precio, index) {
        const itemExistente = carrito.find(item => item.nombre === nombre);
        if (itemExistente) {
          itemExistente.cantidad++;
        } else {
          carrito.push({ nombre, precio, cantidad: 1 });
        }
        guardarCarrito();
        document.getElementById(`contador-${index}`).value = obtenerCantidad(nombre);
      }

      function actualizarCantidadDesdeInput(nombre, valor, index) {
        const cantidad = parseInt(valor);
        const item = carrito.find(item => item.nombre === nombre);
        if (item) {
          item.cantidad = cantidad > 0 ? cantidad : 1;
        } else {
          carrito.push({ nombre, precio: 0, cantidad: cantidad });
        }
        guardarCarrito();
      }

      document.addEventListener("DOMContentLoaded", () => {
        const catalogo = document.getElementById("catalogo-grid");
        for (let i = 1; i <= 20; i++) {
          const nombre = i === 1 ? "Camiseta Pirata" : `Producto ${i}`;
          const precio = i === 1 ? 35000 : 20000 + i * 500;
          const cantidadActual = obtenerCantidad(nombre);
          const div = document.createElement('div');
          div.className = "bg-white p-4 rounded shadow flex flex-col items-center";
          div.innerHTML = `
            <img src="p${i}.jpeg" alt="${nombre}" class="w-full mb-2">
            <h3 class="font-bold text-center">${nombre}</h3>
            <p>$${precio}</p>
            <div class="flex flex-col mt-2 w-full items-center">
              <a href="p${i}.html" class="bg-purple-500 text-white px-4 py-2 rounded text-center mb-2 w-full">Ver detalles</a>
              <div class="flex items-center gap-2 w-full">
                <button onclick="agregarAlCarrito('${nombre}', ${precio}, ${i})" class="bg-pink-500 text-white px-4 py-2 rounded w-2/3 text-sm">
                  Añadir al carro ❤️
                </button>
                <input type="number" min="1" value="${cantidadActual}" id="contador-${i}" class="w-16 border rounded px-2 py-1 text-center" onchange="actualizarCantidadDesdeInput('${nombre}', this.value, ${i})">
              </div>
            </div>
          `;
          catalogo.appendChild(div);
        }
      });
    </script>
  </section>

  <!-- Botón flotante de WhatsApp parpadeante -->
  <a href="https://wa.link/ru46tm" target="_blank" class="fixed bottom-6 right-6 bg-green-500 text-white p-4 rounded-full shadow-lg parpadea text-xl font-bold">
    📩 Escríbenos por WhatsApp
  </a>
</body>
</html>
