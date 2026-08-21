# numidia-legal

Páginas legales de la app Numidia. HTML estático, sin build.

Vive fuera del panel admin **a propósito**: el panel no debe ser público, y
alojar aquí las páginas obligaría a exponer su dominio.

## Decisiones tomadas

No quedan marcadores por rellenar. Tres cosas conviene no deshacer sin pensarlo:

**Los precios no se escriben aquí.** `terms.html` remite al precio mostrado en
la app, que es el que hace fe. Las tiendas convierten a la moneda de cada país
y aplican los impuestos locales, así que una cifra fija en euros solo sería
cierta en la eurozona — y cualquier ajuste de tarifas dejaría el documento
desalineado, que es motivo de rechazo en la revisión de Apple. Lo que Apple
exige es que el precio, la duración y el título aparezcan **en la pantalla de
compra** antes de pagar; de eso se encarga el paywall vía RevenueCat.

De referencia, los precios previstos son 4,49 €/mes y 44,90 €/año — el anual
equivale a dos meses gratis, y eso sí se menciona en el documento porque no
depende de la moneda.

**El editor se identifica sin domicilio.** El RGPD (art. 13) pide la identidad
del responsable y unos datos de contacto; un correo operativo cumple. El
domicilio del editor es particular, y las tiendas piden la dirección por su
cuenta cuando la necesitan.

**El `noindex` se queda.** El `<meta name="robots" content="noindex">` de las
cuatro páginas es una decisión, no un resto del borrador: no queremos estas
páginas en los resultados de búsqueda, y se llega a ellas desde los enlaces de
la app.

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
