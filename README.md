<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tipos de Almacenamiento de Datos</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-800">
  <header class="bg-blue-600 text-white p-6 shadow-md">
    <h1 class="text-2xl font-bold text-center">Tipos de Almacenamiento de Datos</h1>
  </header>

  <main class="max-w-6xl mx-auto p-6 space-y-12">
    <!-- Sección Filtro -->
    <section>
      <h2 class="text-xl font-semibold mb-4">🔍 Filtro</h2>
      <input 
        type="text" 
        placeholder="Busca un tipo de almacenamiento..." 
        class="w-full p-2 border rounded-md shadow-sm"
        oninput="filtrarTarjetas(this.value)" 
      />
    </section>

    <!-- Sección Tarjetas -->
    <section>
      <h2 class="text-xl font-semibold mb-4">📦 Tipos de almacenamiento</h2>
      <div id="tarjetas" class="grid md:grid-cols-3 gap-6"></div>
    </section>

    <!-- Sección Tabla Comparativa -->
    <section>
      <h2 class="text-xl font-semibold mb-4">📊 Tabla Comparativa</h2>
      <div class="overflow-x-auto">
        <table id="tabla" class="w-full border border-gray-300 text-sm"></table>
      </div>
    </section>

    <!-- Sección Glosario -->
    <section>
      <h2 class="text-xl font-semibold mb-4">📖 Glosario</h2>
      <dl id="glosario" class="space-y-4"></dl>
    </section>

    <!-- Sección Quiz -->
    <section>
      <h2 class="text-xl font-semibold mb-4">📝 Quiz</h2>
      <div id="quiz" class="space-y-6"></div>
    </section>
  </main>

  <footer class="bg-gray-800 text-white text-center p-4 mt-12">
    <p>© 2025 Web Educativa sobre Almacenamiento</p>
  </footer>

  <script>
    // =======================
    // Datos
    // =======================
    const items = [
      {
        tipo: "RAM",
        uso: "Memoria volátil de acceso muy rápido",
        ejemplos: "DDR4, DDR5",
        latencia: "Nanosegundos",
        rendimiento: "Muy alto",
        durabilidad: "Volátil (se borra al apagar)",
        escalabilidad: "Limitada por hardware",
        coste: "Alto",
        desc: "Se usa como memoria principal en servidores y PCs."
      },
      {
        tipo: "SSD NVMe",
        uso: "Almacenamiento de alta velocidad",
        ejemplos: "Samsung 980 Pro, WD Black SN850",
        latencia: "Decenas de microsegundos",
        rendimiento: "Muy alto (IOPS elevados)",
        durabilidad: "Media (limitada por ciclos de escritura)",
        escalabilidad: "Limitada al dispositivo",
        coste: "Alto por GB",
        desc: "Unidad de estado sólido conectada por PCIe, mucho más rápida que un HDD."
      },
      {
        tipo: "HDD",
        uso: "Almacenamiento masivo económico",
        ejemplos: "Seagate BarraCuda, WD Blue",
        latencia: "Milisegundos",
        rendimiento: "Medio",
        durabilidad: "Alta (pero susceptible a fallos mecánicos)",
        escalabilidad: "Fácil de ampliar añadiendo discos",
        coste: "Bajo",
        desc: "Basado en platos magnéticos; ideal para grandes volúmenes."
      },
      {
        tipo: "NAS",
        uso: "Almacenamiento compartido en red local",
        ejemplos: "Synology, QNAP",
        latencia: "Baja/Media (depende de red)",
        rendimiento: "Medio/Alto",
        durabilidad: "Alta con RAID",
        escalabilidad: "Ampliable añadiendo discos o nodos",
        coste: "Medio/Alto",
        desc: "Servidor de almacenamiento accesible por múltiples usuarios en red."
      },
      {
        tipo: "SAN",
        uso: "Red de almacenamiento para empresas",
        ejemplos: "Dell EMC PowerMax, HPE 3PAR",
        latencia: "Muy baja",
        rendimiento: "Muy alto",
        durabilidad: "Alta",
        escalabilidad: "Muy alta (nivel corporativo)",
        coste: "Muy alto",
        desc: "Infraestructura dedicada que conecta múltiples dispositivos de almacenamiento."
      },
      {
        tipo: "Objeto en nube",
        uso: "Almacenamiento de objetos en cloud",
        ejemplos: "Amazon S3, Azure Blob",
        latencia: "Decenas a cientos de ms",
        rendimiento: "Variable",
        durabilidad: "Muy alta (99.999999999%)",
        escalabilidad: "Prácticamente ilimitada",
        coste: "Pago por uso",
        desc: "Modelo orientado a objetos, ideal para backups, multimedia y Big Data."
      },
      {
        tipo: "Bloque en nube",
        uso: "Almacenamiento de bloques remoto",
        ejemplos: "Amazon EBS, Google Persistent Disk",
        latencia: "Baja",
        rendimiento: "Alto",
        durabilidad: "Alta",
        escalabilidad: "Escalable según plan",
        coste: "Pago por capacidad reservada",
        desc: "Discos virtuales conectados a máquinas virtuales."
      },
      {
        tipo: "BD relacional",
        uso: "Bases de datos estructuradas con SQL",
        ejemplos: "MySQL, PostgreSQL, Oracle",
        latencia: "Baja",
        rendimiento: "Alto",
        durabilidad: "Alta (con replicación)",
        escalabilidad: "Vertical (limitada horizontalmente)",
        coste: "Variable",
        desc: "Modelo clásico de tablas con consistencia ACID."
      },
      {
        tipo: "NoSQL clave-valor",
        uso: "Almacenamiento distribuido no relacional",
        ejemplos: "Redis, DynamoDB",
        latencia: "Muy baja",
        rendimiento: "Muy alto",
        durabilidad: "Alta (con replicación)",
        escalabilidad: "Horizontal masiva",
        coste: "Variable según nube",
        desc: "Almacenamiento rápido y escalable para grandes volúmenes de datos."
      },
      {
        tipo: "Cinta / Archivado",
        uso: "Almacenamiento a largo plazo y bajo coste",
        ejemplos: "LTO Ultrium",
        latencia: "Alta (segundos/minutos)",
        rendimiento: "Bajo",
        durabilidad: "Muy alta (décadas si se conserva bien)",
        escalabilidad: "Alta (añadiendo cintas)",
        coste: "Muy bajo",
        desc: "Se utiliza para copias de seguridad y archivado histórico."
      }
    ];

    const glosario = [
      { termino: "Latencia", definicion: "Tiempo que tarda un sistema en responder a una operación de E/S." },
      { termino: "IOPS", definicion: "Operaciones de Entrada/Salida por segundo, medida de rendimiento en discos." },
      { termino: "ACID", definicion: "Propiedades de bases de datos: Atomicidad, Consistencia, Aislamiento y Durabilidad." },
      { termino: "RAID", definicion: "Conjunto de técnicas para combinar discos y mejorar rendimiento o tolerancia a fallos." },
      { termino: "CAP", definicion: "Teorema que afirma que en sistemas distribuidos solo se pueden cumplir 2 de 3: Consistencia, Disponibilidad, Tolerancia a particiones." },
      { termino: "Durabilidad", definicion: "Garantía de que los datos persisten después de ser confirmados." },
      { termino: "Escalabilidad", definicion: "Capacidad de un sistema para aumentar su rendimiento al añadir más recursos." }
    ];

    const preguntas = [
      {
        pregunta: "¿Cuál tiene menor latencia?",
        opciones: ["HDD", "SSD NVMe", "Cinta"],
        correcta: "SSD NVMe"
      },
      {
        pregunta: "¿Qué modelo sigue las propiedades ACID?",
        opciones: ["BD relacional", "NoSQL clave-valor", "Objeto en nube"],
        correcta: "BD relacional"
      },
      {
        pregunta: "¿Cuál es ideal para archivado a largo plazo y bajo coste?",
        opciones: ["Cinta / Archivado", "RAM", "NAS"],
        correcta: "Cinta / Archivado"
      }
    ];

    // =======================
    // Renderizado dinámico
    // =======================
    const contTarjetas = document.getElementById("tarjetas");
    const tabla = document.getElementById("tabla");
    const contGlosario = document.getElementById("glosario");
    const contQuiz = document.getElementById("quiz");

    // Tarjetas
    function renderTarjetas() {
      contTarjetas.innerHTML = "";
      items.forEach(item => {
        const card = document.createElement("div");
        card.className = "card bg-white p-4 rounded-xl shadow-md";
        card.dataset.nombre = item.tipo;
        card.innerHTML = `
          <h3 class="font-bold text-lg mb-2">${item.tipo}</h3>
          <p class="text-sm mb-1">${item.desc}</p>
          <p class="text-xs text-gray-500">Uso: ${item.uso}</p>
        `;
        contTarjetas.appendChild(card);
      });
    }

    // Tabla comparativa
    function renderTabla() {
      tabla.innerHTML = `
        <thead class="bg-gray-200">
          <tr>
            <th class="p-2 border">Tipo</th>
            <th class="p-2 border">Latencia</th>
            <th class="p-2 border">Rendimiento</th>
            <th class="p-2 border">Durabilidad</th>
            <th class="p-2 border">Escalabilidad</th>
            <th class="p-2 border">Coste</th>
          </tr>
        </thead>
        <tbody>
          ${items.map(i => `
            <tr>
              <td class="p-2 border">${i.tipo}</td>
              <td class="p-2 border">${i.latencia}</td>
              <td class="p-2 border">${i.rendimiento}</td>
              <td class="p-2 border">${i.durabilidad}</td>
              <td class="p-2 border">${i.escalabilidad}</td>
              <td class="p-2 border">${i.coste}</td>
            </tr>
          `).join("")}
        </tbody>
      `;
    }

    // Glosario
    function renderGlosario() {
      contGlosario.innerHTML = "";
      glosario.forEach(g => {
        const div = document.createElement("div");
        div.innerHTML = `<dt class="font-bold">${g.termino}</dt>
                         <dd>${g.definicion}</dd>`;
        contGlosario.appendChild(div);
      });
    }

    // Quiz
    function renderQuiz() {
      contQuiz.innerHTML = "";
      preguntas.forEach((q, idx) => {
        const div = document.createElement("div");
        div.className = "p-4 bg-white shadow rounded-md";
        div.innerHTML = `
          <p class="font-semibold mb-2">${idx+1}. ${q.pregunta}</p>
          <div class="space-x-2">
            ${q.opciones.map(op => `
              <button onclick="verificar(${idx}, '${op}')" 
                      class="px-3 py-1 bg-gray-200 rounded-md">${op}</button>
            `).join("")}
          </div>
          <p id="resultado-${idx}" class="mt-2 font-semibold"></p>
        `;
        contQuiz.appendChild(div);
      });
    }

    // =======================
    // Funciones interactivas
    // =======================
    function filtrarTarjetas(texto) {
      const tarjetas = document.querySelectorAll("#tarjetas .card");
      tarjetas.forEach(card => {
        const nombre = card.dataset.nombre.toLowerCase();
        card.style.display = nombre.includes(texto.toLowerCase()) ? "block" : "none";
      });
    }

    function verificar(idx, opcion) {
      const resultado = document.getElementById(`resultado-${idx}`);
      if (opcion === preguntas[idx].correcta) {
        resultado.textContent = "✅ Correcto";
        resultado.className = "text-green-600 font-bold mt-2";
      } else {
        resultado.textContent = "❌ Incorrecto";
        resultado.className = "text-red-600 font-bold mt-2";
      }
    }

    // =======================
    // Inicialización
    // =======================
    renderTarjetas();
    renderTabla();
    renderGlosario();
    renderQuiz();
  </script>
</body>
</html>

