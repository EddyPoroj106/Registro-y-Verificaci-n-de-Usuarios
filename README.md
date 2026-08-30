# Proyecto: Registro de Usuarios con Verificación por Correo

Stack: **Node.js + Express + SQLite (better-sqlite3)**

## Estructura del proyecto

```
proyecto-registro/
├── src/
│   ├── db/
│   │   ├── database.js        -> Conexión y creación de la tabla en SQLite
│   │   └── usuarioModel.js    -> Funciones para crear/buscar/verificar usuarios
│   ├── routes/
│   │   └── authRoutes.js      -> Endpoints /api/registro y /api/verificar/:token
│   ├── services/
│   │   └── emailService.js    -> Envío del correo de verificación (nodemailer)
│   └── server.js              -> Arranca el servidor Express
├── public/
│   └── index.html             -> Formulario de registro
├── .env.example                -> Plantilla de configuración
├── .gitignore
├── package.json
└── README.md
```

## Diagrama de clases 

```mermaid

classDiagram

\\\&#x20;   class Usuario {

\\\&#x20;       +int id

\\\&#x20;       +string nombre

\\\&#x20;       +string email

\\\&#x20;       +string password

\\\&#x20;       +bool verificado

\\\&#x20;       +string tokenVerificacion

\\\&#x20;       +datetime tokenExpira

\\\&#x20;       +datetime creadoEn

\\\&#x20;   }



\\\&#x20;   class UsuarioRepository {

\\\&#x20;       +crearUsuario(datos) int

\\\&#x20;       +buscarPorEmail(email) Usuario

\\\&#x20;       +buscarPorToken(token) Usuario

\\\&#x20;       +marcarComoVerificado(id) void

\\\&#x20;       +listarUsuarios() Usuario\\\\\\\[]

\\\&#x20;   }



\\\&#x20;   class EmailService {

\\\&#x20;       +enviarCorreoVerificacion(destinatario, nombre, enlace) void

\\\&#x20;   }



\\\&#x20;   class AuthController {

\\\&#x20;       +registrar(req, res) void

\\\&#x20;       +verificar(req, res) void

\\\&#x20;   }



\\\&#x20;   AuthController --> UsuarioRepository : usa

\\\&#x20;   AuthController --> EmailService : usa

\\\&#x20;   UsuarioRepository --> Usuario : gestiona

```



## 

