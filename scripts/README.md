# Firmas STC (`generate_firmas.py`)

Este directorio contiene la **plantilla** de la firma HTML, el **Excel de datos** y el script que genera un `firma.html` por persona en `../02-firmas/<carpeta>/`.

## Archivos

| Archivo | Uso |
|--------|-----|
| `datos.xlsx` | Datos de cada persona (primera hoja; ver columnas abajo). |
| `02-firmas/kenji-kawaida/firma.html` | **Plantilla** con marcadores `{{PHOTO_SRC}}`, `{{LOGO_SRC}}`, `{{FULL_NAME}}`, `{{CARGO}}`, `{{KV_ROWS}}`, `{{SOCIAL_BUTTONS}}`. Edita el HTML aquí; no copies este archivo tal cual al correo (faltan datos). La salida generada para Kenji va en la carpeta del repo `../02-firmas/kenji-kawaida/firma.html`. |
| `generate_firmas.py` | Rellena la plantilla y escribe `02-firmas/.../firma.html`. |

Las imágenes remotas apuntan a jsDelivr (`kenjikv/stc-signature`): fotos en `02-firmas/<slug>/foto.jpg`, logo e iconos en `01-iconos/`.

## Requisitos

- Python 3.10 o superior recomendado.
- Paquete **openpyxl** (lectura del `.xlsx`).

## Cómo ejecutar el generador

Desde la **raíz del repositorio** (recomendado):

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install openpyxl
python3 scripts/generate_firmas.py
```

Por defecto usa:

- Excel: `scripts/datos.xlsx`
- Plantilla: `scripts/02-firmas/kenji-kawaida/firma.html`

### Argumentos opcionales

```text
python3 scripts/generate_firmas.py [ruta/al/datos.xlsx] [ruta/a/plantilla-firma.html]
```

Ejemplo con rutas explícitas:

```bash
python3 scripts/generate_firmas.py scripts/datos.xlsx scripts/02-firmas/kenji-kawaida/firma.html
```

La salida se escribe en `02-firmas/<slug>/firma.html` (por ejemplo `02-firmas/kenji-kawaida/firma.html`). El mapeo **nombre completo → carpeta (`slug`)** está en `SLUG_BY_NAME` dentro de `generate_firmas.py`; si agregas filas nuevas en el Excel, debes añadir también la carpeta correspondiente y el slug en ese diccionario.

### Columnas esperadas en `datos.xlsx` (fila 1)

`Nombre y apellido`, `Cargo`, `Linkedin`, `Facebook`, `X`, `Instagram`, `Medium/Otra red social`, `Telefono`, `Correo electronico`, `Página web propia`, `Página web de la comunidad`.

En redes, `-` o `X` (sin URL) se omiten. Los teléfonos de 8 dígitos se formatean como `+591 XXX XXXXX`.

## Cómo pegar la firma en el correo (Gmail, Outlook, etc.)

1. Genera las firmas con el script (paso anterior).
2. Abre en el **navegador** el archivo generado, por ejemplo `02-firmas/kenji-kawaida/firma.html` (doble clic o *Arrastrar al Chrome/Firefox/Safari*).
3. En la página, **selecciona todo** el contenido visible de la firma:
   - **Windows / Linux:** `Ctrl + A`
   - **macOS:** `Cmd + A`
4. **Copia** (`Ctrl + C` / `Cmd + C`).
5. En tu gestor de correo, abre **Configuración → Firma** (o equivalente) y **pega** en el editor de firma.

Así el cliente suele conservar mejor tablas e imágenes que si pegas el código HTML en bruto. Si algo se desalinea en un cliente concreto, revisa que las URLs de las imágenes ya estén publicadas en GitHub (jsDelivr tarda un poco en actualizar la caché).

## Plantilla (`02-firmas/kenji-kawaida/firma.html` dentro de `scripts/`)

No borres los marcadores `{{...}}`. El script los sustituye por:

- URLs de foto y logo (`PHOTO_SRC`, `LOGO_SRC`).
- Texto escapado para HTML (`FULL_NAME`, `CARGO`).
- Bloques HTML generados (`KV_ROWS`, `SOCIAL_BUTTONS`).

Tras cambiar la plantilla o el Excel, vuelve a ejecutar `generate_firmas.py` para regenerar todas las salidas en `02-firmas/`.
