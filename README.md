<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simulacro (60 preguntas) - Diseño Mecánico</title>
  <style>
    body { font-family: system-ui, Arial, sans-serif; line-height: 1.35; margin: 20px; }
    h1 { margin-bottom: 6px; }
    .sub { margin-top: 0; color: #444; }
    .q { border: 1px solid #ddd; padding: 12px; border-radius: 10px; margin: 10px 0; }
    .q h3 { margin: 0 0 8px 0; font-size: 16px; }
    .opts label { display:block; margin: 6px 0; cursor: pointer; }
    .btns { margin: 16px 0; display: flex; gap: 10px; flex-wrap: wrap; }
    button { padding: 10px 14px; border-radius: 10px; border: 1px solid #333; background: #fff; cursor: pointer; }
    button:hover { opacity: 0.9; }
    #result { margin-top: 18px; padding: 12px; border-radius: 10px; border: 1px solid #ddd; }
    .ok { color: #0a7; font-weight: 700; }
    .bad { color: #c20; font-weight: 700; }
    .muted { color: #555; }
    .corr-item { border-top: 1px dashed #ddd; padding-top: 10px; margin-top: 10px; }
    .pill { display:inline-block; padding: 2px 10px; border-radius: 999px; border: 1px solid #ddd; font-size: 12px; margin-left: 6px; }
    .topnote { background: #f7f7f7; border: 1px solid #e6e6e6; padding: 10px 12px; border-radius: 10px; }
  </style>
</head>
<body>
  <h1>Simulacro Teórico (60 preguntas)</h1>
  <p class="sub">Selecciona una alternativa por pregunta. Al final: puntaje y correcciones.</p>

  <div class="topnote">
    <strong>Consejo:</strong> responde primero todo y luego recién calcula. Si dejas una pregunta en blanco, contará como incorrecta.
  </div>

  <form id="quiz"></form>

  <div class="btns">
    <button type="button" id="gradeBtn">Calcular puntaje</button>
    <button type="button" id="resetBtn">Reiniciar</button>
  </div>

  <div id="result" aria-live="polite"></div>

<script>
  // 60 preguntas basadas en tu repaso (1–60).
  // Cada pregunta tiene 4 alternativas (A-D) y una explicación breve para la corrección.
  const questions = [
    { q: "1) ¿Qué es el diseño en ingeniería mecánica?",
      opts: ["Proceso para crear sistemas o componentes que cumplan una función segura y eficiente.",
             "Proceso de fabricación en serie sin planificación.",
             "Cálculo exclusivo de costos sin considerar seguridad.",
             "Selección de colores y estética del producto."],
      a: 0, exp: "El diseño se define como un proceso para crear sistemas/componentes que cumplan su función con seguridad y eficiencia." },

    { q: "2) ¿Qué es una necesidad en diseño?",
      opts: ["Problema o requerimiento que motiva el diseño.",
             "Lista final de planos y tolerancias.",
             "Una restricción de material.",
             "Una norma técnica obligatoria."],
      a: 0, exp: "La necesidad es el punto de partida: el problema/requerimiento que origina el diseño." },

    { q: "3) ¿Qué es una especificación?",
      opts: ["Conjunto de requisitos que debe cumplir el diseño.",
             "Una limitación que reduce opciones.",
             "Un factor obtenido luego del diseño final.",
             "Una falla por carga aplicada una sola vez."],
      a: 0, exp: "La especificación es el conjunto de requisitos que el diseño debe satisfacer." },

    { q: "4) ¿Qué es una restricción?",
      opts: ["Limitación que reduce las opciones de diseño.",
             "El máximo esfuerzo antes de fracturarse.",
             "Una carga que genera esfuerzo cortante.",
             "Una herramienta para hallar esfuerzos principales."],
      a: 0, exp: "La restricción limita el espacio de soluciones (costo, espacio, proceso, material, etc.)." },

    { q: "5) ¿Qué es un código de diseño?",
      opts: ["Conjunto de reglas técnicas para garantizar seguridad y funcionamiento.",
             "Documento de marketing del producto.",
             "Cálculo de velocidad crítica en ejes.",
             "Curva esfuerzo–número de ciclos."],
      a: 0, exp: "Un código de diseño reúne reglas técnicas para asegurar seguridad y funcionamiento." },

    { q: "6) ¿Qué es una norma técnica?",
      opts: ["Especificación estandarizada para materiales, dimensiones o procesos.",
             "Un criterio de falla para materiales dúctiles.",
             "La oposición al estiramiento de un tornillo.",
             "El esfuerzo máximo para vida infinita."],
      a: 0, exp: "Una norma técnica estandariza requisitos: materiales, dimensiones, procesos, etc." },

    { q: "7) ¿Qué es el factor de diseño?",
      opts: ["Relación entre resistencia y esfuerzo permisible.",
             "Probabilidad de que un elemento funcione sin fallar.",
             "Máxima carga sin deformación permanente.",
             "Número de ciclos hasta la falla."],
      a: 0, exp: "El factor de diseño compara resistencia vs esfuerzo permisible para diseñar con margen." },

    { q: "8) ¿Qué es el factor de seguridad?",
      opts: ["Factor real obtenido después del diseño final.",
             "Relación entre fuerza y deflexión.",
             "Método gráfico para analizar esfuerzos.",
             "Inestabilidad por compresión."],
      a: 0, exp: "El factor de seguridad se interpreta como el margen real logrado tras dimensionar." },

    { q: "9) ¿Qué es confiabilidad?",
      opts: ["Probabilidad de que un elemento funcione sin fallar.",
             "Separación total del material.",
             "Máximo esfuerzo sin deformación permanente.",
             "Aumento local del esfuerzo por discontinuidades."],
      a: 0, exp: "Confiabilidad = probabilidad de cumplir función sin falla en un periodo/condición." },

    { q: "10) ¿Qué es responsabilidad legal del producto?",
      opts: ["Obligación del fabricante ante daños causados por fallas.",
             "Medición del módulo de elasticidad.",
             "Ensayo para obtener la curva S–N.",
             "Velocidad donde ocurre resonancia."],
      a: 0, exp: "Responsabilidad legal: obligación del fabricante por daños asociados a fallas del producto." },

    { q: "11) ¿Qué es el módulo de elasticidad (E)?",
      opts: ["Relación entre esfuerzo normal y deformación elástica.",
             "Fin de la relación lineal esfuerzo–deformación.",
             "Tendencia a fracturarse sin deformación plástica.",
             "Máximo esfuerzo soportado antes de fracturarse."],
      a: 0, exp: "E vincula esfuerzo normal con deformación elástica (zona lineal)." },

    { q: "12) ¿Qué es el límite proporcional?",
      opts: ["Fin de la relación lineal esfuerzo–deformación.",
             "Inicio de deformación permanente.",
             "Separación total del material.",
             "Relación esfuerzo–número de ciclos."],
      a: 0, exp: "El límite proporcional marca el fin de la linealidad (ley de Hooke estricta)." },

    { q: "13) ¿Qué es el límite elástico?",
      opts: ["Máximo esfuerzo sin deformación permanente.",
             "Esfuerzo donde inicia la deformación plástica.",
             "Máximo esfuerzo antes de fracturarse.",
             "Inestabilidad por compresión."],
      a: 0, exp: "El límite elástico es el máximo esfuerzo sin dejar deformación permanente al descargar." },

    { q: "14) ¿Qué es la región plástica?",
      opts: ["Región donde el material no recupera su forma original.",
             "Región donde todo vuelve exactamente a la forma inicial.",
             "Zona donde no existe esfuerzo cortante.",
             "Zona donde solo hay esfuerzos principales."],
      a: 0, exp: "En región plástica queda deformación permanente (no hay recuperación total)." },

    { q: "15) ¿Qué es el esfuerzo de fluencia?",
      opts: ["Esfuerzo donde inicia la deformación plástica.",
             "Fin de la relación lineal esfuerzo–deformación.",
             "Probabilidad de funcionar sin fallar.",
             "Aumento local del esfuerzo por discontinuidades."],
      a: 0, exp: "La fluencia inicia cuando el material entra a deformación plástica." },

    { q: "16) ¿Qué es la resistencia última?",
      opts: ["Máximo esfuerzo soportado antes de fracturarse.",
             "Máximo esfuerzo sin deformación permanente.",
             "Esfuerzo máximo para vida infinita.",
             "Oposición a la compresión."],
      a: 0, exp: "Resistencia última: el máximo esfuerzo del material antes de fractura." },

    { q: "17) ¿Qué es ductilidad?",
      opts: ["Capacidad de deformarse plásticamente antes de fallar.",
             "Tendencia a fracturarse sin deformación.",
             "Método gráfico para esfuerzos.",
             "Relación entre fuerza y deflexión."],
      a: 0, exp: "Ductilidad: admite deformación plástica significativa antes de romper." },

    { q: "18) ¿Qué es fragilidad?",
      opts: ["Tendencia a fracturarse sin deformación plástica.",
             "Capacidad de deformarse plásticamente antes de fallar.",
             "Carga que genera esfuerzo cortante.",
             "Diseño con alto factor de seguridad."],
      a: 0, exp: "Fragilidad: rompe con poca o nula deformación plástica previa." },

    { q: "19) ¿Qué es trabajo en frío?",
      opts: ["Deformación plástica por debajo de la recristalización.",
             "Tratamiento térmico por encima de la recristalización.",
             "Ensayo para medir velocidad crítica.",
             "Método para sumar efectos de cargas."],
      a: 0, exp: "Trabajo en frío: deformación plástica a temperaturas por debajo de recristalización." },

    { q: "20) ¿Qué efecto tiene el trabajo en frío?",
      opts: ["Aumenta resistencia y reduce ductilidad.",
             "Reduce resistencia y aumenta ductilidad.",
             "Elimina el límite de fatiga.",
             "Hace que no existan concentraciones de esfuerzo."],
      a: 0, exp: "El trabajo en frío endurece: sube resistencia y baja ductilidad." },

    { q: "21) ¿Qué es esfuerzo normal?",
      opts: ["Esfuerzo perpendicular al área.",
             "Esfuerzo paralelo al área.",
             "Esfuerzo que aparece solo en torsión.",
             "Esfuerzo solo en cargas cíclicas."],
      a: 0, exp: "Normal: actúa perpendicular a la sección (tracción/compresión/flexión)." },

    { q: "22) ¿Qué es esfuerzo cortante?",
      opts: ["Esfuerzo paralelo al área.",
             "Esfuerzo perpendicular al área.",
             "Máximo esfuerzo sin deformación permanente.",
             "Separación total del material."],
      a: 0, exp: "Cortante: actúa paralelo a la sección (ej. torsión, corte directo)." },

    { q: "23) ¿Qué es esfuerzo axial?",
      opts: ["Esfuerzo normal debido a tracción o compresión.",
             "Esfuerzo cortante debido a torsión.",
             "Aumento local de esfuerzo por ranuras.",
             "Método para hallar esfuerzos principales."],
      a: 0, exp: "Axial: esfuerzo normal provocado por fuerza de tracción/compresión en el eje del elemento." },

    { q: "24) ¿Qué es flexión?",
      opts: ["Carga que genera esfuerzo normal variable.",
             "Carga que genera esfuerzo cortante uniforme.",
             "Carga que solo aparece en uniones atornilladas.",
             "Falla por carga aplicada una sola vez."],
      a: 0, exp: "Flexión: produce esfuerzos normales que varían a través de la sección." },

    { q: "25) ¿Qué es torsión?",
      opts: ["Carga que genera esfuerzo cortante.",
             "Carga que genera solo esfuerzo normal.",
             "Método energético para deflexiones.",
             "Inestabilidad por compresión."],
      a: 0, exp: "Torsión: momento torsor que induce esfuerzos cortantes (típico en ejes)." },

    { q: "26) ¿Qué es un esfuerzo principal?",
      opts: ["Esfuerzo normal máximo o mínimo sin cortante.",
             "Esfuerzo cortante máximo con normal cero.",
             "Esfuerzo equivalente para fatiga.",
             "Esfuerzo máximo para vida infinita."],
      a: 0, exp: "Principal: esfuerzos normales extremos en planos donde el cortante es cero." },

    { q: "27) ¿Qué es el círculo de Mohr?",
      opts: ["Método gráfico para analizar estados de esfuerzo.",
             "Ensayo para hallar resistencia última.",
             "Método para calcular velocidad crítica.",
             "Norma para tornillos."],
      a: 0, exp: "El círculo de Mohr es una herramienta gráfica para transformar esfuerzos y hallar principales/cortantes." },

    { q: "28) ¿Para qué sirve el círculo de Mohr?",
      opts: ["Encontrar esfuerzos principales y cortantes máximos.",
             "Eliminar concentraciones de esfuerzo.",
             "Determinar corrosión del material.",
             "Calcular el límite proporcional."],
      a: 0, exp: "Sirve para obtener esfuerzos principales y cortante máximo en un punto." },

    { q: "29) ¿Qué es concentración de esfuerzos?",
      opts: ["Aumento local del esfuerzo por discontinuidades.",
             "Disminución local del esfuerzo por uniformidad.",
             "Falla por cargas cíclicas.",
             "Relación entre fuerza y deformación."],
      a: 0, exp: "Discontinuidades geométricas elevan el esfuerzo local respecto al nominal." },

    { q: "30) ¿Dónde suelen aparecer concentraciones?",
      opts: ["Orificios, ranuras, cambios bruscos de sección.",
             "Solo en materiales plásticos.",
             "Solo en columnas esbeltas.",
             "Solo en curvas S–N."],
      a: 0, exp: "Aparecen en geometrías con cambios abruptos: agujeros, ranuras, escalones, etc." },

    { q: "31) ¿Qué es deflexión?",
      opts: ["Deformación producida por una carga.",
             "Separación total del material.",
             "Probabilidad de no fallar.",
             "Carga que genera esfuerzo cortante."],
      a: 0, exp: "Deflexión: desplazamiento/deformación del elemento por carga." },

    { q: "32) ¿Qué es rigidez?",
      opts: ["Resistencia a la deformación.",
             "Tendencia a fracturarse sin deformación plástica.",
             "Relación esfuerzo–número de ciclos.",
             "Margen entre resistencia y esfuerzo."],
      a: 0, exp: "Rigidez: qué tanto se opone el elemento a deformarse." },

    { q: "33) ¿Qué ley relaciona fuerza y deformación?",
      opts: ["Ley de Hooke.",
             "Ley de Newton de gravitación.",
             "Ley de Coulomb (electricidad).",
             "Ley de Boyle (gases)."],
      a: 0, exp: "La ley de Hooke relaciona fuerza (o esfuerzo) con deformación en régimen elástico lineal." },

    { q: "34) ¿Qué es una constante de resorte?",
      opts: ["Relación entre fuerza y deflexión.",
             "Relación entre esfuerzo y número de ciclos.",
             "Aumento local por discontinuidad.",
             "Inestabilidad por compresión."],
      a: 0, exp: "La constante k une fuerza con deflexión: F = k·δ (en comportamiento lineal)." },

    { q: "35) ¿Qué es superposición?",
      opts: ["Método para sumar efectos de cargas.",
             "Método para hallar esfuerzos principales.",
             "Proceso de recristalización.",
             "Tipo de fatiga de alto ciclaje."],
      a: 0, exp: "Superposición: si el sistema es lineal, los efectos se suman." },

    { q: "36) ¿Qué es el teorema de Castigliano?",
      opts: ["Método energético para calcular deflexiones.",
             "Criterio de falla para materiales frágiles.",
             "Norma internacional de estandarización.",
             "Ensayo para vida a fatiga."],
      a: 0, exp: "Castigliano: usa energía de deformación para obtener deflexiones." },

    { q: "37) ¿Por qué se controla la deflexión?",
      opts: ["Para evitar fallas funcionales.",
             "Para aumentar la corrosión.",
             "Para eliminar esfuerzos cortantes.",
             "Para reducir la ductilidad."],
      a: 0, exp: "Se controla porque aunque no rompa, puede fallar funcionalmente (alineación, contacto, etc.)." },

    { q: "38) ¿Qué elemento suele fallar por deflexión excesiva?",
      opts: ["Ejes y engranes.",
             "Solo tornillos.",
             "Solo columnas.",
             "Solo probetas de ensayo."],
      a: 0, exp: "En ejes/engranes, la deflexión causa problemas de alineamiento y contacto." },

    { q: "39) ¿Qué es pandeo?",
      opts: ["Inestabilidad por compresión.",
             "Separación total del material.",
             "Relación entre fuerza y deflexión.",
             "Falla por cargas cíclicas."],
      a: 0, exp: "Pandeo: inestabilidad (flexión lateral súbita) bajo compresión." },

    { q: "40) ¿Qué tipo de elemento sufre pandeo?",
      opts: ["Columnas esbeltas.",
             "Engranes macizos en torsión.",
             "Tornillos solo a tensión.",
             "Barras solo a cortante."],
      a: 0, exp: "Típico en columnas esbeltas: pequeñas imperfecciones disparan inestabilidad." },

    { q: "41) ¿Qué es falla estática?",
      opts: ["Falla por una carga aplicada una sola vez.",
             "Falla por cargas cíclicas repetidas.",
             "Falla por corrosión exclusivamente.",
             "Falla por resonancia a alta velocidad."],
      a: 0, exp: "Estática: se evalúa con cargas no repetitivas (una aplicación principal)." },

    { q: "42) ¿Qué es fluencia?",
      opts: ["Inicio de deformación permanente.",
             "Fin de la relación lineal esfuerzo–deformación.",
             "Número de ciclos hasta la falla.",
             "Máxima carga sin deformación permanente."],
      a: 0, exp: "Fluencia: inicio de deformación plástica permanente." },

    { q: "43) ¿Qué es fractura?",
      opts: ["Separación total del material.",
             "Deformación elástica recuperable.",
             "Suma de efectos de cargas.",
             "Resonancia en un eje."],
      a: 0, exp: "Fractura: separación completa (rotura) del material." },

    { q: "44) ¿Qué es criterio de falla?",
      opts: ["Método para predecir falla.",
             "Norma para tornillos.",
             "Relación entre esfuerzo y deformación elástica.",
             "Proceso para crear componentes."],
      a: 0, exp: "Es una regla/teoría que decide si falla comparando esfuerzos con resistencias." },

    { q: "45) ¿Qué criterio se usa en materiales dúctiles?",
      opts: ["Energía de distorsión (Von Mises).",
             "Esfuerzo normal máximo.",
             "Círculo de Mohr.",
             "Ley de Hooke."],
      a: 0, exp: "Para dúctiles se usa Von Mises (energía de distorsión) en el repaso." },

    { q: "46) ¿Qué criterio se usa en materiales frágiles?",
      opts: ["Esfuerzo normal máximo.",
             "Energía de distorsión (Von Mises).",
             "Superposición.",
             "Constante de resorte."],
      a: 0, exp: "Para frágiles se aplica esfuerzo normal máximo según el repaso." },

    { q: "47) ¿Qué es esfuerzo equivalente?",
      opts: ["Esfuerzo que se compara con la resistencia.",
             "Esfuerzo paralelo al área.",
             "Esfuerzo máximo para vida infinita.",
             "Inestabilidad por compresión."],
      a: 0, exp: "Se calcula un esfuerzo “equivalente” para compararlo con la resistencia del material." },

    { q: "48) ¿Qué es falla por sobrecarga?",
      opts: ["Falla cuando el esfuerzo supera la resistencia.",
             "Falla por cargas cíclicas.",
             "Falla por corrosión que elimina el límite de fatiga.",
             "Falla por pandeo de columnas."],
      a: 0, exp: "Sobrecarga: el esfuerzo aplicado excede la resistencia del material." },

    { q: "49) ¿Qué es factor de seguridad en falla estática?",
      opts: ["Margen entre resistencia y esfuerzo.",
             "Relación entre fuerza y deflexión.",
             "Número de ciclos hasta la falla.",
             "Método para hallar esfuerzos principales."],
      a: 0, exp: "En estática, es el margen que separa la resistencia del esfuerzo actuante." },

    { q: "50) ¿Qué es un diseño conservador?",
      opts: ["Diseño con alto factor de seguridad.",
             "Diseño con el menor diámetro posible siempre.",
             "Diseño solo para cargas cíclicas.",
             "Diseño que ignora concentraciones de esfuerzo."],
      a: 0, exp: "Conservador: usa un factor de seguridad alto para reducir riesgo." },

    { q: "51) ¿Qué es la fatiga?",
      opts: ["Falla por cargas cíclicas.",
             "Falla por una sola carga aplicada.",
             "Inestabilidad por compresión.",
             "Aumento local del esfuerzo por discontinuidades."],
      a: 0, exp: "Fatiga: falla asociada a esfuerzos repetidos/cíclicos." },

    { q: "52) ¿Puede ocurrir bajo el límite elástico?",
      opts: ["Sí.",
             "No, nunca.",
             "Solo si hay pandeo.",
             "Solo si el material es frágil."],
      a: 0, exp: "Según el repaso, la fatiga puede ocurrir incluso bajo el límite elástico." },

    { q: "53) ¿Qué es la curva S–N?",
      opts: ["Relación esfuerzo–número de ciclos.",
             "Relación fuerza–deflexión.",
             "Relación esfuerzo–deformación elástica.",
             "Relación torque–diámetro."],
      a: 0, exp: "S–N vincula amplitud de esfuerzo con cantidad de ciclos hasta falla." },

    { q: "54) ¿Qué es vida a fatiga?",
      opts: ["Número de ciclos hasta la falla.",
             "Máximo esfuerzo sin deformación permanente.",
             "Separación total del material.",
             "Oposición al estiramiento."],
      a: 0, exp: "Vida a fatiga: cuántos ciclos soporta antes de fallar." },

    { q: "55) ¿Qué es el límite de resistencia a la fatiga?",
      opts: ["Esfuerzo máximo para vida infinita.",
             "Esfuerzo donde inicia la deformación plástica.",
             "Fin de la relación lineal esfuerzo–deformación.",
             "Criterio de falla para materiales dúctiles."],
      a: 0, exp: "Límite de fatiga: esfuerzo umbral asociado a vida “infinita” (idealmente)." },

    { q: "56) ¿Dónde inicia una grieta por fatiga?",
      opts: ["En concentraciones de esfuerzo.",
             "Siempre en el centro geométrico.",
             "Solo donde hay compresión pura.",
             "En zonas sin discontinuidades."],
      a: 0, exp: "La iniciación ocurre típicamente en zonas de concentración de esfuerzo." },

    { q: "57) ¿Qué es fatiga de alto ciclaje?",
      opts: ["Más de 10³ ciclos.",
             "Menos de 10³ ciclos.",
             "Más de 10⁶ ciclos exclusivamente.",
             "Un tipo de pandeo por compresión."],
      a: 0, exp: "En el repaso: alto ciclaje > 10³ ciclos." },

    { q: "58) ¿Qué es fatiga de bajo ciclaje?",
      opts: ["Menos de 10³ ciclos.",
             "Más de 10³ ciclos.",
             "Solo si hay corrosión.",
             "Solo en tornillos."],
      a: 0, exp: "En el repaso: bajo ciclaje < 10³ ciclos." },

    { q: "59) ¿Qué efecto tiene la rugosidad superficial?",
      opts: ["Reduce la resistencia a la fatiga.",
             "Aumenta la resistencia a la fatiga.",
             "Elimina el límite proporcional.",
             "Evita concentraciones de esfuerzo."],
      a: 0, exp: "La rugosidad genera micro-entallas → reduce resistencia a fatiga." },

    { q: "60) ¿Qué efecto tiene la corrosión?",
      opts: ["Elimina el límite de fatiga.",
             "Aumenta el límite de fatiga.",
             "Convierte el material en dúctil.",
             "Evita el pandeo."],
      a: 0, exp: "La corrosión acelera daño; según el repaso, elimina el límite de fatiga." },
  ];

  const quizEl = document.getElementById("quiz");
  const resultEl = document.getElementById("result");
  const gradeBtn = document.getElementById("gradeBtn");
  const resetBtn = document.getElementById("resetBtn");

  function renderQuiz() {
    quizEl.innerHTML = "";
    questions.forEach((item, idx) => {
      const box = document.createElement("div");
      box.className = "q";
      const n = idx + 1;

      const title = document.createElement("h3");
      title.textContent = item.q;
      box.appendChild(title);

      const opts = document.createElement("div");
      opts.className = "opts";

      item.opts.forEach((opt, j) => {
        const id = `q${idx}_opt${j}`;
        const label = document.createElement("label");

        const input = document.createElement("input");
        input.type = "radio";
        input.name = `q${idx}`;
        input.value = String(j);
        input.id = id;

        label.htmlFor = id;
        label.appendChild(input);
        label.appendChild(document.createTextNode(" " + ["A) ", "B) ", "C) ", "D) "][j] + opt));

        opts.appendChild(label);
      });

      box.appendChild(opts);
      quizEl.appendChild(box);
    });

    resultEl.innerHTML = "<span class='muted'>Aún no has calculado tu puntaje.</span>";
  }

  function getUserAnswer(qIndex) {
    const chosen = document.querySelector(`input[name="q${qIndex}"]:checked`);
    return chosen ? parseInt(chosen.value, 10) : null;
  }

  function grade() {
    let score = 0;
    const corrections = [];

    questions.forEach((item, idx) => {
      const ans = getUserAnswer(idx);
      const correct = item.a;
      const isCorrect = ans === correct;

      if (isCorrect) score += 1;

      corrections.push({
        num: idx + 1,
        question: item.q,
        chosen: ans,
        correct: correct,
        isCorrect,
        exp: item.exp,
        chosenText: ans === null ? "Sin responder" : item.opts[ans],
        correctText: item.opts[correct],
      });
    });

    const pct = Math.round((score / questions.length) * 100);

    // Render results
    const wrongs = corrections.filter(x => !x.isCorrect).length;

    let html = `
      <h2>Resultado</h2>
      <p><strong>Puntaje:</strong> ${score} / ${questions.length} <span class="pill">${pct}%</span></p>
      <p><strong>Incorrectas:</strong> ${wrongs} | <strong>Correctas:</strong> ${score}</p>
      <h2>Correcciones</h2>
      <p class="muted">Abajo verás por pregunta: tu alternativa, la correcta y una explicación breve.</p>
    `;

    corrections.forEach(c => {
      const status = c.isCorrect ? "<span class='ok'>✔ Correcta</span>" : "<span class='bad'>✘ Incorrecta</span>";
      const chosenLabel = c.chosen === null ? "—" : ["A", "B", "C", "D"][c.chosen];
      const correctLabel = ["A", "B", "C", "D"][c.correct];

      html += `
        <div class="corr-item">
          <div><strong>${c.num}.</strong> ${status}</div>
          <div class="muted"><em>${c.question}</em></div>
          <div><strong>Tu respuesta:</strong> ${chosenLabel}) ${escapeHtml(c.chosenText)}</div>
          <div><strong>Correcta:</strong> ${correctLabel}) ${escapeHtml(c.correctText)}</div>
          <div><strong>Explicación:</strong> ${escapeHtml(c.exp)}</div>
        </div>
      `;
    });

    resultEl.innerHTML = html;
    // Scroll to result
    resultEl.scrollIntoView({ behavior: "smooth", block: "start" });
  }

  function resetAll() {
    const inputs = document.querySelectorAll("input[type=radio]");
    inputs.forEach(i => i.checked = false);
    resultEl.innerHTML = "<span class='muted'>Reiniciado. Vuelve a responder y calcula tu puntaje.</span>";
    window.scrollTo({ top: 0, behavior: "smooth" });
  }

  // Simple HTML escape for safe rendering
  function escapeHtml(str) {
    return String(str)
      .replaceAll("&", "&amp;")
      .replaceAll("<", "&lt;")
      .replaceAll(">", "&gt;")
      .replaceAll('"', "&quot;")
      .replaceAll("'", "&#039;");
  }

  gradeBtn.addEventListener("click", grade);
  resetBtn.addEventListener("click", resetAll);

  renderQuiz();
</script>
</body>
</html>
