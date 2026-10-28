# Práctica SDD: Desarrollo asistido por IA con GitHub Spec Kit

> **Máster de FP en Inteligencia Artificial y Big Data**  
> Metodología investigada: **GitHub Spec Kit**  
> Proyecto desarrollado: **Bookify**

---

## Autores
* **Díaz Plaza, Míriam**
* **Jiménez Muñoz, Álvaro**
* **Bellón Payer, David**

---

## Enlaces del proyecto

| Recurso | Enlace | Descripción |
| :--- | :--- | :--- |
| **Repositorio de código** | [github.com/MiriamDP/tracking_books_sdd](https://github.com/MiriamDP/tracking_books_sdd) | Código fuente, tests y especificaciones en `specs/` |
| **Presentación en Canva** | [canva.link/jiung27g8rxlw27](https://canva.link/jiung27g8rxlw27) | Diapositivas utilizadas en la exposición |
| **Presentación en PDF** | [Presentación_GitHub_Spec.pdf](Presentación_GitHub_Spec.pdf) | Copia local en PDF de la presentación |
| **Despliegue Vercel (rama main)** | [book-tracking-main.vercel.app](https://book-tracking-main.vercel.app) | Versión inicial (formulario en dos columnas) |
| **Despliegue Vercel (rama feature)** | [book-tracking-creation-page.vercel.app](https://book-tracking-creation-page.vercel.app) | Versión con cambio de requisitos (página dedicada `#/books/new`) |

---

## Sobre esta práctica

En esta práctica hemos investigado y aplicado la metodología **Spec-Driven Development (SDD)** utilizando la herramienta **GitHub Spec Kit**. 

El objetivo principal no ha sido generar código rápido mediante prompts improvisados, sino aprender a dirigir y supervisar el desarrollo asistido por IA a través de especificaciones formales, comprobando cómo responde el flujo de trabajo tanto en la creación inicial de una aplicación como al gestionar un cambio de requisitos posterior.

---

## Qué es Spec-Driven Development (SDD)

Cuando trabajamos con asistentes de IA mediante lenguaje natural libre, es habitual encontrarse con problemas como alucinaciones en librerías, pérdida de contexto en proyectos medianos o código generado sin documentación ni criterios claros de verificación.

SDD plantea un enfoque estructurado: **la especificación es la fuente de verdad**. El desarrollador no se dedica a teclear código directamente, sino a definir requisitos, clarificar dudas técnicas con la IA, validar planes de arquitectura y revisar que cada tarea cumpla las condiciones pactadas antes de darla por buena.

---

## La herramienta: GitHub Spec Kit

[GitHub Spec Kit](https://github.com/github/spec-kit) es un framework open source diseñado para guiar a modelos de IA (como GitHub Copilot, Claude o Gemini) mediante un flujo de trabajo por fases y artefactos markdown versionados en el propio repositorio Git.

### Instalación e inicialización
La herramienta se apoya en Python (>=3.11) y en el gestor `uv`:

```bash
# Instalación de la CLI de Spec Kit
uv tool install specify-cli

# Inicializar un proyecto con soporte para Copilot
specify init nombre_proyecto --integration copilot
```

### Flujo de trabajo en 9 fases

El proceso sigue esta secuencia:

```text
[1. Constitution] ➔ [2. Specify] ➔ [3. Clarify] ➔ [4. Plan] ➔ [5. Tasks]
                                                                       │
[9. Iterate] ◄─── [8. Validate] ◄─── [7. Implement] ◄─── [6. Analyze] ◄┘
```

1. **Constitution:** Fijamos las reglas y restricciones técnicas de la aplicación (por ejemplo: desarrollo en Vanilla JS, sin backend y persistencia local en `localStorage`).
2. **Specify:** Redactamos las historias de usuario y los criterios de aceptación en formato Given-When-Then.
3. **Clarify:** La IA nos plantea dudas sobre casos límite o detalles sin definir (como tamaños máximos de imagen o formatos permitidos) y las resolvemos.
4. **Plan:** La IA genera la arquitectura técnica, contratos de interfaz y modelos de datos.
5. **Tasks:** Se desglosa el plan en una lista ordenada de tareas técnicas independientes.
6. **Analyze:** Se analiza la coherencia entre especificación, plan y tareas **antes de programar**, detectando vacíos o contradicciones.
7. **Implement:** La IA va desarrollando el código tarea por tarea según lo planificado.
8. **Validate:** Ejecutamos la suite de tests automatizados para validar que se cumplen los requisitos.
9. **Iterate:** Ciclo continuo para refinar la aplicación o incorporar nuevas funcionalidades.

---

## Aplicación práctica: Bookify y gestión del cambio

Como caso de estudio desarrollamos **Bookify**, un gestor web de lecturas personales para catalogar libros pendientes y leídos, guardar portadas y actualizar su estado sin necesidad de backend.

Para evaluar la metodología en una situación realista, dividimos el desarrollo en dos etapas:

1. **Versión inicial (rama `main`):** Desarrollamos el MVP básico siguiendo las especificaciones de `specs/001-book-reading-tracker/`. En esta versión, la interfaz se organizó en dos columnas: el formulario para dar de alta libros a la izquierda y el listado de lectura a la derecha.
2. **Gestión del cambio de requisitos (rama `feature/pagina-creacion-libro`):** Planteamos el requisito de desacoplar el formulario de la vista principal y moverlo a una página dedicada (`#/books/new`), dejando la biblioteca a ancho completo. En lugar de tocar el código directamente, creamos una nueva especificación con Spec Kit (`specs/002-book-creation-page/`), dejando que la IA replanificara las tareas y ejecutara la migración sin alterar el modelo de datos existente.

Ambas versiones se encuentran desplegadas de forma independiente en Vercel para comprobar el cambio visual y funcional directamente en el navegador.

---

## Dificultades encontradas durante el proyecto

* **Configuración inicial en Windows:** La integración de `specify-cli` con PowerShell y las rutas de extensiones en VS Code requirió ajustar permisos de ejecución y variables de entorno.
* **Detección de 6 inconsistencias en la fase Analyze:** Este fue uno de los mayores aciertos del método. Antes de escribir una sola línea de código, la fase de análisis nos avisó de discrepancias en el límite de tamaño de las portadas y de dependencias no contempladas, lo que nos evitó tener que refactorizar más tarde.
* **Ámbito de trabajo en subcarpetas:** Al ubicar el código dentro de `book-tracking-app/`, tuvimos que acotar el contexto en las reglas iniciales para que la IA no generase archivos en la raíz por error.
* **Despliegue en Vercel:** El proyecto utilizaba un script local `server.js` para desarrollo. Al subirlo a Vercel, la plataforma intentaba tratarlo como función de servidor en lugar de web estática; lo solucionamos creando un script de build (`build.js`) que compila a `dist/` para servirse directamente desde CDN.

---

## Valoración del equipo

* **Lo que más nos ha gustado:** El nivel de control que aporta sobre el modelo de IA. Evita que la IA invente librerías o altere la arquitectura por su cuenta, y deja toda la documentación técnica generada y sincronizada con el código.
* **Aspectos a mejorar o limitaciones:** Para proyectos muy pequeños supone una sobrecarga de configuración inicial notable. Además, requiere que el desarrollador preste atención en las fases de análisis para no validar especificaciones con contradicciones.
* **Conclusión:** Consideramos que SDD es un marco de trabajo muy potente para entornos profesionales donde se quiera utilizar IA de forma predecible, ordenada y mantenible en equipo.

---

## Presentación de la práctica

La presentación utilizada para la exposición en clase resume los puntos clave de la metodología y la experiencia con Bookify:
* **Presentación online en Canva:** [Ver presentación](https://canva.link/jiung27g8rxlw27)
* **Archivo PDF:** Disponible en este mismo repositorio como [`Presentación_GitHub_Spec.pdf`](Presentación_GitHub_Spec.pdf).

> *Para ver las capturas de pantalla de la interfaz y las instrucciones detalladas de ejecución del código, visita el [repositorio de la aplicación](https://github.com/MiriamDP/tracking_books_sdd).*

---

## Bibliografía y recursos utilizados

1. **GitHub Spec Kit (repositorio oficial):** [https://github.com/github/spec-kit](https://github.com/github/spec-kit)
2. **Documentación oficial de Spec Kit:** [https://github.github.com/spec-kit/](https://github.github.com/spec-kit/)
3. **Presentación de la práctica (Canva):** [https://canva.link/jiung27g8rxlw27](https://canva.link/jiung27g8rxlw27)
4. **Repositorio de código de Bookify:** [https://github.com/MiriamDP/tracking_books_sdd](https://github.com/MiriamDP/tracking_books_sdd)
5. **Herramientas de IA empleadas:** GitHub Copilot, Anthropic Claude 3.5 Sonnet y Google Gemini en Antigravity IDE.
6. **Plataforma de despliegue:** [Vercel Documentation](https://vercel.com/docs)
