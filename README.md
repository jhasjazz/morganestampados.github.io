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

    button, a, img {
      transition: transform 0.2s ease-in-out;
    }
    button:hover, a:hover, img:hover {
      transform: scale(1.15);
    }
  </style>
</head>
<body class="text-gray-900 relative">
  <!-- Header -->
  <header class="bg-red-700 text-white p-4 shadow-md flex justify-between items-center">
    <h1 class="text-2xl font-bold">Morgan Estampados</h1>
    <nav class="space-x-4 flex items-center relative">
      <a href="#catalogo" class="hover:underline">Inicio</a>
      <a href="#personaliza.html" class="hover:underline">Personaliza</a>
      <a href="#contacto.html" class="hover:underline">Contacto</a>
      <a href="#pagos.html" class="hover:underline">Pagos</a>
      <a href="carro.html" class="hover:underline">Carrito</a>
      <a href="#compras.html" class="hover:underline">Mis Compras</a>
      <button id="loginBtn" class="bg-white text-red-700 px-2 py-1 rounded">Login Google</button>
      <div id="userDropdown" class="relative hidden">
        <div id="userCircle" class="w-8 h-8 rounded-full bg-white text-red-700 font-bold flex items-center justify-center cursor-pointer"></div>
        <div id="userMenu" class="absolute right-0 mt-2 w-40 bg-white text-red-700 rounded shadow-lg hidden z-50">
          <a href="usuario.html" class="block px-4 py-2 hover:bg-gray-100">Editar usuario</a>
          <button id="logoutBtn" class="block w-full text-left px-4 py-2 hover:bg-gray-100">Cerrar sesión</button>
        </div>
      </div>
    </nav>
  </header>

  <section class="bg-black text-white text-center p-2 text-lg italic">
    <p>
      “¡Arrrr! Que tu estilo navegue con nosotros, marinero del diseño.”
    </p>
  </section>

  <section id="catalogo" class="p-6">
    <h2 class="text-3xl font-semibold text-center mb-6">Catálogo de Productos</h2>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6" id="catalogo-grid"></div>
    <div class="flex justify-center gap-4 mt-6">
      <button id="prevPage" class="bg-red-700 text-white px-4 py-2 rounded">Anterior</button>
      <button id="nextPage" class="bg-red-700 text-white px-4 py-2 rounded">Siguiente</button>
    </div>
  </section>

  <a href="https://wa.link/ru46tm" target="_blank" class="fixed bottom-6 right-6 bg-green-500 text-white p-4 rounded-full shadow-lg parpadea text-xl font-bold">
    📩 Escríbenos por WhatsApp
  </a>

  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyBCwRVaG0-WUaV2SchY00LlpX_VzGCvj8o",
      authDomain: "morganestampadoslogin.firebaseapp.com",
      projectId: "morganestampadoslogin",
      storageBucket: "morganestampadoslogin.firebasestorage.app",
      messagingSenderId: "807816306056",
      appId: "1:807816306056:web:ac494752760b365e15ae3d",
      measurementId: "G-WFSFQLM81S"
    };
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    const loginBtn = document.getElementById("loginBtn");
    const userCircle = document.getElementById("userCircle");
    const userDropdown = document.getElementById("userDropdown");
    const userMenu = document.getElementById("userMenu");
    const logoutBtn = document.getElementById("logoutBtn");

    loginBtn.addEventListener("click", () => {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then(result => {
          const correo = result.user.email;
          localStorage.setItem("usuarioLogueado", correo);
          mostrarUsuario(correo);
        })
        .catch(err => {
          alert("Error al iniciar sesión");
          console.error(err);
        });
    });

    function mostrarUsuario(correo) {
      loginBtn.classList.add("hidden");
      userDropdown.classList.remove("hidden");
      userCircle.textContent = correo.charAt(0).toUpperCase();
    }

    document.addEventListener("DOMContentLoaded", () => {
      const correo = localStorage.getItem("usuarioLogueado");
      if (correo) mostrarUsuario(correo);

      let currentPage = 1;
      const productosPorPagina = 20;
      const totalProductos = 60;

      const catalogo = document.getElementById("catalogo-grid");
      let carrito = JSON.parse(localStorage.getItem("carrito")) || [];

      function guardarCarrito() {
        localStorage.setItem("carrito", JSON.stringify(carrito));
      }

      function obtenerCantidad(nombre) {
        const item = carrito.find(item => item.nombre === nombre);
        return item ? item.cantidad : 0;
      }

      window.agregarAlCarrito = function(nombre, precio, index) {
        const itemExistente = carrito.find(item => item.nombre === nombre);
        if (itemExistente) {
          itemExistente.cantidad++;
        } else {
          carrito.push({ nombre, precio, cantidad: 1 });
        }
        guardarCarrito();
        const input = document.getElementById(`contador-${index}`);
        if (input) input.value = obtenerCantidad(nombre);
      }

      window.actualizarCantidadDesdeInput = function(nombre, valor, index) {
        const cantidad = parseInt(valor);
        const item = carrito.find(item => item.nombre === nombre);
        if (item) {
          item.cantidad = cantidad > 0 ? cantidad : 1;
        } else {
          carrito.push({ nombre, precio: 0, cantidad: cantidad });
        }
        guardarCarrito();
      }

      function renderCatalogo(page) {
        catalogo.innerHTML = "";
        const inicio = (page - 1) * productosPorPagina + 1;
        const fin = Math.min(inicio + productosPorPagina - 1, totalProductos);
        for (let i = inicio; i <= fin; i++) {
          const nombre = i === 1 ? "Camiseta Pirata" : `Producto ${i}`;
          const precio = i === 1 ? 35000 : 20000 + i * 500;
          const cantidadActual = obtenerCantidad(nombre);
          const div = document.createElement('div');
          div.className = "bg-white p-4 rounded shadow flex flex-col items-center";
          div.innerHTML = `
            <img src="p${i}.jpeg" alt="${nombre}" class="w-full mb-2 rounded">
            <h3 class="font-bold text-center">${nombre}</h3>
            <p>$${precio}</p>
            <div class="flex flex-col mt-2 w-full items-center">
              <a href="p${i}.html" class="bg-purple-500 text-white px-4 py-2 rounded text-center mb-2 w-full">Ver detalles</a>
              <div class="flex items-center gap-2 w-full">
                <button onclick="agregarAlCarrito('${nombre}', ${precio}, ${i})" class="bg-pink-500 text-white px-4 py-2 rounded w-2/3 text-sm">Añadir al carro ❤️</button>
                <input type="number" min="1" value="${cantidadActual}" id="contador-${i}" class="w-16 border rounded px-2 py-1 text-center" onchange="actualizarCantidadDesdeInput('${nombre}', this.value, ${i})">
              </div>
            </div>`;
          catalogo.appendChild(div);
        }
      }

      document.getElementById("prevPage").addEventListener("click", () => {
        if (currentPage > 1) {
          currentPage--;
          renderCatalogo(currentPage);
        }
      });

      document.getElementById("nextPage").addEventListener("click", () => {
        const maxPage = Math.ceil(totalProductos / productosPorPagina);
        if (currentPage < maxPage) {
          currentPage++;
          renderCatalogo(currentPage);
        }
      });

      renderCatalogo(currentPage);
    });

    userCircle.addEventListener("click", () => {
      userMenu.classList.toggle("hidden");
    });

    logoutBtn.addEventListener("click", () => {
      auth.signOut().then(() => {
        localStorage.clear();
        userDropdown.classList.add("hidden");
        userMenu.classList.add("hidden");
        loginBtn.classList.remove("hidden");
        location.reload();
      });
    });
  </script>
</body>
</html>
