<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Morgan Estampados</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
</head>
<body class="bg-gray-100 text-gray-900">
  <!-- Barra de navegación -->
  <header class="bg-red-700 text-white p-4 shadow-md flex justify-between items-center">
    <h1 class="text-2xl font-bold">Morgan Estampados</h1>
    <nav class="space-x-4">
      <a href="#catalogo" class="hover:underline">Inicio</a>
      <a href="#personaliza" class="hover:underline">Personaliza</a>
      <a href="#contacto" class="hover:underline">Contacto</a>
      <a href="#pagos" class="hover:underline">Pagos</a>
      <button id="loginBtn" class="bg-white text-red-700 px-2 py-1 rounded">Login Google</button>
    </nav>
  </header>

  <!-- Catálogo de productos -->
  <section id="catalogo" class="p-6">
    <h2 class="text-3xl font-semibold text-center mb-6">Catálogo de Productos</h2>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div class="bg-white p-4 rounded shadow">
        <img src="https://via.placeholder.com/300x300?text=Camiseta" alt="Camiseta" class="w-full mb-2">
        <h3 class="font-bold">Camiseta Pirata</h3>
        <p>$35.000</p>
        <a href="https://wa.link/ru46tm" target="_blank" class="mt-2 inline-block bg-red-700 text-white px-4 py-1 rounded">Pedir</a>
      </div>
      <div class="bg-white p-4 rounded shadow">
        <img src="https://via.placeholder.com/300x300?text=Tote+Bag" alt="Tote Bag" class="w-full mb-2">
        <h3 class="font-bold">Tote Bag Morgan</h3>
        <p>$25.000</p>
        <a href="https://wa.link/ru46tm" target="_blank" class="mt-2 inline-block bg-red-700 text-white px-4 py-1 rounded">Pedir</a>
      </div>
    </div>
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
    <div class="flex justify-center space-x-4">
      <a href="#" class="bg-green-600 text-white px-4 py-2 rounded">Nequi</a>
      <a href="#" class="bg-yellow-500 text-white px-4 py-2 rounded">Bancolombia</a>
    </div>
  </section>

  <!-- WhatsApp flotante -->
  <a href="https://wa.link/ru46tm" target="_blank" class="fixed bottom-6 right-6 bg-green-500 text-white p-3 rounded-full shadow-lg">
    🟢 WhatsApp
  </a>

  <!-- Script Firebase Login Google (configuración pendiente) -->
  <script>
    const loginBtn = document.getElementById("loginBtn");
    loginBtn.addEventListener("click", () => alert("Login con Google próximamente disponible"));
  </script>
</body>
</html>

