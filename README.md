# Green · Generador de presentaciones OpEx/DMAIC

Convierte un JSON (generado por el agente de Glean) en el .pptx completo del
tollgate OpEx, usando el template `Simplicity_Template_OpEx_DA_2026.pptx`.

## Estructura

```
template/generate_pptx.py.pptx   <- template original (no tocar)
scripts/generate_pptx.py         <- motor de llenado (python-pptx)
schema/EJEMPLO.json              <- ejemplo de JSON válido, ya probado
.github/workflows/generate.yml   <- Action: recibe el JSON, genera el pptx, publica un Release
docs/index.html                  <- wizard page (GitHub Pages) para pegar el JSON y generar
GLEAN_AGENT_PROMPT.md            <- prompt del agente de Glean que genera el JSON
```

## Puesta en marcha (una sola vez)

1. Crea el repo en GitHub (puede ser privado) y sube este contenido.
2. Settings → Pages → Source: **Deploy from branch**, branch `main`, carpeta `/docs`.
   Te va a quedar una URL tipo `https://tuusuario.github.io/green-opex/`.
3. Settings → Actions → General → Workflow permissions: **Read and write permissions**
   (para que el Action pueda publicar el Release).
4. Genera un Personal Access Token (Settings de tu cuenta → Developer settings →
   Fine-grained tokens) con permisos **Contents: read/write** y **Actions: read/write**
   sobre este repo.
5. Abre la wizard page, pon `owner/repo` y el token (se guardan solo en tu navegador).

## Uso día a día

1. Le pides al agente de Glean que arme la presentación (ver `GLEAN_AGENT_PROMPT.md`);
   te regresa un JSON.
2. Pegas ese JSON en la wizard page y das clic en **Crear presentación**.
3. En ~30-60 segundos te da un botón de descarga con el .pptx ya listo.

## Probar localmente sin GitHub Actions

```bash
pip install python-pptx
python scripts/generate_pptx.py schema/EJEMPLO.json output/prueba.pptx
```

## Límites conocidos

- El glosario (slide 1) se reemplaza como texto simple; pierde el acomodo en
  columnas del template original si el agente manda muchos términos.
- Las imágenes de "antes/después" en la slide 5 y las gráficas de la slide 7
  se generaron manualmente en el proyecto de referencia; el JSON solo controla
  los textos y valores, no reemplaza imágenes. Si un proyecto nuevo necesita
  gráficas distintas, esas se ajustan a mano después de generar el .pptx.
- El checklist de la slide 8 usa siempre los mismos 7 criterios del template
  (Standard Tool, Access Control, Automated Feed, etc.) — el JSON solo llena
  Y/N y comentarios.
