# GitHub Actions Demo

Proyecto de demostración de CI/CD con GitHub Actions para aplicaciones Python.

## Estructura del proyecto

```
github_actions_demo/
├── .github/
│   └── workflows/
│       └── devops.yml          # Workflow principal de CI/CD
├── hello.py                    # Aplicación Python simple
├── tests/
│   └── test_hello.py          # Tests unitarios
├── requirements.txt            # Dependencias Python
└── README.md                   # Documentación del proyecto
```

## Aplicación

`hello.py` expone tres funciones:

- `greet(name)` → saluda por nombre
- `add(a, b)` → suma dos enteros
- `is_even(n)` → indica si un número es par

```bash
python hello.py
```

## Tests

```bash
pip install -r requirements.txt
export PYTHONPATH="${PYTHONPATH}:$(pwd)"
pytest -q
```

## Workflow de CI/CD

El pipeline se ejecuta en cada `push` a `main` y en cada `pull_request`.

| Job | Descripción | Depende de |
|-----|-------------|------------|
| `test` | Instala dependencias, corre pytest, genera tag y sube artefacto | — |
| `package` | Crea zip de la aplicación con el tag del job anterior | `test` |
| `docs` | Imprime metadata del repositorio y runner | `test`, `package` |
| `deploy` | Simula despliegue a producción (solo en `main`) | `docs` |

### Artefactos generados

- **build-info**: archivo de texto con el build tag
- **app-zip**: paquete zip de la aplicación lista para despliegue
