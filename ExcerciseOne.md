# Pseudocodigo y Diagramas de flujo
---
## FrontEnd

```
INICIO

    LEER usuario.
    LEER contraseña.
        LLAMAR 
            api auth: api.com/auth
            enviar: usuario, contraseña
        Respuesta:
            SI respuesta ES Error ENTONCES
                mostrar: error
            SINO
                token ← token
                **Dar acceso a la aplicación

    ** Una vez accedido a la aplicación

    ** Ver lista de productos

        LLAMAR
            api productos: api.com/productos,
            authorization: token.
            petición: GET
        Respuesta
            SI respuesta ES Error ENTONCES
                mostrar: error
            SINO
                mostrar: productos

    ** Guardar productos
    
    LEER nombre.
    LEER categoría.
    LEER precio

    SI validar_datos(nombre, categoria, precio) ENTONCES
        LLAMAR
            api productos: api.com/productos,
            enviar: nombre, categoría, precio,
            authorization: token.
            petición: POST
        Respuesta:
            SI respuesta ES error ENTONCES
                mostrar: error
            SINO
                mostrar: respuesta
        


    FUNCION validar_datos(nombre, categoría, precio): boolean
        
        SI nombre Es Vacío
            MOSTRAR "Por Favor ingrese un nombre"
            RETORNAR false

        SI categoría NO ES [Bebidas, Aperitivos, Snacks, Dulces] ENTONCES
            MOSTRAR "Por favor ingrese una categoría valida"
            RETORNAR false

        SI precio NO ES número ENTONCES
            MOSTRAR "Por favor introduce un valor númerico".
            RETORNAR false

        RETORNAR true

    FIN_FUNCION

FIN
```

```mermaid
flowchart TD
   E1@{ shape: circle, label: "Inicio"}
   E2@{ shape: manual-input, label: "Leer Nombre usuario"}
   E3@{ shape: manual-input, label: "Leer Contraseña"}
   E4@{ shape: subproc, label: "Call Auth <br> api.com/auth <br> Enviar: Usuario y contraseña" }
   E5{ Respuesta es Error}
   E6@{ shape: lean-l, label: "Error" }
   E7(token ← respuesta.token)
   E8@{ shape: hex, label: "Ver Productos" }
   E9@{ shape: subproc, label: "Call API Produtos <br> api.com/productos <br> authorization: token <br> Petición: GET" }
   E10{respuesta == Error}
   E11@{ shape: lean-l, label: "Mostrar Error" }
   E12@{ shape: lean-l, label: "Mostrar Producto" }
   E13@{ shape: hex, label: "Agregar Productos" }
   E14@{ shape: manual-input, label: "Leer Nombre"}
   E15@{ shape: manual-input, label: "Leer Categoría"}
   E16@{ shape: manual-input, label: "Leer Precio"}
   E17{"validar_datos(nombre,categoria,precio)"}
   E18@{ shape: subproc, label: "Call API Produtos <br> api.com/productos <br> authorization: token <br> Petición: POST" }
   E19{respuesta == Error}
   E20@{ shape: lean-l, label: "Mostrar Error" }
   E21@{ shape: lean-l, label: "Mostrar Respuesta" }
   E22@{ shape: circle, label: "Fin"}
   E23@{ shape: circle, label: "Fin"}
   E24@{ shape: circle, label: "Fin"}

   E1 --> E2 --> E3 --> E4 --> E5
   E5 -- SI --> E6
   E5 -- NO --> E7
   E7 --> E8
   E8 --> E9
   E9 --> E10
   E10 -- SI --> E11
   E10 -- NO --> E12
   E7 --> E13
   E13 --> E14 --> E15 --> E16 --> E17
   E17 -- SI --> E18 --> E19
   E19 -- SI --> E20
   E19 -- NO --> E21
   E6 --> E22
   E11 --> E23
   E12 --> E23
   E20 --> E24
   E21 --> E24
```

### Funcion validar_datos(nombre,categoria,precio)

```mermaid
flowchart TD
    D1@{ shape: circle, label: "Inicio Funcion validar_datos()"}
    D2{Nombre esta Vacío}
    D3@{ shape: lean-l, label: "Por Favor ingrese un nombre" }
    D4(Return false)
    D5{"Categoria esta en [Bebidas, Aperitivos, Snacks, Dulces]"}
    D6@{ shape: lean-l, label: "Por Favor ingrese una categoria valida" }
    D7(Return false)
    D8{"Precio es número"}
    D9@{ shape: lean-l, label: "Por Favor ingrese un número en el precio" }
    D10(Return false)
    D11(Return true)
    D12@{ shape: circle, label: "Fin Funcion validar_datos()"}

    D1 --> D2
    D2 -- SI --> D3
    D3 --> D4
    D2 -- NO --> D5
    D5 -- NO --> D6
    D6 --> D7
    D5 -- SI --> D8
    D8 -- NO --> D9
    D9 --> D10
    D8 -- SI --> D11
    D4 --> D12
    D7 --> D12
    D10 --> D12
    D11 --> D12
```

---
## BackEnd

```
INICIO

    FUNCION auth(usuario, contraseña)
        LEER petición con el nombre de usuario y contraseña.
        ** se busca el usuario y se confirma la contraseña.
            SI usuario no existe ENTONCES
                RETORNAR Error "Usuario No Encontrado".
            SINO
                ** Si el usuario si se encuentra, se verifica la contraseña.
                SI contraseña coincide ENTONCES
                    RETORNAR respuesta: token.
                SINO 
                    RETORNAR Error: "Contraseña incorrecta".
    FIN_FUNCION auth



    FUNCION productos(solicitud)
    
    ** Verificar si son horas en las que la api puede ser accedida (08:00 a 19:00)
    SI NO api_abierta() ENTONCES
        RETORNAR Error "Solo Se puede acceder a desde las 08:00 hasta las 19:00"

    ** Verificar que el usuario que realiza la solicitud tenga el rol de gerente
    SI NO es_gerente(token) ENTONCES
        RETORNAR Error "No tiene los permisos necesarios para acceder"
           

    ** Si la solicitud es GET (para obtener los productos)
    SI solicitud ES GET ENTONCES
        RETORNAR productos

    
    SI solicitud ES POST ENTONCES
    
        SI validar_datos(nombre,categoria,precio) ENTONCES
            GUARDAR producto
            RETORNAR respuesta "Producto guardado correctamente"
        SINO
            RETORNAR Error "Error al crear el producto"
        

    FIN_FUNCION productos

    
    FUNCION api_abierta(): boolean
        SI hora >= 8:00 && <= 19:00
            RETORNAR true
        RETORNAR false
    FIN_FUNCION api_abierta

    
    FUNCTION es_gerente(token): boolean
        Si Usuario Rol = gerente
            RETORNAR true
        RETORNAR false
    FIN_FUNCION es_gerente


    FUNCION validar_datos(nombre, categoría, precio): boolean
        
        SI nombre Es Vacío
            MOSTRAR "Por Favor ingrese un nombre"
            RETORNAR false

        SI categoría NO ES [Bebidas, Aperitivos, Snacks, Dulces] ENTONCES
            MOSTRAR "Por favor ingrese una categoría valida"
            RETORNAR false

        SI precio NO ES número ENTONCES
            MOSTRAR "Por favor introduce un valor númerico".
            RETORNAR false

        RETORNAR true

    FIN_FUNCION validar_datos
    
FIN
```

### Función Auth
```mermaid
flowchart TD
    H1@{ shape: circle, label: "Inicio Funcion Auth" }
    H2(Leer Peticion con nombre de usuario y contraseña)
    H3{¿Usuario No existe?}
    H4(Retornar Error 'Usuario No encontrado')
    H5{¿Contraseña coincide?}
    H6(Retornar Token)
    H7(Retornar Error: 'Contraseña Incorrecta')
    H8@{ shape: circle, label: "Fin Funcion Auth" }


    H1 --> H2
    H2 --> H3
    H3 -- Si --> H4
    H3 -- No --> H5
    H5 -- Si --> H6
    H5 -- No --> H7
    H7 --> H8
    H4 --> H8
    H6 --> H8
```

### Funcion productos
```mermaid
flowchart TD
    A1@{ shape: circle, label: "Inicio Funcion productos" }
    A2{api_abierta != false}
    A3(Retornar Error 'Solo se puede acceder a la app desde las 08:00 a las 19:00')
    A4{es_gerente != false}
    A5(Retornar Error 'No tiene los permisos necesarios para acceder')
    A6(Solicitudes)
    A7{solicitud == GET}
    A8(Retornar productos)
    A9{solicitud == POST}
    A10{"validar_datos(nombre,categoria,precio)==true"}
    A11(Guardar producto)
    A12(Retornar respuesta 'Producto Guardado correctamente')
    A13(Retornar Error 'Error al crear el producto')
    A14@{ shape: circle, label: "Fin Funcion productos" }

    A1 --> A2
    A2 -- NO --> A3
    A2 -- SI --> A4
    A4 -- NO --> A5
    A4 -- SI --> A6
    A6 --> A7
    A7 -- SI --> A8
    A6 --> A9
    A9 -- SI --> A10
    A10 -- SI --> A11
    A11 --> A12
    A10 -- NO --> A13
    A13 --> A14
    A12 --> A14
    A8 --> A14
    A5 --> A14
    A3 --> A14
   
```

### Funcion api_abierta

``` mermaid
flowchart TD
    B1@{ shape: circle, label: "Inicio funcion api_abierta()"}
    B2{hora >=8 && hora <= 19}
    B3(Retornar true)
    B4(Retornar false)
    B5@{ shape: circle, label: "Fin funcion api_abierta()"}

    B1 --> B2
    B2 -- SI --> B3
    B2 -- No --> B4
    B3 --> B5
    B4 --> B5
```

### Funcion es_gerente()

```mermaid
flowchart TD
    C1@{ shape: circle, label: "Inicio funcion es_gerente()"}
    C2{Usuario Rol = gerente}
    C3(Retornar true)
    C4(Retornar false)
    C5@{ shape: circle, label: "Fin funcion es_gerente()"}

    C1 --> C2
    C2 -- SI --> C3
    C2 -- NO --> C4
    C3 --> C5
    C4 --> C5
```

### Funcion validar_datos(nombre,categoria,precio)

```mermaid
flowchart TD
    D1@{ shape: circle, label: "Inicio Funcion validar_datos()"}
    D2{Nombre esta Vacío}
    D3@{ shape: lean-l, label: "Por Favor ingrese un nombre" }
    D4(Return false)
    D5{"Categoria esta en [Bebidas, Aperitivos, Snacks, Dulces]"}
    D6@{ shape: lean-l, label: "Por Favor ingrese una categoria valida" }
    D7(Return false)
    D8{"Precio es número"}
    D9@{ shape: lean-l, label: "Por Favor ingrese un número en el precio" }
    D10(Return false)
    D11(Return true)
    D12@{ shape: circle, label: "Fin Funcion validar_datos()"}

    D1 --> D2
    D2 -- SI --> D3
    D3 --> D4
    D2 -- NO --> D5
    D5 -- NO --> D6
    D6 --> D7
    D5 -- SI --> D8
    D8 -- NO --> D9
    D9 --> D10
    D8 -- SI --> D11
    D4 --> D12
    D7 --> D12
    D10 --> D12
    D11 --> D12
```

