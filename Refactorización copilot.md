#1 nombres de variables mas descriptivos
def calcular_total_original(p, d, t):
subtotal = p * d
return subtotal + subtotal * t

#Refactorización 1

def calcular_total(precio_unitario, cantidad, tasa_impuesto):
subtotal = precio_unitario * cantidad
impuesto = subtotal * tasa_impuesto
return subtotal + impuesto

#2 extraccion de metodos

def generar_resumen_original(usuario):
nombre = usuario["nombre"].strip().title()
correo = usuario["correo"].strip().lower()
telefono = usuario["telefono"].strip()

if not nombre:
    nombre = "Sin nombre"
if not correo:
    correo = "Sin correo"
if not telefono:
    telefono = "Sin telefono"

return f"Nombre: {nombre}\nCorreo: {correo}\nTelefono: {telefono}"


#Refactorización 2

def _normalizar_valor(valor, valor_predeterminado, transformar=str):
valor_normalizado = transformar(valor).strip()
return valor_normalizado or valor_predeterminado

def _formatear_datos_usuario(usuario):
nombre = _normalizar_valor(usuario["nombre"], "Sin nombre", str.title)
correo = _normalizar_valor(usuario["correo"], "Sin correo", str.lower)
telefono = _normalizar_valor(usuario["telefono"], "Sin telefono")
return nombre, correo, telefono

def generar_resumen(usuario):
nombre, correo, telefono = _formatear_datos_usuario(usuario)
return f"Nombre: {nombre}\nCorreo: {correo}\nTelefono: {telefono}"

#3 simplificacion de condicionales

def obtener_etiqueta_original(edad, tiene_membresia, esta_activo):
if esta_activo:
if tiene_membresia:
if edad >= 18:
return "miembro adulto"
else:
return "miembro menor"
else:
return "usuario activo"
else:
return "usuario inactivo"

#Refactorización 3

def obtener_etiqueta(edad, tiene_membresia, esta_activo):
if not esta_activo:
return "usuario inactivo"
if not tiene_membresia:
return "usuario activo"
return "miembro adulto" if edad >= 18 else "miembro menor"

if name == "main":
print(calcular_total(20, 3, 0.21))
print(generar_resumen({
"nombre": " ana garcia ",
"correo": " ANA@EJEMPLO.COM ",
"telefono": " 555-0100 ",
}))
print(obtener_etiqueta(25, True, True))
