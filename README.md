# numidia-legal

Páginas legales de la app Numidia. HTML estático, sin build.

Vive fuera del panel admin **a propósito**: el panel no debe ser público, y
alojar aquí las páginas obligaría a exponer su dominio.

## Antes de publicar

Busca y reemplaza en los tres HTML:

| Marcador | Qué poner |
|----------|-----------|
| `[DATE]` | Fecha de la última revisión |
| `[NOM DE L'ÉDITEUR]` | Nombre legal de quien publica la app |
| `[ADRESSE]` | Domicilio del editor |
| `[EMAIL DE CONTACT]` | Correo de contacto para asuntos de datos |
| `[PAYS]` | País cuya ley aplica |
| `[PRIX]` | Precios reales, **idénticos** a los de Play Console y App Store |

Y borra los bloques `<div class="todo">` de cada página.

> Los precios de `terms.html` tienen que coincidir exactamente con los de las
> tiendas. Una discrepancia ahí es motivo de rechazo en la revisión de Apple.

El `<meta name="robots" content="noindex">` de las cuatro páginas **se queda**.
Es una decisión, no un resto del borrador: no queremos estas páginas en los
resultados de búsqueda, y se llega a ellas desde los enlaces de la app.

No bloquea ningún trámite. `noindex` solo afecta a la indexación, no a la
descarga: Play Console, AdMob y App Store Connect leen la página igual. Lo que
sí la bloquearía es un `robots.txt` con `Disallow`, así que no lo añadas.

## Publicar

**Vercel** (proyecto nuevo, distinto del panel):

```bash
npx vercel --prod
```

**GitHub Pages**: repositorio público, Settings → Pages → rama `main`, carpeta raíz.

Cualquiera de los dos sirve: son ficheros estáticos sin dependencias.

## Después de publicar

Las URLs van a tres sitios:

```bash
# 1. La app
eas env:create --scope project --environment production --visibility plaintext --name EXPO_PUBLIC_PRIVACY_URL --value "https://.../privacy.html"
eas env:create --scope project --environment production --visibility plaintext --name EXPO_PUBLIC_TERMS_URL --value "https://.../terms.html"
eas env:create --scope project --environment production --visibility plaintext --name EXPO_PUBLIC_DELETE_ACCOUNT_URL --value "https://.../delete-account.html"
```

2. **Play Console** → ficha de la app → política de privacidad, y el formulario
   de Data Safety, que pide además la URL de borrado de cuenta.

3. **AdMob** → Privacy & messaging → el mensaje de GDPR pide la URL de la
   política de privacidad.

Sin las variables del paso 1, el paywall no muestra los enlaces legales y
**Apple rechaza la app**. `app.config.ts` avisa de ello al compilar en producción.

## Otros idiomas

La app está en francés, inglés, español y árabe. Estas páginas solo están en
francés, que es suficiente legalmente para el mercado principal. Para añadir
otros idiomas, duplica los HTML en subcarpetas (`en/`, `es/`, `ar/`) y enlaza
entre ellas desde el `<nav>`.
