# Arcana - Material de Programación Avanzada

Arcana es un recurso de estudio complementario para la cátedra de **Programación Avanzada de la Universidad Nacional de La Matanza**. Reúne definiciones, técnicas y resultados centrales de la materia en un único lugar pensado tanto para el estudio secuencial como para la consulta puntual.

El proyecto es mantenido por la cátedra y sus alumnos: es un documento vivo que crece con cada ciclo lectivo y se corrige cuando alguien detecta un error. Nuestro objetivo es conservar la rigurosidad y, a la vez, la claridad: estudiar algoritmos puede ser serio y disfrutable.

## Invitación técnica a colaborar

- ¿Tenés un problema interesante, una solución alternativa o un algoritmo que quieras compartir? Hacé un Pull Request. Buscamos aportes que incluyan:
  - Explicación clara del problema y su contexto.
  - Código correcto y legible (preferentemente con comentarios y ejemplos).
  - Análisis de complejidad o notas sobre variantes/edge-cases cuando aplique.

- Formato recomendado: agregá un nuevo archivo en la carpeta `content/` siguiendo la estructura del proyecto (mirá `content/about.md` como ejemplo). Si tu aporte incluye imágenes, adjuntalas en `content/attachments/`.

- **Convención de nombres en `content/bestiario/leetcode/`:**
  - Enunciado base: `NNNN_titulo_en_snake_case.md`, donde `NNNN` es el número del problema en LeetCode con cero a la izquierda hasta 4 dígitos, y el título va en minúsculas con guion bajo entre palabras.
  - Solución: `NNNN_titulo_en_snake_case-tecnica.md`, agregando un guion y un slug de técnica en kebab-case. Reutilizá los slugs ya existentes en vez de inventar uno nuevo: `fuerza-bruta`, `backtracking`, `greedy`, `programacion-dinamica`, `pd-top-down`, `pd-bottom-up`, `division-y-conquista`, `branch-and-bound`, `bfs-fuerza-bruta`, `recursivo-greedy`.
  - No incluyas "leetcode"/"LeetCode" en el nombre del archivo: ya está en la carpeta contenedora y en el tag `b/leetcode`.

- **Revisión:** los PRs serán revisados por el equipo docente y por ayudantes; se valora la corrección, claridad y el esfuerzo por documentar supuestos y límites.

### Cómo empezar

1. Forkeá este repositorio.
2. Creá una rama descriptiva: `feature/nombre-del-problema` o `fix/descripcion`.
3. Añadí tu contenido en `content/` y commiteá cambios pequeños y atómicos.
4. Abrí el Pull Request apuntando a `master` y dejá una descripción con lo que hiciste.

## Licencia y crédito

Este repositorio es mantenido por la cátedra de Programación Avanzada (UNLaM) y sus alumnos. Consultá el archivo `LICENSE.txt` para detalles sobre la licencia.

## Contacto

Si necesitás orientación sobre formato o revisión, abrí un issue o contactá a los docentes/ayudantes responsables de la materia.

¡Gracias por querer sumar! Tus PRs hacen que el recurso mejore para todos.

## Linting

Este repo incluye una regla básica de lint para archivos Markdown. Usamos `markdownlint-cli2` mediante `npx` para no forzar una dependencia global.

- Ver los errores en `content/`:

```bash
npm run lint:md
```

- Intentar arreglar automáticamente los problemas reparables:

```bash
npm run lint:md:fix
```

Si preferís revisar solo un archivo, pasa la ruta en lugar del glob, por ejemplo:

```bash
npx -y markdownlint-cli2 "content/bestiario/greedy/la-defensa-del-muro-rose.md" --config .markdownlint.json
```

La acción de GitHub Actions `.github/workflows/lint-markdown.yml` ejecuta el linter en pushes y PRs que toquen `content/**`.
