<script>
  import { onMount } from 'svelte';
  
  let kor = [88, 93, 95, 97, 99];
  let usa = [75, 82, 78, 70, 67];
  let years = [2008, 2012, 2016, 2020, 2024];
  let svgWidth = 500;
  let svgHeight = 500;
  let centerX = svgWidth / 2;
  let centerY = svgHeight / 2;
  let maxRadius = Math.min(centerX, centerY) - 20;
  let positions = [];
  let positions2 = [];

  let tooltipVisible = false;
  let tooltipText = '';
  let tooltiplabel = '';
  let tooltipX = 0;
  let tooltipY = 0;

  function calculatePositionAndRotation(number) {
    let radius = ((number-115) / 100) * maxRadius;
    let angle = Math.random() * 360;
    let radians = angle * (Math.PI / 180);

    let x = centerX + radius * Math.cos(radians);
    let y = centerY + radius * Math.sin(radians);
    let rotation = angle + 90;

    return { x, y, rotation };
  }

  function showTooltip(event, number, label) {
    tooltipVisible = true;
    tooltipText = number;
    tooltiplabel= label;
    tooltipX = event.clientX -20;
    tooltipY = event.clientY -150;
  }

  function hideTooltip() {
    tooltipVisible = false;
  }

  onMount(() => {
    positions = kor.map(calculatePositionAndRotation);
    positions2 = usa.map(calculatePositionAndRotation);
  });
</script>

<!-- Imagen SVG centrada arriba del título -->
<div style="text-align: center; margin-bottom: 40px; margin-top: 35px;">
  <img src="./images/aro.png" alt="aro" style="width: 200px; height: auto;" />
</div>

<!-- Título -->
<h1 style="text-align: center; margin-bottom: 20px; margin-right:50px; margin-left:50px">"Corea vs EEUU: Duelo en el Tiro con Arco Olímpico (2008-2024)"</h1>

<!-- Contenedor centrado -->
<div style="display: flex; justify-content: center; margin-bottom: 15px; align-items: center; position: relative;">
  <svg width={svgWidth} height={svgHeight}>
    <!-- Imagen de la diana -->
    <image href="./images/Diana.svg" x={centerX - maxRadius} y={centerY - maxRadius} width={maxRadius * 2} height={maxRadius * 2} />

    <!-- Flechas de Corea del Sur -->
    {#each positions as { x, y, rotation }, index}
      <g 
        role="img"
        transform={`translate(${x},${y}) rotate(${rotation})`} 
        on:mouseenter={(event) => showTooltip(event, kor[index], "KOR")}
        on:mouseleave={hideTooltip}>
        <image href="./images/flecha kor.svg" x={-10} y={-50} width="25" height="110" />
      </g>
    {/each}

    <!-- Flechas de Estados Unidos -->
    {#each positions2 as { x, y, rotation }, index}
      <g   
        role="img"
        transform={`translate(${x},${y}) rotate(${rotation})`} 
        on:mouseenter={(event) => showTooltip(event, usa[index], "USA")}
        on:mouseleave={hideTooltip}>
        <image href="./images/flecha usa.svg" x={-10} y={-50} width="25" height="110" />
      </g>
    {/each}
  </svg>

  <!-- Tooltip -->
  {#if tooltipVisible}
    <div style="position: absolute; top: {tooltipY}px; left: {tooltipX}px; background-color: black; color: white; padding: 5px; border-radius: 3px;">
      {tooltipText}
    </div>
  {/if}
</div>

<div class="container">
  <div>
    <span class="cuadro rojo"></span>Corea del Sur
  </div>
  <div>
    <span class="cuadro azul"></span>Estados Unidos
  </div>
</div>

<!-- Texto debajo de la diana -->
<p style=" margin-top: 20px; margin-bottom: 40px; margin-right:50px; margin-left:50px">En la visualización, cada flecha representa el desempeño de un país en un año olímpico específico. La diana simboliza el objetivo máximo de 100 puntos, con las posiciones de las flechas mostrando cuán cerca estuvieron de alcanzar la perfección en cada edición de los Juegos.</p>

<!-- Contenedor para la imagen y la tabla -->
<div style="display: flex; justify-content: center; align-items: center; margin-top: 35px; margin-bottom: 80px;">
  <!-- Imagen del tirador a la izquierda -->
  <div style="margin-right: 100px; margin-left: 200px">
    <img src="./images/tirador.avif" alt="tirador" style="width: 500px; height: auto;" />
  </div>

  <!-- Tabla de resultados a la derecha -->
  <div style="flex-grow: 1;">
    <table style="border-collapse: collapse; width: 75%;">
      <thead>
        <tr>
          <th style="border: 1px solid black; padding: 10px;">Año</th>
          <th style="border: 1px solid black; padding: 10px;">Corea del Sur</th>
          <th style="border: 1px solid black; padding: 10px;">Estados Unidos</th>
        </tr>
      </thead>
      <tbody>
        {#each years as year, index}
          <tr>
            <td style="border: 1px solid black; padding: 10px;">{year}</td>
            <td style="border: 1px solid black; padding: 10px; color: red;">{kor[index]}</td>
            <td style="border: 1px solid black; padding: 10px; color: blue;">{usa[index]}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>

<p style="margin-top: 50px; margin-bottom: 0px; font-size:30px;"><strong>Los ultimos años</strong></p>

<p style="margin-top: 30px; margin-right:50px; margin-left:50px">Desde los Juegos Olímpicos de 2008 hasta 2024, Corea del Sur y Estados Unidos han sido los principales contendientes en tiro con arco. Estos países han dominado el medallero, con una rivalidad que se ha intensificado a lo largo de los años.</p>

<!-- Contenedor para la visualización de Flourish -->
<div class="fl_container" style="width: 80%; height: 550px; margin: 0 auto; margin-top: 50px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-chart" data-src="visualisation/19287848" style="width: 100%; height: 100%;">
    <script src="https://public.flourish.studio/resources/embed.js"></script>
  </div>
</div>

<!-- Descripción debajo de la tabla -->
<div style="margin-top: 60px; margin-right:50px; margin-left:50px">
  <p style="margin-bottom: 5px"><strong>2008: El Inicio de la Dominación de Corea del Sur</strong></p>
  <p style="margin-bottom: 20px">En los Juegos Olímpicos de 2008 en Beijing, Corea del Sur reafirmó su supremacía en tiro con arco, alcanzando 88 puntos y asegurando varias medallas de oro. Estados Unidos, aunque competitivo, quedó rezagado con 75 puntos, comenzando a notar la brecha entre ellos y los surcoreanos.</p>
  <div class="fl_container" style="width: 80%; height: 300px; margin: 0 auto; margin-top: 30px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/19291346"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/19291346/thumbnail" width="100%" alt="bar-chart-race visualization" /></noscript></div>
  </div>
  
  <p style="margin-top: 40px; margin-bottom: 5px"><strong>2012: La Rivalidad se Intensifica</strong></p>
  <p style="margin-bottom: 20px">En Londres 2012, Corea del Sur mejoró su desempeño, alcanzando 93 puntos, mientras que Estados Unidos también subió su nivel, logrando 82 puntos. Este fue un año en el que la rivalidad se intensificó, con ambos países luchando por cada medalla.</p>
  <div class="fl_container" style="width: 80%; height: 300px; margin: 0 auto; margin-top: 30px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/19419911"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/19419911/thumbnail" width="100%" alt="bar-chart-race visualization" /></noscript></div>
  </div>

  <p style="margin-top: 50px; margin-bottom: 5px"><strong>2016: La Consistencia de Corea del Sur</strong></p>
  <p style="margin-bottom: 20px">Durante los Juegos Olímpicos de 2016 en Río, Corea del Sur casi alcanzó la perfección con 95 puntos, consolidando su dominio. Estados Unidos, a pesar de mantener un buen nivel, no pudo seguir el ritmo y terminó con 78 puntos. Este año reafirmó a Corea del Sur como la potencia indiscutible en tiro con arco.</p>
  <div class="fl_container" style="width: 80%; height: 300px; margin: 0 auto; margin-top: 30px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/19420439"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/19420439/thumbnail" width="100%" alt="bar-chart-race visualization" /></noscript></div>
  </div>

  <p style="margin-top: 50px; margin-bottom: 5px"><strong>2020: La Diferencia se Amplía</strong></p>
  <p style="margin-bottom: 20px">En Tokio 2020, Corea del Sur continuó su racha de excelencia con 97 puntos, acercándose aún más al puntaje perfecto. Estados Unidos, por otro lado, vio una caída en su desempeño, alcanzando solo 70 puntos. La brecha entre ambos países se hizo más evidente, reflejando la consistencia y precisión de los arqueros surcoreanos.</p>
  <div class="fl_container" style="width: 80%; height: 300px; margin: 0 auto; margin-top: 30px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/19420583"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/19420583/thumbnail" width="100%" alt="bar-chart-race visualization" /></noscript></div>
  </div>

  <p style="margin-top: 50px; margin-bottom: 5px"><strong>2024: La Perfección Alcanzada</strong></p>
  <p style="margin-bottom: 20px">En los Juegos Olímpicos de París 2024, Corea del Sur finalmente alcanzó casi la perfección con 99 puntos, estableciendo un récord en la historia del tiro con arco. Estados Unidos, a pesar de su esfuerzo, continuó su declive con 67 puntos. Este año marcó el clímax de la rivalidad, con Corea del Sur consolidando su estatus como la nación más dominante en la historia del tiro con arco olímpico.</p>
  <div class="fl_container" style="width: 80%; height: 300px; margin: 0 auto; margin-top: 30px; margin-botton: 100px; text-align: center;">
  <div class="flourish-embed flourish-bar-chart-race" data-src="visualisation/19420633"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/19420633/thumbnail" width="100%" alt="bar-chart-race visualization" /></noscript></div>
  </div>
</div>

<footer class="footer">
  <p style= "font-size: 15px; margin-bottom: 5px ">Creado por Federico Villanueva</p>
  <p style= "font-size: 15px; margin-bottom: 5px">
    <a href="https://www.linkedin.com/in/federico-mateo-villanueva-a52196279" target="_blank">LinkedIn</a> |
    <a href="https://github.com/fedemvilla/vd_maqueta-main" target="_blank">GitHub</a>
  </p>
  <p style= "font-size: 15px; margin-bottom: 20px">Visualización de Datos, Universidad Di Tella</p>
</footer>

<link href="https://fonts.googleapis.com/css2?family=Lato:wght@300;400&family=Open+Sans:wght@300;400&display=swap" rel="stylesheet">

<style>
  /* Título - Futura Bold o una alternativa como Roboto Condensed */
  h1 {
    font-family: 'Futura', 'Roboto Condensed', sans-serif;
    font-weight: bold;
    font-size: 36px;
    text-align: center;
    margin-bottom: 50px;
    margin-right: 50px;
    margin-left: 50px;
  }
  
  /* Subtítulo - Lato */
  p {
    font-family: 'Lato', sans-serif;
    font-size: 18px;
    margin-top: 20px;
    margin-bottom: 50px;
    margin-right: 50px;
    margin-left: 50px;
  }
  
  .cuadro {
    width: 15px;  /* Tamaño del cuadrado */
    height: 15px;
    display: inline-block;
    margin-right: 8px;/* Espacio entre el cuadrado y el texto */
  }

  .rojo {
    background-color: red;
  }

  .azul {
    background-color: blue;
  }
  
  .container {
    display: flex;
    flex-direction: row;
    gap: 50px;  /* Espacio entre cada fila */
    justify-content: center;
    align-items: center;
    margin-bottom: 30px;
  }

  .footer a {
    color: #0077b5;  /* Color para los enlaces */
    text-decoration: none;
    margin: 0 3px;
  }

  .footer {
    text-align: center;
    color: gray;  /* Color gris clarito */
    padding: 20px;
  }

  .footer a:hover {
    color: gray;  /* Cambia a un gris más oscuro al pasar el ratón por encima */
  }
</style>