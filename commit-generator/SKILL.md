---
name: commit-generator
description: Genera o revisa mensajes de commit según Conventional Commits 1.0.0 a partir de una descripción de cambios o un diff de Git. Úsala cuando se pida redactar, corregir o validar un mensaje de commit.
---

# Generar mensajes de commit

Redacta un mensaje que describa los cambios reales y siga [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/). Si hay contexto del repositorio, respeta su idioma y sus convenciones compatibles con la especificación. No crees el commit salvo que el usuario lo solicite.

## Elegir el contenido

- Si el usuario aporta una descripción o un diff, úsalo como fuente. Si pide un mensaje para el trabajo actual, inspecciona primero los cambios preparados para commit (`git diff --cached`). Si no hay cambios preparados, revisa los cambios sin preparar y aclara que el mensaje corresponde a ellos. Incluye archivos nuevos sin seguimiento solo cuando puedas conocer su contenido.
- Resume el efecto del cambio, no solo los archivos editados. No atribuyas funciones, correcciones ni incompatibilidades que la evidencia no muestre. Si faltan datos esenciales, pide una aclaración breve.
- Usa `feat` para una funcionalidad nueva y `fix` para una corrección de error. Para otros cambios, elige un tipo descriptivo compatible con las convenciones del proyecto; por ejemplo, `docs`, `test`, `refactor`, `build`, `ci` o `chore`. Estos tipos adicionales son ejemplos, no requisitos de la especificación.
- Añade un scope entre paréntesis cuando identifique claramente una sección del código; omítelo si sería impreciso. Si los cambios independientes merecen commits separados, sugiere la división cuando sea útil.

## Formato

```text
<tipo>[scope opcional][! opcional]: <descripción breve>

[cuerpo opcional]

[pie(s) opcional(es)]
```

- El encabezado requiere tipo, dos puntos, un espacio y una descripción breve. Si hay scope, debe ser un sustantivo entre paréntesis después del tipo. Coloca `!` inmediatamente antes de `:` cuando señales un cambio incompatible.
- Usa el cuerpo para explicar contexto o consecuencias que el encabezado no cubra. Sepáralo del encabezado con una línea en blanco. Puede tener varios párrafos.
- Separa los pies del cuerpo con una línea en blanco. Cada pie comienza con un token sin espacios seguido de `: ` o ` #` y su valor; usa guiones en tokens compuestos, como `Reviewed-by`. `BREAKING CHANGE` es la excepción permitida con espacio.
- Señala cualquier cambio incompatible con `!` en el encabezado o con `BREAKING CHANGE: <explicación>` en el pie. Si usas `!`, el pie es opcional; agrégalo cuando ayude a explicar la migración. También se admite `BREAKING-CHANGE` como token de pie.
- No impongas un límite de caracteres, idioma, modo verbal o capitalización del tipo como si fueran reglas de Conventional Commits. Sigue las preferencias del proyecto cuando existan.

Entrega el mensaje listo para copiar, sin explicaciones adicionales salvo que el usuario pida alternativas, validación o justificación.
