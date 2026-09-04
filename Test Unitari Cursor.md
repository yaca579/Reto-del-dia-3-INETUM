import unittest

from ejemplos_refactorizacion import (
_formatear_datos_usuario,
_normalizar_valor,
calcular_total,
calcular_total_original,
generar_resumen,
generar_resumen_original,
obtener_etiqueta,
obtener_etiqueta_original,
)

class TestCalcularTotal(unittest.TestCase):
def test_calcula_total_con_impuesto(self):
self.assertAlmostEqual(calcular_total(20, 3, 0.21), 72.6)

def test_sin_impuesto_devuelve_solo_subtotal(self):
    self.assertEqual(calcular_total(10, 2, 0), 20)

def test_cantidad_cero_devuelve_cero(self):
    self.assertEqual(calcular_total(99, 0, 0.21), 0)

def test_precio_cero_devuelve_cero(self):
    self.assertEqual(calcular_total(0, 5, 0.21), 0)

def test_valores_decimales(self):
    self.assertAlmostEqual(calcular_total(15.5, 4, 0.1), 68.2)

def test_refactor_conserva_resultado_original(self):
    casos = [
        (10, 2, 0),
        (15.5, 4, 0.1),
        (100, 1, 0.21),
        (20, 3, 0.21),
        (0, 10, 0.5),
        (8.99, 0, 0.21),
    ]
    for precio_unitario, cantidad, tasa_impuesto in casos:
        with self.subTest(caso=(precio_unitario, cantidad, tasa_impuesto)):
            self.assertEqual(
                calcular_total(precio_unitario, cantidad, tasa_impuesto),
                calcular_total_original(
                    precio_unitario, cantidad, tasa_impuesto
                ),
            )


class TestNormalizarValor(unittest.TestCase):
def test_aplica_transformacion_y_strip(self):
self.assertEqual(_normalizar_valor(" ANA ", "fallback", str.lower), "ana")

def test_usa_predeterminado_si_queda_vacio(self):
    self.assertEqual(_normalizar_valor("   ", "Sin nombre", str.title), "Sin nombre")

def test_usa_str_por_defecto(self):
    self.assertEqual(_normalizar_valor("  555-0100 ", "Sin telefono"), "555-0100")


class TestFormatearDatosUsuario(unittest.TestCase):
def test_normaliza_nombre_correo_y_telefono(self):
usuario = {
"nombre": " ana garcia ",
"correo": " ANA@EJEMPLO.COM ",
"telefono": " 555-0100 ",
}
self.assertEqual(
_formatear_datos_usuario(usuario),
("Ana Garcia", "ana@ejemplo.com", "555-0100"),
)

def test_aplica_valores_predeterminados(self):
    usuario = {"nombre": " ", "correo": "", "telefono": "   "}
    self.assertEqual(
        _formatear_datos_usuario(usuario),
        ("Sin nombre", "Sin correo", "Sin telefono"),
    )


class TestGenerarResumen(unittest.TestCase):
def test_normaliza_datos_del_usuario(self):
usuario = {
"nombre": " ana garcia ",
"correo": " ANA@EJEMPLO.COM ",
"telefono": " 555-0100 ",
}
esperado = (
"Nombre: Ana Garcia\n"
"Correo: ana@ejemplo.com\n"
"Telefono: 555-0100"
)
self.assertEqual(generar_resumen(usuario), esperado)

def test_usa_valores_predeterminados(self):
    usuario = {"nombre": " ", "correo": "", "telefono": "   "}
    esperado = (
        "Nombre: Sin nombre\n"
        "Correo: Sin correo\n"
        "Telefono: Sin telefono"
    )
    self.assertEqual(generar_resumen(usuario), esperado)

def test_falla_si_faltan_claves(self):
    with self.assertRaises(KeyError):
        generar_resumen({"nombre": "Ana"})

def test_refactor_conserva_resultado_original(self):
    casos = [
        {
            "nombre": " Luis Perez ",
            "correo": " LUIS@EJEMPLO.COM ",
            "telefono": " 555-0101 ",
        },
        {"nombre": " ", "correo": "", "telefono": "   "},
        {
            "nombre": "  ana garcia ",
            "correo": " ANA@EJEMPLO.COM ",
            "telefono": " 555-0100 ",
        },
    ]
    for usuario in casos:
        with self.subTest(usuario=usuario):
            self.assertEqual(
                generar_resumen(usuario),
                generar_resumen_original(usuario),
            )


class TestObtenerEtiqueta(unittest.TestCase):
def test_devuelve_etiquetas_para_todas_las_combinaciones(self):
casos = [
(25, True, True, "miembro adulto"),
(18, True, True, "miembro adulto"),
(17, True, True, "miembro menor"),
(0, True, True, "miembro menor"),
(25, False, True, "usuario activo"),
(17, False, True, "usuario activo"),
(25, True, False, "usuario inactivo"),
(17, True, False, "usuario inactivo"),
(25, False, False, "usuario inactivo"),
]
for edad, tiene_membresia, esta_activo, esperado in casos:
with self.subTest(caso=(edad, tiene_membresia, esta_activo)):
self.assertEqual(
obtener_etiqueta(edad, tiene_membresia, esta_activo),
esperado,
)

def test_inactivo_tiene_prioridad_sobre_membresia_y_edad(self):
    self.assertEqual(obtener_etiqueta(40, True, False), "usuario inactivo")

def test_refactor_conserva_resultado_original(self):
    casos = [
        (0, False, False),
        (17, True, True),
        (18, True, True),
        (17, False, True),
        (99, False, True),
        (25, True, False),
        (12, False, False),
    ]
    for edad, tiene_membresia, esta_activo in casos:
        with self.subTest(caso=(edad, tiene_membresia, esta_activo)):
            self.assertEqual(
                obtener_etiqueta(edad, tiene_membresia, esta_activo),
                obtener_etiqueta_original(
                    edad, tiene_membresia, esta_activo
                ),
            )


if name == "main":
unittest.main()
