# autotube-legal

Páginas legales mínimas (inicio, política de privacidad con borrado de datos y términos de uso) para las apps de autotube en Google, Meta y TikTok.

## Hooks de git (prek)

Los gestiona [prek](https://github.com/j178/prek), un sustituto de pre-commit en Rust, con la configuración en `.pre-commit-config.yaml`.

### Instalación

Una vez por máquina:

```bash
uv tool install prek==0.5.5   # deja prek.exe en ~/.local/bin, que está en el PATH
prek install --allow-missing-config   # desde cualquier worktree
```

prek se instala fuera del repo, a propósito: los hooks viven en el `.git/hooks` común a todos los worktrees y guardan la ruta del `prek` con el que se instalaron. Con `--allow-missing-config`, una rama sin `.pre-commit-config.yaml` no ejecuta nada; sin esa opción, cualquier commit en ella falla.

### Qué corre

Solo **antes de cada commit**, sobre los ficheros que van en él (menos de un segundo), con los hooks integrados de prek, sin red ni dependencias:

- `check-merge-conflict`, `check-case-conflict`, `detect-private-key` y `check-added-large-files` (hasta 500 KB).
- `trailing-whitespace`, `end-of-file-fixer` y `mixed-line-ending`. Si arreglan algo, el commit se para con «files were modified by this hook»: revisa el cambio, haz `git add` y repite el commit.

No hay hooks de pre-push: el proyecto es HTML estático, sin tests, build, lint ni CI, y no hay nada que comprobar antes de subir.

### A mano

```bash
prek run                # sobre lo que está en el índice
prek run --all-files    # sobre todo el repo
```

### Saltarse un hook

Solo en una emergencia, y diciéndolo en el commit:

```bash
PREK_SKIP=end-of-file-fixer git commit   # salta los hooks con esos ids (separados por comas)
git commit --no-verify                   # salta todos
```
