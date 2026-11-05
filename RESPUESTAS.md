# Preguntas para discutir en clase · Dolores Hoy

1. **¿Por qué `--dart-define` no basta para una clave que cuesta dinero?** Porque la clave se compila DENTRO del programa; quien descompile o inspeccione el APK puede encontrarla. Para claves de pago se usa un proxy en tu propio servidor (Nivel 4): la app llama a tu servidor y este llama a la API con la clave guardada ahí.
2. **Un compañero sube `secrets.json` a GitHub, ¿qué hace, en orden?**
   1) Revocar la clave de inmediato y generar una nueva (borrar el archivo no basta, queda en el historial).
   2) Poner la clave nueva solo en su `secrets.json` local.
   3) Asegurar que `.gitignore` incluya `secrets.json` y quitarlo del repo (`git rm --cached secrets.json`).
   4) Limpiar el historial si se puede (git filter-repo / BFG) y revisar si hubo uso indebido de la clave vieja.
3. **¿Ventaja de que todas las APIs pasen por `ClienteSeguro`?** Las defensas (https obligatorio, timeout, límite de tamaño, JSON validado, errores traducidos, User-Agent) se escriben y se prueban una sola vez; ninguna API nueva se puede "olvidar" de protegerse.
4. **¿Por qué validar la respuesta si "la NASA es confiable"?** Porque puede cambiar su formato, fallar, ser suplantada en una red insegura o devolver HTML de un Wi-Fi público; una URL rara (`http://`, `javascript:`) o un video en lugar de imagen rompería o dañaría la app. Validar la salida es tan importante como validar la entrada.
5. **Si Wikipedia cambia el campo `extract`:** el usuario ve "El servidor respondió con datos que no entiendo." (RespuestaInvalidaException), no un crash. Las pruebas con MockClient usan respuestas fijas, así que no se enteran solas: avisaría primero abrir la URL real en el navegador, y al actualizar `jsonWiki` en `ayudas.dart` fallaría la prueba "pide el resumen del tema con guiones bajos".

## Reto 7.9 (cambiar Open-Meteo a http://)
Si en `ServicioClima` usas `Uri.http(...)` en lugar de `Uri.https(...)`, falla **"convierte la respuesta en Clima"** (y la del tablero) con `ConexionInseguraException`: "Por seguridad solo se permiten direcciones https://". Déjalo en `Uri.https`.

## Retos de código incluidos
- Ronda 6: caja de texto para buscar un tema en Wikipedia (`pantalla_tablero.dart`, `buscarWiki` en el ViewModel y 2 pruebas nuevas). Con 100 letras ves: "Dato inválido: tema demasiado largo".

## Retos que quedan de tu lado
- Ronda 1: registrar tu clave gratuita en https://api.nasa.gov y ponerla en `secrets.json` (nunca en un chat ni en un doc compartido).
- 7.7: la auditoría del APK con tu clave de prueba.
