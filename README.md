<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Datos del Usuario - Morgan Estampados</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>
</head>
<body class="min-h-screen bg-cover bg-no-repeat bg-center text-gray-900" style="background-image: url('fondopagina2.png');">
  <header class="bg-red-700 text-white p-4 shadow-md flex justify-between items-center">
    <h1 class="text-2xl font-bold">Morgan Estampados</h1>
    <nav class="space-x-4 flex items-center relative">
      <a href="index.html" class="hover:underline">Inicio</a>
      <a href="personaliza.html" class="hover:underline">Personaliza</a>
      <a href="contacto.html" class="hover:underline">Contacto</a>
      <a href="pagos.html" class="hover:underline">Pagos</a>
      <a href="carro.html" class="hover:underline">Carrito</a>
      <button id="loginBtn" class="bg-white text-red-700 px-2 py-1 rounded">Login Google</button>
      <div id="userDropdown" class="relative hidden">
        <div id="userCircle" class="w-8 h-8 rounded-full bg-white text-red-700 font-bold flex items-center justify-center cursor-pointer"></div>
        <div id="userMenu" class="absolute right-0 mt-2 w-40 bg-white text-red-700 rounded shadow-lg hidden z-50">
          <button id="logoutBtn" class="block w-full text-left px-4 py-2 hover:bg-gray-100">Cerrar sesión</button>
        </div>
      </div>
    </nav>
  </header>

  <section class="max-w-4xl mx-auto mt-12 bg-white border-4 border-red-600 rounded-2xl shadow-2xl p-10">
    <h2 class="text-4xl font-extrabold text-center text-red-700 mb-6">👤 Datos del Usuario</h2>
    <form id="form-usuario" class="space-y-6">
      <div class="grid md:grid-cols-2 gap-6">
        <div>
          <label class="block text-red-700 font-bold mb-2">Nombre completo</label>
          <input type="text" id="nombre" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
        <div>
          <label class="block text-red-700 font-bold mb-2">Cédula</label>
          <input type="text" id="cedula" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
      </div>
      <div class="grid md:grid-cols-2 gap-6">
        <div>
          <label class="block text-red-700 font-bold mb-2">Celular</label>
          <input type="tel" id="celular" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
        <div>
          <label class="block text-red-700 font-bold mb-2">País</label>
          <input type="text" id="pais" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
      </div>
      <div class="grid md:grid-cols-2 gap-6">
        <div>
          <label class="block text-red-700 font-bold mb-2">Departamento</label>
          <input type="text" id="departamento" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
        <div>
          <label class="block text-red-700 font-bold mb-2">Dirección completa</label>
          <input type="text" id="direccion" required class="w-full border-2 border-red-300 rounded-xl px-4 py-3" />
        </div>
      </div>
      <div class="flex items-center gap-3 mt-4">
        <input type="checkbox" id="predeterminada" class="h-5 w-5 text-red-700 border-gray-300 rounded" />
        <label for="predeterminada" class="text-red-700 font-medium">Usar como dirección predeterminada</label>
      </div>
      <div class="flex justify-center mt-8 gap-4">
        <button type="submit" class="bg-red-700 text-white px-8 py-3 rounded-full font-bold hover:scale-105 transition-transform">📂 Guardar dirección</button>
      </div>
    </form>
    <div id="estado-usuario" class="text-center mt-6 text-green-700 font-semibold hidden">¡Datos guardados correctamente!</div>
  </section>

  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyBCwRVaG0-WUaV2SchY00LlpX_VzGCvj8o",
      authDomain: "morganestampadoslogin.firebaseapp.com",
      projectId: "morganestampadoslogin",
      storageBucket: "morganestampadoslogin.appspot.com",
      messagingSenderId: "807816306056",
      appId: "1:807816306056:web:ac494752760b365e15ae3d"
    };
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.firestore();

    auth.onAuthStateChanged(user => {
      if (!user) {
        alert("Inicia sesión para editar tus datos.");
        return;
      }

      document.getElementById("form-usuario").addEventListener("submit", async (e) => {
        e.preventDefault();
        const data = {
          nombre: document.getElementById("nombre").value,
          cedula: document.getElementById("cedula").value,
          celular: document.getElementById("celular").value,
          pais: document.getElementById("pais").value,
          departamento: document.getElementById("departamento").value,
          direccion: document.getElementById("direccion").value,
          predeterminada: document.getElementById("predeterminada").checked,
          timestamp: new Date()
        };

        await db.collection("usuarios").doc(user.email).collection("direcciones").add(data);
        document.getElementById("estado-usuario").classList.remove("hidden");
      });
    });
  </script>
</body>
</html>
