# Analizador Léxico en HTML y JavaScript

## Descripción General

Este proyecto implementa un analizador léxico interactivo, desarrollado en HTML y JavaScript puro, capaz de identificar dos tipos de tokens fundamentales en el análisis de lenguajes de programación:

- **Identificadores**: Secuencias que inician con una letra y pueden contener letras o dígitos (ejemplo: `variable1`, `x2`).
- **Números reales**: Secuencias numéricas con punto decimal, siguiendo el formato `entero.entero+` (ejemplo: `12.34`, `0.5`).

El objetivo es mostrar de manera didáctica y visual cómo funciona un analizador léxico, facilitando la comprensión de los procesos de reconocimiento de tokens y el flujo de análisis.

---

## Características

- **Interfaz web amigable**: Solo necesitas abrir el archivo `index.html` en tu navegador.
- **Análisis en tiempo real**: Ingresa cualquier texto y obtén el análisis léxico al instante.
- **Tablas explicativas**:
  - **Tabla de Tokens**: Muestra cada lexema reconocido y su tipo.
  - **Análisis Paso a Paso**: Explica cada acción tomada por el analizador sobre el texto.
- **Código claro y documentado**: Fácil de entender y modificar para adaptarlo a otros lenguajes o reglas.

---

## ¿Cómo funciona el analizador?

1. **Entrada del usuario**: El usuario escribe una cadena de texto en el área de texto.
2. **Recorrido carácter por carácter**: El analizador recorre el texto, identificando espacios, identificadores, números reales y caracteres desconocidos.
3. **Reconocimiento de tokens**:
   - Si encuentra una letra, reconoce un identificador.
   - Si encuentra un dígito seguido de un punto y más dígitos, reconoce un número real.
   - Si el patrón no coincide, lo marca como desconocido o lo ignora.
4. **Visualización**: Se muestran dos tablas con los resultados y el proceso de análisis.

---

## Ejemplo de uso

Introduce el siguiente texto en el área de análisis:

```
abc 12.34 x1 0.5 var123 7. 45.67 _error 9.8.7
```

**Resultados esperados:**
- Identificadores: `abc`, `x1`, `var123`
- Números reales: `12.34`, `0.5`, `45.67`
- Desconocidos o ignorados: `7.`, `_error`, `9.8.7`

---

## Estructura del código

El núcleo del analizador está en la función `analizar()` dentro de `index.html`. A continuación, se explica paso a paso:

```js
function analizar() {
    const texto = document.getElementById('input').value;
    const tokens = [];
    const tabla = [];
    let i = 0;
    const pasos = [];
    while (i < texto.length) {
        let c = texto[i];
        if (/[ \t\n]/.test(c)) {
            pasos.push([i, c, 'Ignorar espacio']);
            i++;
            continue;
        }
        // Identificador: letra(letra|digito)*
        if (/[a-zA-Z]/.test(c)) {
            let start = i;
            i++;
            while (i < texto.length && /[a-zA-Z0-9]/.test(texto[i])) i++;
            let lexema = texto.slice(start, i);
            tokens.push({ tipo: 'Identificador', valor: lexema });
            tabla.push([lexema, 'Identificador']);
            pasos.push([start, lexema, 'Reconoce Identificador']);
            continue;
        }
        // Real: entero.entero+
        if (/[0-9]/.test(c)) {
            let start = i;
            while (i < texto.length && /[0-9]/.test(texto[i])) i++;
            if (texto[i] === '.' && /[0-9]/.test(texto[i+1])) {
                i++;
                let decStart = i;
                while (i < texto.length && /[0-9]/.test(texto[i])) i++;
                if (i > decStart) {
                    let lexema = texto.slice(start, i);
                    tokens.push({ tipo: 'Real', valor: lexema });
                    tabla.push([lexema, 'Real']);
                    pasos.push([start, lexema, 'Reconoce Real']);
                    continue;
                }
            }
            pasos.push([start, texto.slice(start, i), 'No es real, ignorar']);
            i = start + 1;
            continue;
        }
        // Otro carácter
        tabla.push([c, 'Desconocido']);
        pasos.push([i, c, 'Desconocido']);
        i++;
    }
    // ...
}
```

### Explicación paso a paso
- **Ignorar espacios**: Si el carácter es espacio, tabulación o salto de línea, simplemente avanza.
- **Identificadores**: Si encuentra una letra, consume todas las letras y dígitos siguientes y lo reconoce como identificador.
- **Números reales**: Si encuentra un número seguido de punto y más números, lo reconoce como real.
- **Desconocidos**: Cualquier otro carácter se marca como desconocido.

---

## ¿Por qué es útil para empresas?

- **Demuestra habilidades en análisis léxico y desarrollo web**.
- **Código limpio, comentado y portable**.
- **Ideal para pruebas, enseñanza o como base para proyectos más complejos**.
- **Fácil de extender para otros lenguajes o reglas léxicas**.

---

## Cómo ejecutar

1. Descarga o clona este repositorio.
2. Abre el archivo `index.html` en tu navegador favorito.
3. Ingresa el texto a analizar y presiona "Analizar".

---

## Autor

Proyecto realizado para la materia: Seminario de Solución de Problemas de Traductores de Lenguajes II.

Puedes usar, modificar y compartir este código libremente.

---

¿Tienes dudas o sugerencias? ¡Abre un issue o contacta al autor!
