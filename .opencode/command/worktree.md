---
description: Crea un git worktree bajo .worktrees/ derivando el nombre del argumento.
---

El usuario invocó `/worktree` con el siguiente argumento (puede contener espacios o no):

$ARGUMENTS

Tu única tarea es:

1. Derivar un nombre de worktree a partir del argumento aplicando estas reglas, en orden:
   - Pasar a minúsculas.
   - Reemplazar cualquier espacio y guion bajo por guion.
   - Eliminar todo carácter que no sea `[a-z0-9-]`.
   - Colapsar guiones repetidos y recortar guiones al inicio/fin.
   - Si el resultado queda vacío, preguntar al usuario con la herramienta `question`.
2. Ejecutar **exactamente** este comando usando el tool `bash` con el parámetro
   `workdir` apuntando a la raíz del proyecto (NO uses `cd` en el comando):

   git worktree add .worktrees/<nombre-derivado>

3. Reportar en una sola línea el resultado (worktree creado en `.worktrees/<nombre>` o
   el error que git haya devuelto).
4. No hagas nada más: no cambies de directorio, no ejecutes comandos adicionales,
   no edites archivos, no hagas commits, no respondas con análisis extra.
