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

\\\\\\\&#x20;   class Usuario {

\\\\\\\&#x20;       +int id

\\\\\\\&#x20;       +string nombre

\\\\\\\&#x20;       +string email

\\\\\\\&#x20;       +string password

\\\\\\\&#x20;       +bool verificado

\\\\\\\&#x20;       +string tokenVerificacion

\\\\\\\&#x20;       +datetime tokenExpira

\\\\\\\&#x20;       +datetime creadoEn

\\\\\\\&#x20;   }



\\\\\\\&#x20;   class UsuarioRepository {

\\\\\\\&#x20;       +crearUsuario(datos) int

\\\\\\\&#x20;       +buscarPorEmail(email) Usuario

\\\\\\\&#x20;       +buscarPorToken(token) Usuario

\\\\\\\&#x20;       +marcarComoVerificado(id) void

\\\\\\\&#x20;       +listarUsuarios() Usuario\\\\\\\\\\\\\\\[]

\\\\\\\&#x20;   }



\\\\\\\&#x20;   class EmailService {

\\\\\\\&#x20;       +enviarCorreoVerificacion(destinatario, nombre, enlace) void

\\\\\\\&#x20;   }



\\\\\\\&#x20;   class AuthController {

\\\\\\\&#x20;       +registrar(req, res) void

\\\\\\\&#x20;       +verificar(req, res) void

\\\\\\\&#x20;   }



\\\\\\\&#x20;   AuthController --> UsuarioRepository : usa

\\\\\\\&#x20;   AuthController --> EmailService : usa

\\\\\\\&#x20;   UsuarioRepository --> Usuario : gestiona

```



\## Instalación y ejecución



1\. Instalar dependencias:

```

&#x20;  npm install

```



2\. Copiar el archivo de configuración:

```

&#x20;  cp .env.example .env

```

&#x20;  (En Windows: `copy .env.example .env`)



3\. Configurar el envío de correos en `.env`. Ejemplo con Gmail:

&#x20;  - Activa la verificación en 2 pasos en la cuenta de Gmail que uses para enviar correos: https://myaccount.google.com/security

&#x20;  - Genera una contraseña de aplicación en: https://myaccount.google.com/apppasswords

&#x20;  - Completa el `.env`:

```

&#x20;    SMTP\_HOST=smtp.gmail.com

&#x20;    SMTP\_PORT=587

&#x20;    SMTP\_USER=tucorreo@gmail.com

&#x20;    SMTP\_PASS=la\_contraseña\_de\_aplicacion

&#x20;    SMTP\_FROM=tucorreo@gmail.com

```



&#x20;  > \*\*Nota:\*\* si no configuras el `.env`, el proyecto sigue funcionando igual: el enlace de verificación se imprime en la consola del servidor en lugar de enviarse por correo real, así se puede probar el flujo completo sin necesidad de credenciales SMTP.



4\. Levantar el servidor:

```

&#x20;  npm start

```



5\. Abrir en el navegador: `http://localhost:3000`

&#x20;  - Llenar el formulario y registrar un usuario.

&#x20;  - Revisar el correo (o la consola del servidor si no se configuró SMTP) para obtener el enlace de verificación.

&#x20;  - Hacer clic en el enlace para verificar la cuenta.



6\. Confirmar que quedó verificado entrando a: `http://localhost:3000/api/usuarios`



> \*\*Nota sobre el correo:\*\* los mensajes de verificación pueden llegar a la carpeta de \*\*Spam / Correo no deseado\*\*, especialmente si la cuenta de Gmail configurada es nueva o recién empezó a enviar correos automáticos. Revisar ahí si no aparece en la bandeja de entrada.



\## Variables de entorno



| Variable      | Descripción                                              |

|---------------|-----------------------------------------------------------|

| `PORT`        | Puerto donde corre el servidor (por defecto 3000)         |

| `BASE\_URL`    | URL base usada para construir el enlace de verificación   |

| `SMTP\_HOST`   | Servidor SMTP (ej. `smtp.gmail.com`)                       |

| `SMTP\_PORT`   | Puerto SMTP (587 para TLS, 465 para SSL)                   |

| `SMTP\_USER`   | Correo remitente                                           |

| `SMTP\_PASS`   | Contraseña de aplicación del correo remitente              |

| `SMTP\_FROM`   | Correo que aparece como remitente en el mensaje enviado    |



El archivo `.env` \*\*no se sube al repositorio\*\* (está en `.gitignore`) por seguridad. Cada persona que descargue el proyecto debe crear su propio `.env` a partir de `.env.example`.



## 

