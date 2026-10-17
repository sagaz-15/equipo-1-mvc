# Entrega · Laboratorio MVC · Notas del semestre

**Equipo: 1**  
**Integrantes:** Santiago Machado, Sergio Perez, Juan Manuel Restrepo  
**Repositorio:** https://github.com/sagaz-15/equipo-1-mvc.git

> Completa este archivo a medida que avanzas (cada sección dice en qué parte de la guía se llena).
> Escribe con tus palabras: las respuestas copiadas de la guía o de otro equipo no cuentan.

---

## 1. Mapa de responsabilidades (Parte 1)

Clasifica cada fragmento de `espagueti.html`: **M** (Modelo), **V** (Vista), **C** (Controlador) o **Mezcla**
(en ese caso, escribe qué roles se mezclan). En "¿Por qué?" justifica en una frase.

| # | Fragmento | Rol(es) | ¿Por qué? |
|---|---|---|---|
| 1 | `var n = document.getElementById('nombre').value.trim();` | V | Lo clasificamos como Vista porque solo lee lo que el usuario escribio en el formulario. No decide nada con ese dato. |
| 2 | `if (isNaN(p) \|\| p < 1 \|\| p > 100) { err.textContent = 'El peso debe estar entre 1 y 100.'; return; }` | Mezcla de M y V | La condicion del peso entre 1 y 100 es una regla del negocio, pero en la misma linea se escribe el mensaje en la pantalla con err.textContent. |
| 3 | `if (total + p > 100) { err.textContent = '...'; return; }` | Mezcla de M y V | Revisar que los pesos no pasen de 100 es una regla, y mostrar el mensaje de error es de pantalla. Van pegadas en el mismo if. |
| 4 | `evals.push({ id: sig++, nombre: n, peso: p, nota: nota });` | M | Aqui se crea la evaluacion y se le asigna el id, o sea que son datos puros. |
| 5 | `localStorage.setItem('notas.v1', JSON.stringify(evals));` | M | Guardar en localStorage es persistencia, y eso le toca al Modelo a traves de un almacen. La Vista no deberia saber donde se guardan los datos. |
| 6 | `document.getElementById('nombre').value = '';` | V | Solo limpia el campo de la pantalla, no tiene reglas ni datos. |
| 7 | `'<input ... onchange="ponerNota(' + e.id + ', this.value)">'` | Mezcla de V y C | Dibuja el input en la tabla y a la vez engancha el evento con onchange, que es reaccionar al usuario, todo dentro de un texto armado a mano. |
| 8 | `nec = Math.ceil(nec * 100 - 1e-9) / 100;` | M | Es el redondeo hacia arriba de la nota necesaria, una regla de calculo que no tiene que ver con la pantalla. |
| 9 | `if (acum + 1e-9 >= 3.0) { txt = '¡Aprobado! ...'; cls = 'ok'; }` | Mezcla de M y V | Decidir que el estudiante aprobo (acum >= 3.0) es regla del negocio, pero en la misma linea se escoge el texto y la clase CSS que se ven en pantalla. |
| 10 | `document.getElementById('tabla').innerHTML = filas;` | V | Es meter el HTML armado en la tabla, dibujo puro. |
| 11 | `function borrar(id) { ...splice...; localStorage.setItem(...); pintar(); }` | Mezcla de M, V y C | La funcion reacciona al clic, que es C, borra de la lista y guarda, que seria M, y vuelve a pintar y limpia el error, que es V. Hace las tres cosas en cuatro lineas. |
| 12 | `(e.peso * e.nota / 100).toFixed(2)` | Mezcla de M y V | El calculo del aporte es peso por nota entre 100, es regla, y el toFixed(2) es solo formato para mostrarlo. Se puede argumentar que es casi todo M, pero como el formato esta pegado, lo clasificamos como mezcla. |

## 2. Olores del espagueti (Parte 1)

Lista **al menos 5** problemas de diseño de `espagueti.html`. Para cada uno, escribe qué consecuencia tiene
cuando el proyecto crece y en qué pieza de MVC viviría la solución.

| Olor | Consecuencia | Dónde se arregla (M, V o C) |
|---|---|---|
| Variables globales, son evals y sig | cualquier funcion las puede cambiar y cuesta saber quien dañó algo | M |
| La validacion de la nota esta repetida en agregar y en ponerNota | si cambiamos la regla en un lado y se nos olvida el otro, la app queda inconsistente | M |
| Valores magicos repetidos, como 3.0, 4.0, 5.0 y la clave notas.v1 | para cambiar la nota minima hay que buscar cada 3.0 y es facil dejar uno viejo | M |
| HTML armado con texto e innerHTML | lo que escribe el usuario se interpreta como HTML, por eso `<b>hola</b>` sale en negrita, y es un riesgo de seguridad | V |
| Funciones que hacen varias cosas, como agregar que valida, guarda, limpia y pinta | son largas, dificiles de leer y un cambio en una parte puede romper otra | C |

### Experimentos

**El cambio de regla.** Buscamos 3.0 con Ctrl + F en espagueti.html y aparece en tres lugares. Esta en el subtitulo de la pagina, en el calculo de la nota necesaria y en la comparacion que decide si aprobo. Si se nos olvida uno, la pantalla dice una nota minima pero el calculo usa otra, y la app se contradice sola.

**El nombre raro.** Cuando escribimos `<b>hola</b>` como nombre, la tabla mostro hola en negrita y no el texto con las etiquetas. Pasa porque pintar arma las filas con texto e innerHTML, y el navegador toma lo que escribio el usuario como si fuera HTML.

**La prueba imposible.** No se puede comprobar la regla de que los pesos sumen maximo 100 sin abrir el navegador. La regla esta dentro de agregar, mezclada con document.getElementById y err.textContent, y sin navegador eso no existe. Por eso no hay forma de llamar la regla sola.

## 3. El flujo de «agregar una evaluación» (Parte 6)

Dibuja (a mano y pega una foto, o con texto/Mermaid) el recorrido completo desde que el usuario presiona
**Agregar** hasta que ve la fila nueva en la tabla. Marca en cada paso **el archivo y el método** que actúa.

```mermaid
https://lucid.app/lucidchart/8d18ca02-5bfa-4286-824e-67abd633cdeb/edit?viewport_loc=-1016%2C-454%2C1321%2C1787%2C0_0&invitationId=inv_a09c145b-520d-4a0a-99d3-47787168b629
```

## 4. Evidencias (Partes 2, 3, 4 y 6)

**Salida de `npm test`** (pega el final, donde dice cuántas pruebas pasan):

```text
C:\Users\santi\Downloads\solucion lab\laboratorio-mvc-notas>npm test

> mvc-notas-lab@1.0.0 test
> node pruebas/ejecutar.mjs


MODELO
  ✓ A · Agregar y validar ......................... 11/11
  ✓ B · Calificar y eliminar ...................... 7/7
  ✓ C · Resumen ................................... 10/10
  ✓ D · Observador y persistencia ................. 8/8
  ✓ E · Parte 5A · Nombres únicos ................. 3/3

ARQUITECTURA
  ✓ Modelo puro ................................... 4/4
  ✓ Vista pasiva .................................. 5/5
  ✓ Controlador delgado ........................... 6/6

✓ 54 de 54 pruebas pasan.

```

**Salida de `npm run arquitectura`:**

```text
C:\Users\santi\Downloads\solucion lab\laboratorio-mvc-notas>npm run arquitectura

> mvc-notas-lab@1.0.0 arquitectura
> node pruebas/ejecutar.mjs arquitectura


ARQUITECTURA
  ✓ Modelo puro ................................... 4/4
  ✓ Vista pasiva .................................. 5/5
  ✓ Controlador delgado ........................... 6/6

✓ 15 de 15 pruebas pasan.
```

**Captura de pantalla** de la app con las dos vistas (la tabla/resumen y el termómetro): _(pega la imagen o su enlace)_

## 5. Tres cambios, tres lugares (Parte 5)

Después de cada cambio ejecuta `git diff --stat HEAD~1` (o `git show --stat`) y pega el resultado.
La idea: **cada cambio debía tocar solo las piezas que le corresponden**.

| Cambio | Archivos que tocaste | ¿Era lo esperado? ¿Por qué? |
|---|---|---|
| 5A · Nombres únicos | `src/modelo.js` | Sí, porque validar que no se repitan los nombres es una regla del negocio y debe estar en el Modelo. Se ignoran las mayúsculas y los espacios sobrantes. |
| 5B · Segunda vista (termómetro) | `src/termometro-vista.js`, `src/main.js` | Sí, porque se creó una nueva vista para mostrar el acumulado y la nota mínima para aprobar. Solo fue necesario crear la vista y conectarla con el Modelo desde `main.js`. |
| 5C · Otro almacén | `src/almacen.js`, `src/main.js` | Sí, porque se agregó `AlmacenSesion` utilizando `sessionStorage` y se seleccionó desde `main.js`. El Modelo no necesitó cambios porque el nuevo almacén mantiene los métodos `cargar()` y `guardar()`. |

## 6. Reflexión (Parte 6)

1. En la versión espagueti, ¿qué tuviste que leer o tocar para entender dónde se decide si el estudiante aprobó? ¿Y en la versión MVC?

R/ En el espagueti tuvimos que leer toda la funcion pintar, porque ahi se calcula el acumulado y se decide si aprobo, y todo esta revuelto con el codigo que dibuja la tabla. En la version MVC esa decision queda en resumen() del Modelo, en un solo lugar y sin nada de pantalla.

2. ¿Por qué el Modelo **no** debe llamar a `alert()` ni a `document`? ¿Qué pasaría con las pruebas del Modelo si lo hiciera?

R/ Porque el Modelo solo debe tener datos y reglas. Si usara alert() o document, las pruebas no se podrian correr sin navegador, y cualquier cambio en la pantalla nos obligaria a tocar el Modelo.

3. El Modelo avisa de sus cambios sin saber quién escucha (Observer). ¿Qué ganamos con eso frente a que el Modelo llame directamente a `vista.render()`?

R/ Con el Observer el Modelo no tiene que saber cuantas vistas hay. Asi podemos agregar el termometro sin cambiar el Modelo. Si llamara directo a vista.render(), quedaria amarrado a esa vista y cada vista nueva lo obligaria a cambiar.

4. Elige un olor de MVC (controlador gordo, lógica en la Vista, Modelo que toca la pantalla, Modelo anémico) que te hubiera sido fácil cometer hoy. ¿Cuál y cómo lo evitaron?

R/ El que mas facil nos pudo pasar era meter logica en la Vista, por ejemplo decidir en render si el estudiante aprobo. Lo evitamos dejando esa decision en resumen() del Modelo. La Vista solo convierte el estado que le llega en un texto y un color. Ademas, las pruebas de arquitectura revisan que la Vista no tenga reglas.

5. ¿Para qué tipo de aplicación **no** usarías MVC? Justifica con lo que viste en el laboratorio.

R/ No lo usariamos en un script pequeño o de una sola vez, porque separar en tres piezas daria mas trabajo que beneficio. Se justifica cuando la app va a crecer, tiene reglas que probar o necesita varias pantallas.



