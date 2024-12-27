# Case 1

> Usuario: Pablo
> Password: PabloPass
> Rol: Gerente
> Hora: 13:35
> Producto: Dr. Pepper
> Categoria: Bebidas
> Precio: 12.50

---
## FrontEnd

```mermaid
flowchart TD
   E1@{ shape: circle, label: "Inicio"}
   E2@{ shape: manual-input, label: "Leer Nombre usuario"}
   A@{ shape: comment, label: "Pablo" }
   E3@{ shape: manual-input, label: "Leer Contraseña"}
   B@{ shape: comment, label: "PabloPass" }
   E4@{ shape: subproc, label: "Call Auth <br> api.com/auth <br> Enviar: Usuario y contraseña" }
   C@{ shape: comment, label: "token: XXXXXX" }
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
   D@{ shape: comment, label: "Dr. Pepper" }
   E15@{ shape: manual-input, label: "Leer Categoría"}
   E@{ shape: comment, label: "Bebidas" }
   E16@{ shape: manual-input, label: "Leer Precio"}
   F@{ shape: comment, label: "12.50" }
   E17{"validar_datos(nombre,categoria,precio)"}
   E18@{ shape: subproc, label: "Call API Produtos <br> api.com/productos <br> authorization: token <br> Petición: POST" }
   G@{ shape: comment, label: "Authorization: XXXXXX <br> Nombre: Dr. Pepper <br> Categoria: Bebidas <br> Precio: 12.50" }
   E19{respuesta == Error}
   E20@{ shape: lean-l, label: "Mostrar Error" }
   E21@{ shape: lean-l, label: "Mostrar Respuesta" }
   H@{ shape: comment, label: "Respuesta: Producto guardado correctamente <br> Nombre: Dr. Pepper <br> Categoria: Bebidas <br> Precio: 12.50 " }
   E22@{ shape: circle, label: "Fin"}
   E23@{ shape: circle, label: "Fin"}
   E24@{ shape: circle, label: "Fin"}

   E1 ==> E2 ==> E3 ==> E4 ==> E5
   E5 -- SI --> E6
   E5 == NO ==> E7
   E7 --> E8
   E8 --> E9
   E9 --> E10
   E10 -- SI --> E11
   E10 -- NO --> E12
   E7 ==> E13
   E13 ==> E14 ==> E15 ==> E16 ==> E17
   E17 == SI ==> E18 ==> E19
   E19 -- SI --> E20
   E19 == NO ==> E21
   E6 --> E22
   E11 --> E23
   E12 --> E23
   E20 --> E24
   E21 ==> E24

   E1 ~~~ A
   E2 ~~~ B
   E4 ~~~ C
   E13 ~~~ D
   E14 ~~~ E
   E15 ~~~ F
   E17 ~~~ G
   E19 ~~~ H

   style E1 fill:#32CD32
   style E2 fill:#32CD32
   style E3 fill:#32CD32
   style E4 fill:#32CD32
   style E5 fill:#32CD32
   style E7 fill:#32CD32
   style E13 fill:#32CD32
   style E14 fill:#32CD32
   style E15 fill:#32CD32
   style E16 fill:#32CD32
   style E17 fill:#32CD32
   style E18 fill:#32CD32
   style E19 fill:#32CD32
   style E21 fill:#32CD32
   style E24 fill:#32CD32
```

### Function validar_datos(nombre,categoria,precio)

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

    D1 ==> D2
    D2 -- SI --> D3
    D3 --> D4
    D2 == NO ==> D5
    D5 -- NO --> D6
    D6 --> D7
    D5 == SI ==> D8
    D8 -- NO --> D9
    D9 --> D10
    D8 == SI ==> D11
    D4 --> D12
    D7 --> D12
    D10 --> D12
    D11 ==> D12

    A@{ shape: comment, label: "Nombre: Dr. Pepper<br>Categoria: Bebidas<br>Precio: 12.50" }
    B@{ shape: comment, label: "Nombre: Dr. Pepper" }
    D1 ~~~ B

    style D1 fill:#32CD32
    style D2 fill:#32CD32
    style D5 fill:#32CD32
    style D8 fill:#32CD32
    style D11 fill:#32CD32
    style D12 fill:#32CD32
```

---
## BackEnd

### Function Auth
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


    H1 ==> H2
    H2 ==> H3
    H3 -- Si --> H4
    H3 == No ==> H5
    H5 == Si ==> H6
    H5 -- No --> H7
    H7 --> H8
    H4 --> H8
    H6 ==> H8

    A@{ shape: comment, label: "Username: Pablo<br>Password: PabloPass" }
    H1 ~~~ A

    style H1 fill:#32CD32
    style H2 fill:#32CD32
    style H3 fill:#32CD32
    style H5 fill:#32CD32
    style H6 fill:#32CD32
    style H8 fill:#32CD32

```

### Function productos
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

    A1 ==> A2
    A2 -- NO --> A3
    A2 == SI ==> A4
    A4 -- NO --> A5
    A4 == SI ==> A6
    A6 --> A7
    A7 -- SI --> A8
    A6 ==> A9
    A9 == SI ==> A10
    A10 == SI ==> A11
    A11 ==> A12
    A10 -- NO --> A13
    A13 --> A14
    A12 ==> A14
    A8 --> A14
    A5 --> A14
    A3 --> A14

    style A1 fill:#32CD32
    style A2 fill:#32CD32
    style A4 fill:#32CD32
    style A6 fill:#32CD32
    style A9 fill:#32CD32
    style A10 fill:#32CD32
    style A11 fill:#32CD32
    style A12 fill:#32CD32
    style A14 fill:#32CD32
```

### Funcion api_abierta

``` mermaid
flowchart TD
    B1@{ shape: circle, label: "Inicio funcion api_abierta()"}
    A@{ shape: comment, label: "Hora: 13:35"}
    B2{hora >=8 && hora <= 19}
    B3(Retornar true)
    B4(Retornar false)
    B5@{ shape: circle, label: "Fin funcion api_abierta()"}

    B1 ==> B2
    B2 == SI ==> B3
    B2 -- No --> B4
    B3 ==> B5
    B4 --> B5

    style B1 fill:#32CD32
    style B2 fill:#32CD32
    style B3 fill:#32CD32
    style B5 fill:#32CD32

```

### Funcion es_gerente()

```mermaid
flowchart TD
    C1@{ shape: circle, label: "Inicio funcion es_gerente()"}
    C2{Usuario Rol = gerente}
    C3(Retornar true)
    C4(Retornar false)
    C5@{ shape: circle, label: "Fin funcion es_gerente()"}

    C1 ==> C2
    C2 == SI ==> C3
    C2 -- NO --> C4
    C3 ==> C5
    C4 --> C5

    style C1 fill:#32CD32
    style C2 fill:#32CD32
    style C3 fill:#32CD32
    style C5 fill:#32CD32
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

    D1 ==> D2
    D2 -- SI --> D3
    D3 --> D4
    D2 == NO ==> D5
    D5 -- NO --> D6
    D6 --> D7
    D5 == SI ==> D8
    D8 -- NO --> D9
    D9 --> D10
    D8 == SI ==> D11
    D4 --> D12
    D7 --> D12
    D10 --> D12
    D11 ==> D12

    style D1 fill:#32CD32
    style D2 fill:#32CD32
    style D5 fill:#32CD32
    style D8 fill:#32CD32
    style D11 fill:#32CD32
    style D12 fill:#32CD32
```

