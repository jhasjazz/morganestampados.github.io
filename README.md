<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Morgan Estampados</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-auth-compat.js"></script>
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
    <nav class="space-x-4 flex items-center">
      <a href="#catalogo" class="hover:underline">Inicio</a>
      <a href="#personaliza" class="hover:underline">Personaliza</a>
      <a href="#contacto" class="hover:underline">Contacto</a>
      <a href="#pagos" class="hover:underline">Pagos</a>
      <a href="carro.html" class="hover:underline">Carrito</a>
      <a href="#compras" class="hover:underline">Mis Compras</a>
      <button id="loginBtn" class="bg-white text-red-700 px-2 py-1 rounded">Login Google</button>
      <div id="userCircle" class="hidden w-8 h-8 rounded-full bg-white text-red-700 font-bold flex items-center justify-center"></div>
    </nav>
  </header>

  <!-- Frase Pirata -->
  <section class="bg-black text-white text-center p-2 text-lg italic">
    <p>“¡Arrrr! Que tu estilo navegue con nosotros, marinero del diseño.”</p>
  </section>

  <!-- Catálogo de productos -->
  <section id="catalogo" class="p-6">
    <h2 class="text-3xl font-semibold text-center mb-6">Catálogo de Productos</h2>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6" id="catalogo-grid"></div>
  </section>

  <!-- Resto del contenido igual -->
  <section id="compras" class="p-6 bg-white">
    <h2 class="text-2xl font-bold text-center mb-4">Compras Realizadas</h2>
    <p class="text-center">Esta sección estará disponible próximamente con seguimiento de pedidos.</p>
  </section>

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

  <section id="contacto" class="p-6">
    <h2 class="text-2xl font-bold text-center mb-4">Contacto</h2>
    <form class="max-w-xl mx-auto space-y-4">
      <input type="text" placeholder="Nombre" class="w-full border p-2 rounded">
      <input type="email" placeholder="Correo" class="w-full border p-2 rounded">
      <textarea placeholder="Mensaje" class="w-full border p-2 rounded"></textarea>
      <button class="bg-red-700 text-white px-4 py-2 rounded">Enviar</button>
    </form>
  </section>

  <section id="pagos" class="p-6 bg-white">
    <h2 class="text-2xl font-bold text-center mb-4">Pagos en Línea</h2>
    <p class="text-center mb-2">Realiza tu pago a través de nuestros métodos autorizados.</p>
    <div class="flex flex-wrap justify-center gap-4">
      <a href="#" class="bg-green-600 text-white px-4 py-2 rounded">Nequi</a>
      <a href="#" class="bg-yellow-500 text-white px-4 py-2 rounded">Bancolombia</a>
      <a href="#" class="bg-blue-700 text-white px-4 py-2 rounded">Daviplata</a>
    </div>
  </section>

  <!-- WhatsApp flotante -->
  <a href="https://wa.link/ru46tm" target="_blank" class="fixed bottom-6 right-6 bg-green-500 text-white p-4 rounded-full shadow-lg parpadea text-xl font-bold">
    📩 Escríbenos por WhatsApp
  </a>

  <!-- Scripts -->
  <script>
    // Firebase Config (REEMPLAZA ESTOS DATOS)
    const firebaseConfig = {
      apiKey: "TU_API_KEY",
      authDomain: "TU_AUTH_DOMAIN",
      projectId: "TU_PROJECT_ID",
      storageBucket: "TU_STORAGE_BUCKET",
      messagingSenderId: "TU_MESSAGING_SENDER_ID",
      appId: "TU_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    const loginBtn = document.getElementById("loginBtn");
    const userCircle = document.getElementById("userCircle");

    loginBtn.addEventListener("click", () => {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then(result => {
          const correo = result.user.email;
          localStorage.setItem("usuarioLogueado", correo);
          loginBtn.classList.add("hidden");
          userCircle.textContent = correo.charAt(0).toUpperCase();
          userCircle.classList.remove("hidden");
        })
        .catch(error => {
          alert("Error al iniciar sesión.");
          console.error(error);
        });
    });

    document.addEventListener("DOMContentLoaded", () => {
      const correo = localStorage.getItem("usuarioLogueado");
      if (correo) {
        loginBtn.classList.add("hidden");
        userCircle.textContent = correo.charAt(0).toUpperCase();
        userCircle.classList.remove("hidden");
      }

      // Generar catálogo
      const carrito = JSON.parse(localStorage.getItem('carrito')) || [];
      const catalogo = document.getElementById("catalogo-grid");

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
</body>
</html>
