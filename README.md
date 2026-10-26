# Explorador de Google Drive

Módulo en Python que monta Google Drive (Google Colab), explora un directorio
configurado mediante constantes y procesa archivos de datos `.json` y `.csv`.
Devuelve un diccionario estructurado, listo para ser consumido por una API.

## Características

1. **Filtrado selectivo:** solo lista archivos `.json` y `.csv` (configurable con `EXTENSIONES_PERMITIDAS`).
2. **Metadatos:** tamaño en KB (`os.path.getsize`) y fecha de última modificación (`os.path.getmtime`).
3. **Parseo estructurado:** si existe `inventario.json`, lo lee con `json.load()` y valida que sea una lista de registros (objetos JSON).
4. **Retorno modular:** `list_files_in_directory()` retorna un `dict` en memoria.
5. **Manejo de excepciones:** directorio inexistente, ruta que no es carpeta, falta de permisos y JSON malformado.

## Estructura

```
.
├── explorador_archivo.py
├── README.md
├── LICENSE
└── .gitignore
```

## Uso en Google Colab

```python
from explorador_archivo import montar_drive, list_files_in_directory, DIRECTORIO

montar_drive()                            
resultado = list_files_in_directory(DIRECTORIO)
```

También se puede ejecutar como script: `python explorador_drive.py`.

### Constantes de configuración

| Constante | Valor por defecto |
|---|---|
| `DIRECTORIO` | `/content/drive/My Drive/` |
| `EXTENSIONES_PERMITIDAS` | `('.json', '.csv')` |
| `ARCHIVO_INVENTARIO` | `InventarioColab` |

## Estructura del diccionario retornado

```json
{
  "directorio": "/content/drive/My Drive/",
  "existe": true,
  "total_archivos": 2,
  "archivos": [
    {"nombre": "datos.csv", "extension": ".csv", "tamano_kb": 0.01,
     "ultima_modificacion": "2026-10-25 14:55:53"}
  ],
  "inventario": {
    "encontrado": true, "valido": true, "total_registros": 2,
    "registros": [{"id": 1, "producto": "Lapiz", "stock": 10}],
    "mensaje": "Lista válida con 2 registro(s)."
  },
  "error": null
}
```

## Pruebas

Salida real con una carpeta de ejemplo (`datos.csv`, `inventario.json`, `notas.txt`;
el `.txt` es excluido por el filtro):

```
Archivos .json, .csv en /ruta/t1:
 └── datos.csv  |  0.01 KB  |  modificado: 2026-10-25 14:55:53
 └── inventario.json  |  0.08 KB  |  modificado: 2026-10-25 14:55:53
Total: 2 archivo(s).

inventario.json: VÁLIDO - Lista válida con 2 registro(s).
```

Inventario con estructura incorrecta:

```
inventario.json: INVÁLIDO - Se esperaba una lista y se obtuvo dict.
```

Directorio inexistente:

```
Directorio no encontrado.
```

## Licencia

Distribuido bajo licencia MIT. Ver [LICENSE](LICENSE).
