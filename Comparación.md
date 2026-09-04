 Comparación entre código refactorizado y tests unitarios:

1. calcular_total

+ Refactor : mantiene la fórmula (subtotal + impuesto).

- Tests : cubren casos típicos y comparan con la versión original.

  * no hay tests para cantidades o tasas negativas ni validación de tipos.

1. generar_resumen + helpers

+ Refactor : extrae _normalizar_valor y _formatear_datos_usuario;.

- Tests : verifican ejemplos, defaults y paridad con el original.

  * el cambio de orden entre transformaciones (strip vs title/lower) no está probado.


1. obtener_etiqueta

- Lógica reescrita con guard clauses; comportamiento probado y comparado con el original.


Conclusión y recomendaciones: Los tests ejercitan bien el camino fácil, pero faltan pruebas de bordes (valores negativos, tipos, y casos que detecten la diferencia de orden en la normalización). Añadir esas pruebas lo solucionará.
