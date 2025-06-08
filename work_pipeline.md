# Pipeline de trabajo

El primer paso del proyecto consistió en analizar los códigos QR disponibles, con el objetivo de identificar los distintos problemas que podríamos enfrentar durante el procesamiento. A partir de este análisis, buscamos una librería de Python que nos permitiera detectar códigos QR. Encontramos y utilizamos **Pyzbar**, una herramienta que permite decodificar códigos QR de forma sencilla.

Una vez implementada esta herramienta, evaluamos cuántos códigos eran legibles directamente y cuántos no. Observamos que aproximadamente la mitad de los códigos QR no podían ser reconocidos de forma directa. Por ello, nos enfocamos en trabajar sobre esas imágenes *no legibles*, aplicándoles distintos filtros, transformaciones y operaciones de procesamiento para evaluar si era factible restaurar su legibilidad.

Tras obtener algunos resultados prometedores, decidimos escalar el enfoque desarrollando una clase `Pipeline` que nos permitiera aplicar una serie de funciones de manera sistemática y modular sobre cada imagen. Además, definimos una función encargada de verificar si un código QR había sido correctamente leído, así como una función que escanea una carpeta de imágenes `.png`, detecta los códigos QR en cada una y los asocia con archivos `.txt` de nombre correspondiente ubicados en otra carpeta.

Este sistema nos permitió automatizar el proceso y evaluar de forma más eficiente el impacto de distintos pipelines sobre grandes volúmenes de imágenes.

Durante las pruebas, notamos que uno de los enfoques más efectivos consistía en aplicar una conversión a escala de grises. Esto tiene sentido, ya que muchos códigos QR presentaban colores y variaciones tonales que dificultaban su lectura. 
Como los QRs solo requieren blanco y negro, la umbralización también resultó ser muy efectiva.

Mediante conversión a escala de grises logramos reconocer **90 imágenes**. Luego, aplicando umbralización con diferentes umbrales, conseguimos recuperar hasta **100 códigos QR**, lo que representa una mejora significativa, considerando que la propia librería Pyzbar ya realiza un preprocesamiento interno. Estos resultados demostraron que nuestro procesamiento adicional aportaba valor.

A continuación, intentamos optimizar aún más el rendimiento implementando pipelines más complejos que incluían:

- Operaciones aritméticas sobre la imagen,
- Transformaciones morfológicas,
- Umbralización basada en histogramas,
- Filtros en el dominio de Fourier,
- Distintas combinaciones entre ellas,
- Y otras más

Sin embargo, estas técnicas no lograron superar significativamente los resultados obtenidos con los métodos más simples. También intentamos clasificar las imágenes en función de la suma de sus histogramas (comparando imágenes reconocidas y no reconocidas), pero no encontramos diferencias sustanciales que justificaran una segmentación basada en ese criterio.

## Resultados Finales

- **90 imágenes reconocidas** aplicando conversión a escala de grises.
- **75 imágenes reconocidas** mediante pipelines personalizados.
- **30 imágenes reconocidas** aplicando umbralización de fuerza bruta.

