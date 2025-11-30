<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Challenge Vial Safe</title>

<style>
    body {
        margin: 0;
        background: #d7e9ff;
        font-family: Arial, sans-serif;
        user-select: none;
    }

    /* Pantalla de inicio */
    #inicio {
        position: fixed;
        width: 100%; height: 100%;
        background: #b8d7ff;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        z-index: 10;
        text-align: center;
    }

    #inicio h1 {
        font-size: 50px;
        color: #003366;
        text-shadow: 3px 3px white;
    }

    .boton {
        padding: 15px 30px;
        background: #003366;
        color: white;
        border-radius: 10px;
        font-size: 22px;
        cursor: pointer;
        margin-top: 20px;
    }

    /* Contenedor del juego */
    #juego {
        display: flex;
        justify-content: center;
        margin-top: 20px;
    }

    /* TABLERO */
    #tablero {
        width: 600px;
        height: 600px;
        display: grid;
        background: white;
        border: 6px solid black;
        grid-template-columns: repeat(11, 1fr);
        grid-template-rows: repeat(11, 1fr);
        position: relative;
    }

    .casilla {
        border: 1px solid #ccc;
        background: #e9f4ff;
        font-size: 9px;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 2px;
    }

    .oculta {
        visibility: hidden;
    }

    /* Jugador */
    #jugador {
        width: 25px;
        height: 25px;
        background: red;
        border-radius: 50%;
        position: absolute;
        transition: 0.4s ease;
        z-index: 5;
        border: 2px solid black;
    }

    /* Botón dado */
    #panel {
        margin-left: 40px;
        padding: 20px;
        background: #ffffffaa;
        border-radius: 15px;
        height: 200px;
    }

    #btnDado {
        padding: 15px;
        background: #003366;
        color: white;
        font-size: 20px;
        border-radius: 12px;
        cursor: pointer;
        width: 160px;
    }

    /* Carta */
    #carta {
        position: fixed;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
        padding: 25px;
        width: 350px;
        background: white;
        border: 5px solid black;
        display: none;
        z-index: 20;
        text-align: center;
    }

    .respuesta {
        background: #003366;
        color: white;
        padding: 10px;
        margin: 7px 0;
        border-radius: 10px;
        cursor: pointer;
    }

    #puntajeBox {
        margin-top: 20px;
        font-size: 20px;
        font-weight: bold;
    }

</style>
</head>
<body>

<!-- PANTALLA DE INICIO -->
<div id="inicio">
    <h1>Challenge Vial Safe</h1>
    <p style="font-size:22px; max-width:600px;">
        Aprende educación vial mientras avanzas por un tablero estilo Monopoly.  
        Lanza el dado, responde preguntas, ¡y suma puntaje!
    </p>
    <div class="boton" onclick="iniciar()">Comenzar</div>
</div>

<!-- CONTENIDO DEL JUEGO -->
<div id="juego">
    <!-- TABLERO -->
    <div id="tablero"></div>

    <!-- PANEL LATERAL -->
    <div id="panel">
        <h2>Dado</h2>
        <div id="btnDado" onclick="tirarDado()">Tirar 🎲</div>

        <div id="puntajeBox">
            Puntaje: <span id="puntaje">0</span>
        </div>
    </div>
</div>

<!-- CARTA DE PREGUNTA -->
<div id="carta">
    <h3 id="pregunta"></h3>

    <div id="respuestas"></div>
</div>

<!-- JUGADOR -->
<div id="jugador"></div>

<script>

////////////////////////////////////////////
// GENERAR TABLERO EXTERIOR AUTOMÁTICO
////////////////////////////////////////////

const tablero = document.getElementById("tablero");
const posiciones = [];

function generarTablero() {
    for (let i = 0; i < 121; i++) {
        const div = document.createElement("div");
        div.classList.add("casilla");

        if (
            i < 11 || i >= 110 || i % 11 === 0 || i % 11 === 10
        ) {
            div.innerText = "Casilla " + posiciones.length;
            posiciones.push(i);
        } else {
            div.classList.add("oculta");
        }

        tablero.appendChild(div);
    }
}

generarTablero();

////////////////////////////////////////////
// POSICIÓN JUGADOR
////////////////////////////////////////////

const jugador = document.getElementById("jugador");
let indexJugador = 0;

function moverJugador() {
    const pos = posiciones[indexJugador];
    const fila = Math.floor(pos / 11);
    const col = pos % 11;

    jugador.style.top = (tablero.offsetTop + fila * 54 + 10) + "px";
    jugador.style.left = (tablero.offsetLeft + col * 54 + 10) + "px";
}

////////////////////////////////////////////
// PREGUNTAS
////////////////////////////////////////////

const preguntas = [
    {
        p: "¿Qué significa la señal PARE?",
        r: ["Detenerse completamente", "Reducir la velocidad", "Continuar con cuidado"],
        correcta: 0
    },
    {
        p: "¿Velocidad en zona escolar?",
        r: ["30 km/h", "60 km/h", "80 km/h"],
        correcta: 0
    },
    {
        p: "Luz amarilla intermitente significa:",
        r: ["Precaución y avanzar", "Detenerse siempre", "Velocidad máxima"],
        correcta: 0
    },
    {
        p: "Un peatón cruza entre autos, tú debes:",
        r: ["Detenerte", "Acelerar", "Tocar la bocina sin frenar"],
        correcta: 0
    }
];

function mostrarPregunta() {
    const carta = document.getElementById("carta");
    const p = preguntas[Math.floor(Math.random() * preguntas.length)];

    document.getElementById("pregunta").innerText = p.p;

    let html = "";
    p.r.forEach((texto, i) => {
        html += `<div class='respuesta' onclick='verificar(${i}, ${p.correcta})'>${texto}</div>`;
    });

    document.getElementById("respuestas").innerHTML = html;

    carta.style.display = "block";
}

let puntaje = 0;

function verificar(i, correcta) {
    if (i === correcta) {
        puntaje += 10;
        document.getElementById("puntaje").innerText = puntaje;
    }

    document.getElementById("carta").style.display = "none";
}

////////////////////////////////////////////
// DADO
////////////////////////////////////////////

function tirarDado() {
    const dado = Math.floor(Math.random() * 6) + 1;
    indexJugador = (indexJugador + dado) % posiciones.length;
    moverJugador();
    mostrarPregunta();
}

////////////////////////////////////////////
// INICIAR
////////////////////////////////////////////

function iniciar() {
    document.getElementById("inicio").style.display = "none";
    moverJugador();
}

</script>
</body>
</html>
