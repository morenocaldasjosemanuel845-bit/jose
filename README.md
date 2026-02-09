<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simulacro (60) - Alternativas similares</title>
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
    .tiny { font-size: 12px; color: #666; margin-top: 4px; }
  </style>
</head>
<body>
  <h1>Simulacro Teórico (60 preguntas)</h1>
  <p class="sub">Alternativas intencionalmente similares. Al final: puntaje + correcciones.</p>

  <div class="topnote">
    <strong>Regla:</strong> 1 alternativa por pregunta. Si dejas en blanco, se considera incorrecta.
    <div class="tiny">Basado en el REPASO (cap. 1–8): diseño, materiales, esfuerzos, deflexión, falla estática, fatiga, ejes, tornillos.</div>
  </div>

  <form id="quiz"></form>

  <div class="btns">
    <button type="button" id="gradeBtn">Calcular puntaje</button>
    <button type="button" id="resetBtn">Reiniciar</button>
  </div>

  <div id="result" aria-live="polite"></div>

<script>
  // NOTA: correct (a) varía, NO es siempre A.
  const questions = [
    // 1–10 Diseño / términos
    { q:"1) En diseño mecánico, ¿qué enunciado describe mejor el 'diseño'?",
      opts:[
        "Solo la elaboración de planos y tolerancias finales.",
        "Proceso para crear sistemas/componentes que cumplan función con seguridad y eficiencia.",
        "Proceso de fabricación y control de calidad posterior.",
        "Cálculo de costos para seleccionar el material más barato."
      ],
      a:1, exp:"En el repaso, el diseño se entiende como un proceso para crear soluciones que cumplan función, seguridad y eficiencia." },

    { q:"2) La 'necesidad' en diseño se refiere a:",
      opts:[
        "El problema o requerimiento que origina el diseño.",
        "El conjunto de restricciones de fabricación.",
        "La lista de ensayos para validación.",
        "La norma que regula el producto."
      ],
      a:0, exp:"Necesidad = problema/requerimiento que motiva el diseño." },

    { q:"3) Una 'especificación' es:",
      opts:[
        "La restricción que limita el número de alternativas.",
        "Una recomendación opcional del fabricante del material.",
        "El conjunto de requisitos que debe cumplir el diseño.",
        "El margen entre resistencia y esfuerzo aplicado."
      ],
      a:2, exp:"Especificación = requisitos que el diseño debe cumplir." },

    { q:"4) Una 'restricción' en el proceso de diseño es:",
      opts:[
        "El esfuerzo máximo para vida infinita.",
        "Una limitación que reduce opciones (costo/espacio/proceso).",
        "El criterio de falla para dúctiles.",
        "La relación esfuerzo–deformación elástica."
      ],
      a:1, exp:"Restricción = limitación práctica o técnica que acota soluciones." },

    { q:"5) ¿Cuál describe mejor un 'código de diseño'?",
      opts:[
        "Guía técnica con reglas para asegurar seguridad y funcionamiento.",
        "Catálogo comercial de componentes estándar.",
        "Modelo matemático de fatiga (S–N).",
        "Método gráfico para transformar esfuerzos."
      ],
      a:0, exp:"Código de diseño = reglas técnicas para garantizar seguridad y desempeño." },

    { q:"6) La 'norma técnica' se usa para:",
      opts:[
        "Definir esfuerzos equivalentes por Von Mises.",
        "Estimar velocidad crítica de un eje.",
        "Estandarizar especificaciones (materiales/dimensiones/procesos).",
        "Reemplazar ensayos experimentales."
      ],
      a:2, exp:"Norma técnica estandariza requisitos: materiales, dimensiones, procesos, etc." },

    { q:"7) Factor de diseño (n_d) es:",
      opts:[
        "Relación entre resistencia y esfuerzo permisible adoptado.",
        "Relación entre esfuerzo alternante y esfuerzo medio.",
        "Probabilidad de no fallar en servicio.",
        "Máxima carga sin deformación permanente."
      ],
      a:0, exp:"El repaso define factor de diseño como relación resistencia / esfuerzo permisible." },

    { q:"8) Factor de seguridad (n) se entiende mejor como:",
      opts:[
        "Un valor impuesto antes de diseñar, sin relación con dimensiones finales.",
        "El valor real resultante tras definir el diseño (margen logrado).",
        "El esfuerzo donde inicia la deformación plástica.",
        "La pendiente de la curva esfuerzo–deformación."
      ],
      a:1, exp:"Factor de seguridad = margen real obtenido con el diseño final." },

    { q:"9) Confiabilidad es:",
      opts:[
        "Resistencia última dividida entre esfuerzo equivalente.",
        "Probabilidad de que un elemento funcione sin fallar.",
        "Capacidad de deformarse plásticamente antes de fallar.",
        "Inestabilidad bajo compresión."
      ],
      a:1, exp:"Confiabilidad = probabilidad de cumplir función sin falla." },

    { q:"10) Responsabilidad legal del producto se refiere a:",
      opts:[
        "Obligación del fabricante ante daños por fallas del producto.",
        "Límite elástico establecido por norma ISO.",
        "Costo asociado al mantenimiento.",
        "Ensayo de carga para fatiga."
      ],
      a:0, exp:"Responsabilidad legal = obligación del fabricante por daños causados por fallas." },

    // 11–20 Materiales
    { q:"11) Módulo de elasticidad (E) representa:",
      opts:[
        "Relación esfuerzo normal / deformación elástica (zona lineal).",
        "Máximo esfuerzo sin deformación permanente.",
        "Máximo esfuerzo antes de fractura.",
        "Energía de distorsión para falla dúctil."
      ],
      a:0, exp:"E vincula esfuerzo normal con deformación elástica en régimen lineal." },

    { q:"12) Límite proporcional es:",
      opts:[
        "Inicio de la deformación plástica.",
        "Fin de la relación lineal esfuerzo–deformación.",
        "Máximo esfuerzo antes de fractura.",
        "Esfuerzo máximo para vida infinita."
      ],
      a:1, exp:"Marca el fin de la linealidad (ley de Hooke estricta)." },

    { q:"13) Límite elástico se define mejor como:",
      opts:[
        "Esfuerzo donde inicia la deformación plástica (fluencia).",
        "Máximo esfuerzo sin dejar deformación permanente al descargar.",
        "Máximo esfuerzo alcanzable en ensayo de fatiga.",
        "Esfuerzo normal máximo en un estado plano."
      ],
      a:1, exp:"Límite elástico = máximo esfuerzo sin deformación permanente." },

    { q:"14) Resistencia última (Sut) es:",
      opts:[
        "Máximo esfuerzo soportado antes de fracturarse.",
        "Esfuerzo donde empieza la plasticidad.",
        "Fin de la zona elástica lineal.",
        "Esfuerzo equivalente de Von Mises."
      ],
      a:0, exp:"Sut = máximo esfuerzo que alcanza el material antes de fractura." },

    { q:"15) Ductilidad es:",
      opts:[
        "Tendencia a fracturarse sin deformación plástica.",
        "Capacidad de deformarse plásticamente antes de fallar.",
        "Resistencia a la corrosión.",
        "Capacidad de resistir cargas cíclicas indefinidamente."
      ],
      a:1, exp:"Ductilidad = deformación plástica significativa antes de ruptura." },

    { q:"16) Fragilidad es:",
      opts:[
        "Capacidad de deformarse plásticamente sin romper.",
        "Tendencia a fracturarse con poca o nula deformación plástica.",
        "Resistencia a esfuerzos cortantes.",
        "Capacidad de recuperar deformación plástica."
      ],
      a:1, exp:"Frágil = rompe sin gran deformación plástica previa." },

    { q:"17) En términos generales, el trabajo en frío:",
      opts:[
        "Aumenta resistencia y reduce ductilidad.",
        "Reduce resistencia y aumenta ductilidad.",
        "Elimina la concentración de esfuerzos.",
        "Aumenta el módulo de elasticidad de forma significativa."
      ],
      a:0, exp:"Trabajo en frío endurece: sube resistencia y baja ductilidad." },

    { q:"18) ¿Cuál afirmación es más correcta sobre E en aceros (idea de ejes)?",
      opts:[
        "E cambia mucho con el tipo de acero; por eso mejora deflexión comprando acero caro.",
        "E es casi constante en aceros; para reducir deflexión suele servir más aumentar diámetro.",
        "E solo importa en fatiga, no en deflexión.",
        "E solo depende de la rugosidad superficial."
      ],
      a:1, exp:"En aceros, E es aproximadamente constante; la deflexión se mejora más con geometría." },

    { q:"19) La fluencia se asocia principalmente con:",
      opts:[
        "Deformación permanente.",
        "Deformación elástica recuperable.",
        "Inestabilidad por compresión.",
        "Fractura frágil inmediata."
      ],
      a:0, exp:"Fluencia implica inicio de deformación plástica permanente." },

    { q:"20) En un diseño estático, una pieza dúctil se evalúa típicamente con:",
      opts:[
        "Esfuerzo normal máximo.",
        "Energía de distorsión (Von Mises).",
        "Círculo de Mohr como criterio de falla.",
        "Curva S–N."
      ],
      a:1, exp:"En el repaso: Von Mises se usa para dúctiles." },

    // 21–30 Análisis de esfuerzos
    { q:"21) Esfuerzo normal es:",
      opts:[
        "Paralelo al área.",
        "Perpendicular al área.",
        "Siempre máximo en torsión.",
        "Exclusivo de carga cíclica."
      ],
      a:1, exp:"Normal: perpendicular al área de la sección." },

    { q:"22) Esfuerzo cortante es:",
      opts:[
        "Perpendicular al área.",
        "Paralelo al área.",
        "Siempre nulo en torsión.",
        "Igual a Sut/2 en cualquier material."
      ],
      a:1, exp:"Cortante: actúa paralelo al plano de la sección." },

    { q:"23) La flexión se caracteriza porque:",
      opts:[
        "Genera esfuerzo normal que varía en la sección.",
        "Genera solo esfuerzo cortante uniforme.",
        "No depende de la geometría.",
        "Solo aparece en tornillos."
      ],
      a:0, exp:"Flexión induce esfuerzos normales variables a través de la sección." },

    { q:"24) La torsión en un eje produce principalmente:",
      opts:[
        "Esfuerzo normal uniforme.",
        "Esfuerzo cortante.",
        "Solo esfuerzo principal máximo sin cortante.",
        "Solo deflexión axial."
      ],
      a:1, exp:"Torsión produce esfuerzo cortante." },

    { q:"25) Esfuerzos principales son:",
      opts:[
        "Los esfuerzos normales máximos/mínimos en planos con cortante cero.",
        "Los cortantes máximos con normal cero.",
        "Los esfuerzos equivalentes de fatiga.",
        "Los esfuerzos que aparecen solo en ejes."
      ],
      a:0, exp:"Principales: normales extremos en planos sin cortante." },

    { q:"26) El círculo de Mohr sirve para:",
      opts:[
        "Determinar esfuerzos principales y cortante máximo en un punto.",
        "Determinar la vida a fatiga directamente.",
        "Eliminar concentraciones geométricas.",
        "Calcular el módulo de elasticidad."
      ],
      a:0, exp:"Herramienta gráfica de transformación de esfuerzos." },

    { q:"27) Concentración de esfuerzos significa:",
      opts:[
        "Aumento local del esfuerzo por discontinuidades geométricas.",
        "Aumento del módulo de elasticidad por tratamiento térmico.",
        "Aumento de resistencia última por trabajo en frío.",
        "Reducción del esfuerzo nominal en la sección."
      ],
      a:0, exp:"Concentración: picos locales por geometría (agujeros, ranuras, escalones)." },

    { q:"28) ¿Qué enunciado es más correcto?",
      opts:[
        "Una entalla siempre aumenta el esfuerzo nominal, no el local.",
        "Una entalla aumenta el esfuerzo local; el nominal puede no cambiar.",
        "Una entalla elimina la posibilidad de fatiga.",
        "Una entalla solo afecta materiales frágiles."
      ],
      a:1, exp:"La entalla eleva el esfuerzo local; el nominal es promedio/teórico." },

    { q:"29) Esfuerzo equivalente (en estática) se usa para:",
      opts:[
        "Comparar un estado complejo de esfuerzos con una resistencia del material.",
        "Calcular la rugosidad superficial.",
        "Reemplazar el círculo de Mohr en todas las situaciones.",
        "Determinar la corrosión."
      ],
      a:0, exp:"El equivalente resume el estado de esfuerzos para comparar con resistencia (ej. Von Mises)." },

    { q:"30) Para materiales frágiles, el repaso sugiere evaluar con:",
      opts:[
        "Esfuerzo normal máximo.",
        "Von Mises siempre.",
        "Superposición.",
        "Castigliano."
      ],
      a:0, exp:"En el repaso: frágiles → esfuerzo normal máximo." },

    // 31–40 Deflexión y rigidez
    { q:"31) Deflexión es:",
      opts:[
        "Deformación producida por una carga.",
        "Separación total del material.",
        "Probabilidad de no fallar.",
        "Esfuerzo máximo para vida infinita."
      ],
      a:0, exp:"Deflexión = deformación/desplazamiento por carga." },

    { q:"32) Rigidez es:",
      opts:[
        "Resistencia a la deformación.",
        "Tendencia a fracturarse sin deformación plástica.",
        "Número de ciclos hasta la falla.",
        "Máximo esfuerzo sin deformación permanente."
      ],
      a:0, exp:"Rigidez describe oposición a deformarse." },

    { q:"33) Superposición en deflexiones aplica cuando:",
      opts:[
        "El comportamiento es lineal elástico.",
        "Existe deformación plástica significativa.",
        "Hay pandeo post-crítico.",
        "La fatiga domina el diseño."
      ],
      a:0, exp:"Se usa en sistemas lineales: efectos de cargas se suman." },

    { q:"34) Castigliano se usa principalmente para:",
      opts:[
        "Calcular deflexiones mediante energía de deformación.",
        "Determinar resistencia última.",
        "Obtener la curva S–N experimentalmente.",
        "Determinar esfuerzo principal con un método gráfico."
      ],
      a:0, exp:"Castigliano: método energético para deflexiones." },

    { q:"35) Controlar deflexión es importante porque:",
      opts:[
        "Una pieza puede no romper, pero fallar funcionalmente.",
        "Siempre reduce la resistencia última del material.",
        "Elimina la fatiga automáticamente.",
        "Incrementa la confiabilidad sin cambiar geometría."
      ],
      a:0, exp:"Aunque no haya fractura, puede fallar funcionalmente (alineación, contacto, etc.)." },

    { q:"36) Pandeo es:",
      opts:[
        "Inestabilidad por compresión (lateral).",
        "Fractura por tracción pura.",
        "Deformación elástica recuperable.",
        "Falla por cargas cíclicas."
      ],
      a:0, exp:"Pandeo: pérdida de estabilidad bajo compresión en elementos esbeltos." },

    { q:"37) Un elemento típicamente susceptible al pandeo es:",
      opts:[
        "Columna esbelta.",
        "Eje corto macizo a torsión pura.",
        "Tornillo corto a tracción.",
        "Placa gruesa a compresión uniforme."
      ],
      a:0, exp:"Columnas esbeltas son el caso típico de pandeo." },

    { q:"38) La ley que relaciona esfuerzo y deformación elástica es:",
      opts:[
        "Hooke.",
        "Coulomb.",
        "Bernoulli.",
        "Pascal."
      ],
      a:0, exp:"Ley de Hooke: esfuerzo proporcional a deformación en elástico lineal." },

    { q:"39) Una constante de resorte se interpreta como:",
      opts:[
        "Relación fuerza/deflexión (k).",
        "Relación esfuerzo/ciclos (S–N).",
        "Relación Sut/Sy.",
        "Relación esfuerzo equivalente/resistencia."
      ],
      a:0, exp:"k vincula fuerza con deflexión en comportamiento lineal." },

    { q:"40) En ejes, para reducir deflexión normalmente es más efectivo:",
      opts:[
        "Aumentar diámetro.",
        "Comprar acero 'más resistente' esperando que E suba mucho.",
        "Reducir el factor de seguridad.",
        "Pulir superficie para aumentar E."
      ],
      a:0, exp:"E en aceros casi no cambia; la geometría (diámetro) influye fuerte en rigidez." },

    // 41–50 Falla estática
    { q:"41) Falla estática se refiere a:",
      opts:[
        "Falla por carga aplicada una sola vez (o no cíclica).",
        "Falla siempre por corrosión.",
        "Falla por vibración a velocidad crítica.",
        "Falla por cargas cíclicas."
      ],
      a:0, exp:"Estática: carga no repetitiva (comparado con fatiga)." },

    { q:"42) Sobrecarga ocurre cuando:",
      opts:[
        "El esfuerzo aplicado supera la resistencia.",
        "El esfuerzo aplicado es menor que el límite elástico.",
        "La rugosidad superficial reduce Se.",
        "Se usa Von Mises en frágiles."
      ],
      a:0, exp:"Sobrecarga = esfuerzo excede resistencia ⇒ falla." },

    { q:"43) En dúctiles, la 'falla' estática más típica antes de fractura es:",
      opts:[
        "Fluencia (deformación permanente).",
        "Pandeo siempre.",
        "Corrosión acelerada.",
        "Vida finita por fatiga."
      ],
      a:0, exp:"En dúctiles, suele gobernar la fluencia antes de fractura final." },

    { q:"44) Un criterio de falla es:",
      opts:[
        "Regla/método para decidir falla comparando esfuerzos y resistencias.",
        "Una medida de rugosidad superficial.",
        "Un tipo de tornillo de alta resistencia.",
        "Un sinónimo de norma técnica."
      ],
      a:0, exp:"Criterio de falla: teoría para predecir falla en estados de esfuerzo." },

    { q:"45) Von Mises se asocia con:",
      opts:[
        "Materiales dúctiles y energía de distorsión.",
        "Materiales frágiles y esfuerzo normal máximo.",
        "Deflexión por energía total.",
        "Curvas S–N de fatiga."
      ],
      a:0, exp:"Von Mises (energía de distorsión) es criterio típico para dúctiles." },

    { q:"46) Esfuerzo normal máximo se asocia con:",
      opts:[
        "Materiales frágiles.",
        "Materiales dúctiles.",
        "Método de Castigliano.",
        "Ecuación de Paris."
      ],
      a:0, exp:"En el repaso: frágiles → esfuerzo normal máximo." },

    { q:"47) Esfuerzo equivalente es útil porque:",
      opts:[
        "Permite comparar un estado multiaxial con una resistencia uniaxial.",
        "Elimina concentraciones de esfuerzos.",
        "Evita la necesidad de conocer geometría.",
        "Es igual a Sy para cualquier estado."
      ],
      a:0, exp:"Resume el estado para compararlo contra resistencia (ej. Von Mises)." },

    { q:"48) En el diseño estático, el factor de seguridad se interpreta como:",
      opts:[
        "Margen entre resistencia y esfuerzo aplicado.",
        "Número de ciclos hasta falla.",
        "Relación fuerza-deflexión.",
        "Relación entre rugosidad y corrosión."
      ],
      a:0, exp:"En estática: margen para reducir riesgo frente a incertidumbre." },

    { q:"49) Un diseño conservador implica usualmente:",
      opts:[
        "Mayor factor de seguridad.",
        "Menor factor de seguridad.",
        "Ignorar concentraciones de esfuerzo.",
        "Diseñar solo con fatiga."
      ],
      a:0, exp:"Conservador = más margen (más n) para reducir probabilidad de falla." },

    { q:"50) La fractura se define como:",
      opts:[
        "Separación total del material.",
        "Inicio de deformación plástica.",
        "Fin de la linealidad esfuerzo-deformación.",
        "Esfuerzo máximo para vida infinita."
      ],
      a:0, exp:"Fractura = ruptura/separación completa del material." },

    // 51–60 Fatiga + ejes + tornillos (repaso)
    { q:"51) Fatiga es:",
      opts:[
        "Falla por cargas cíclicas/repetidas.",
        "Falla por una sola carga aplicada.",
        "Solo corrosión en la superficie.",
        "Inestabilidad por compresión."
      ],
      a:0, exp:"Fatiga: falla por cargas cíclicas." },

    { q:"52) Según el repaso, la fatiga puede ocurrir:",
      opts:[
        "Solo por encima del límite elástico.",
        "Incluso por debajo del límite elástico.",
        "Solo si hay pandeo.",
        "Solo si el material es frágil."
      ],
      a:1, exp:"El repaso indica que puede ocurrir bajo el límite elástico." },

    { q:"53) Curva S–N representa:",
      opts:[
        "Relación esfuerzo–número de ciclos.",
        "Relación fuerza–deflexión.",
        "Relación esfuerzo–deformación elástica.",
        "Relación torque–ángulo de giro."
      ],
      a:0, exp:"S–N: esfuerzo alternante vs ciclos a falla." },

    { q:"54) Vida a fatiga es:",
      opts:[
        "Número de ciclos hasta la falla.",
        "Máximo esfuerzo sin deformación permanente.",
        "Probabilidad de no fallar.",
        "Energía de distorsión crítica."
      ],
      a:0, exp:"Vida: ciclos hasta falla." },

    { q:"55) Límite de resistencia a la fatiga es:",
      opts:[
        "Esfuerzo máximo para vida infinita.",
        "Esfuerzo donde inicia la plasticidad.",
        "Máximo esfuerzo antes de fractura.",
        "Fin de la zona lineal."
      ],
      a:0, exp:"En el repaso: límite de fatiga = esfuerzo máximo para vida infinita." },

    { q:"56) Una grieta por fatiga inicia típicamente en:",
      opts:[
        "Concentraciones de esfuerzo.",
        "Cualquier punto, siempre al azar.",
        "Solo en el centro geométrico.",
        "Zonas con compresión pura y sin entallas."
      ],
      a:0, exp:"Se inicia en concentradores por los picos locales de esfuerzo." },

    { q:"57) Alto ciclaje (según repaso) corresponde a:",
      opts:[
        "N > 10^3 ciclos.",
        "N < 10^3 ciclos.",
        "N < 10^2 ciclos.",
        "N > 10^9 ciclos exclusivamente."
      ],
      a:0, exp:"Repaso: alto ciclaje > 10^3." },

    { q:"58) Bajo ciclaje (según repaso) corresponde a:",
      opts:[
        "N < 10^3 ciclos.",
        "N > 10^3 ciclos.",
        "N = 10^6 ciclos.",
        "N = 1 ciclo."
      ],
      a:0, exp:"Repaso: bajo ciclaje < 10^3." },

    { q:"59) En ejes, el repaso indica que el criterio usado para diseño es:",
      opts:[
        "Von Mises.",
        "Esfuerzo normal máximo.",
        "Castigliano.",
        "Círculo de Mohr."
      ],
      a:0, exp:"En el repaso: ejes se diseñan con Von Mises." },

    { q:"60) En uniones atornilladas, 'precarga' es:",
      opts:[
        "Fuerza inicial generada al apretar el tornillo.",
        "Fuerza externa máxima antes de fluencia.",
        "Fuerza que aparece solo en fatiga.",
        "Fuerza que elimina el pandeo."
      ],
      a:0, exp:"Precarga: fuerza inicial por apriete que mantiene unidas las piezas." },
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
        label.appendChild(document.createTextNode(" " + ["A) ","B) ","C) ","D) "][j] + opt));

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

  function escapeHtml(str) {
    return String(str)
      .replaceAll("&", "&amp;")
      .replaceAll("<", "&lt;")
      .replaceAll(">", "&gt;")
      .replaceAll('"', "&quot;")
      .replaceAll("'", "&#039;");
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
    const wrongs = corrections.filter(x => !x.isCorrect).length;

    let html = `
      <h2>Resultado</h2>
      <p><strong>Puntaje:</strong> ${score} / ${questions.length} <span class="pill">${pct}%</span></p>
      <p><strong>Incorrectas:</strong> ${wrongs} | <strong>Correctas:</strong> ${score}</p>
      <h2>Correcciones</h2>
      <p class="muted">Para cada pregunta: tu alternativa, la correcta y la explicación.</p>
    `;

    corrections.forEach(c => {
      const status = c.isCorrect ? "<span class='ok'>✔ Correcta</span>" : "<span class='bad'>✘ Incorrecta</span>";
      const chosenLabel = c.chosen === null ? "—" : ["A","B","C","D"][c.chosen];
      const correctLabel = ["A","B","C","D"][c.correct];

      html += `
        <div class="corr-item">
          <div><strong>${c.num}.</strong> ${status}</div>
          <div class="muted"><em>${escapeHtml(c.question)}</em></div>
          <div><strong>Tu respuesta:</strong> ${chosenLabel}) ${escapeHtml(c.chosenText)}</div>
          <div><strong>Correcta:</strong> ${correctLabel}) ${escapeHtml(c.correctText)}</div>
          <div><strong>Explicación:</strong> ${escapeHtml(c.exp)}</div>
        </div>
      `;
    });

    resultEl.innerHTML = html;
    resultEl.scrollIntoView({ behavior:"smooth", block:"start" });
  }

  function resetAll() {
    const inputs = document.querySelectorAll("input[type=radio]");
    inputs.forEach(i => i.checked = false);
    resultEl.innerHTML = "<span class='muted'>Reiniciado. Vuelve a responder y calcula tu puntaje.</span>";
    window.scrollTo({ top: 0, behavior: "smooth" });
  }

  gradeBtn.addEventListener("click", grade);
  resetBtn.addEventListener("click", resetAll);

  renderQuiz();
</script>
</body>
</html>
