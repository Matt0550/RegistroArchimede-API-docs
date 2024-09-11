[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![MIT License][license-shield]][license-url]
[![Discord][discord-shield]][discord-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/Matt0550/RegistroArchimede-API-docs">
    <img src="https://raw.githubusercontent.com/Matt0550/public-gaac/main/uploads/registro-archimede-logo-api-docs.png" alt="Logo" height="150">
  </a>

  <h3 align="center">Registro Archimede Unofficial API DOCS</h3>

  <p align="center">
    All the information about the endpoints of the Registro Archimede App.
    <br />
    <br />
    <a href="https://github.com/Matt0550/RegistroArchimede-API-docs/pulls">Contribute</a>
  </p>
</div>

All the information about the endpoints of the Registro Archimede App.
The endpoints was discovered using [HTTP toolkit](https://httptoolkit.com/) 
(or [MITM Proxy](https://mitmproxy.org/)) and the app on rooted Android Emulator with Android 14 (API 35). Rooted with [Magisk](https://magiskmanager.com/) to bypass certificate pinning.

This project is for educational purposes only. Please use this project responsibly. 

Copyright and all rights belong to the respective owners.

 
## API Endpoints
>[!WARNING]
> All listed JSON responses are given only when a request is successful. If a request fails the response may be in html or with an error code (not tested).
> 
### Base URL
```http
  https://app.registroarchimede.it/archimede/seam/resource/rest
```

### Authentication

#### Login/User Info
```http
  POST /loginRest/login
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `username` | `string` | **Required**. Your username |
| `password` | `string` | **Required**. Your password |
| `tokenFirebase` | `string` | (Optional) Your Firebase token (i don't know what is this) |

##### JSON Response
```json
{
  "classe": "",
  "visualizzaAlternzanzaScuolaLavoro": true,
  "cognome": "",
  "visualizzaAssenze": true,
  "visualizzaVoti": true,
  "roles": [
    {
      "role": "Alunno"
    }
  ],
  "visualizzaCertificazioneComp": true,
  "nome": "",
  "trimestre": true,
  "primaria": true,
  "giustificaFamiglia": true,
  "visualizzaCompitiArgomenti": true,
  "soloFad": true,
  "visualizzaComunicazioni": true,
  "giustificaConPin": true,
  "scuola": "",
  "sesso": "",
  "visualizzaNoteDisciplinari": true,
  "visualizzaRiepilogoAssenze": true,
  "visualizzaArgomenti": true
}
```

#### Logout
```http
  GET /loginRest/logout
```

##### JSON Response
```json
{
  "ok": "ok"
}
```

### Student Area (Alunno) (Login required)
> [!IMPORTANT]
> Before using these endpoints, you need to login with the `Login/User Info` endpoint. A cookie (`JSESSIONID`) will be set in your browser.

#### Subscription type
```http
  GET /AreaAlunno/tipoAbbonamento/{firebaseToken}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `firebaseToken` | `string` | **Required**. Your Firebase token |

##### JSON Response
```json
{
  "stato": "archimede_2024.25"
}
```

#### Absences list
```http
  GET /AreaAlunno/assenze/{period} 
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `period` | `int` | **Required**. The period of the absences (`0` or `1`) |

##### JSON Response
> Not available yet

#### Subjects list
```http
  GET /AreaAlunno/materie/
```

##### JSON Response
```json
[
  {
    "descrizione": "",
    "corso_id": 000,
    "docente": null,
    "img_docente": "base64"
  },
]
```

#### Grades list
```http
  GET /AreaAlunno/voti/{corsoId}/{period}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `corsoId` | `int` | **Required**. The course id. `-1` for all courses |
| `period` | `int` | **Required**. The period of the grades (`0` or `1`) | 

##### JSON Response
> Not available yet

#### Delays and permits list
```http
  GET /AreaAlunno/ritardiPermessi/
```

##### JSON Response
> Not available yet

#### Homework list
```http
  GET /AreaAlunno/compiti/{corsoId}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `corsoId` | `int` | **Required**. The course id. `-1` for all courses |

##### JSON Response
> Not available yet

#### Arguments list
```http
  GET /AreaAlunno/argomenti/{corsoId}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `corsoId` | `int` | **Required**. The course id. `-1` for all courses |

##### JSON Response
```json
[
  {
    "data": "yyyy-MM-dd",
    "argomenti": [
      {
        "ora": 0,
        "materia": "",
        "argomento": ""
      },
    ]
  },
]
```

#### Messages list
```http
  GET /AreaAlunno/messaggi/
```

##### JSON Response
```json
[
  {
    "oggetto": "",
    "messaggio": "",
    "mittente": "",
    "letto": true,
    "id": 0000,
    "data": "yyyy-MM-dd",
    "allegati": [
      {
        "file": "{path}",
        "nome": "{filename}",
        "id": 000,
        "size": 000
      },
    ]
  },
]
```

#### Read message
```http
  GET /AreaAlunno/leggiMessaggioFamiglia/{messageId}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `messageId` | `int` | **Required**. The message id |

##### JSON Response
```
OK
```
Probably with this endpoint the message is marked as read and the content is taken from the endpoint `/AreaAlunno/messaggi/`

#### Download attachment
```http
  GET /AreaAlunno/download/allegato/{attachmentId}/{messageId}/{filename}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `attachmentId` | `int` | **Required**. The attachment id |
| `messageId` | `int` | **Required**. The message id |
| `filename` | `string` | **Required**. The filename with extension |


#### Discipline notes list
```http
  GET /AreaAlunno/noteDisciplinari/
```

##### JSON Response
> Not available yet

#### Teacher communication list
```http
  GET /AreaAlunno/comunicazioni/
```

##### JSON Response
> Not available yet

#### Documents list
```http
  GET /AreaAlunno/documenti/{docId}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `docId` | `int` | **Required**. The document id. `-1` for all documents |

##### JSON Response
```json
[
  {
    "materia": "",
    "nome": "",
    "id": -1,
    "mittente": "",
    "tipo": "pagella",
    "pagina": ""
  }
]
```

#### Document download
```http
  GET /AreaAlunno/download/documenti/{doc}/scarica
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `doc` | `int` | **Required**. Base64 encoded `pagina` field from the document list |

#### Teachers' bulletin board
```http
  GET /AreaAlunno/bacheca/
```

##### JSON Response
```json
[
  {
    "descrizione": "",
    "corso_id": 382482,
    "docente": "",
    "img_docente": "base64"
  }
]
```

#### FAD (Formazione a Distanza) list
> [!WARNING]
> Work in progress

```http
  GET /AreaAlunno/fadPaginato/0/Tutti
```

##### JSON Response
> Not available yet

#### Video conferences list
```http
  GET /AreaAlunno/videoconferenze/{id}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id` | `int` | **Required**. The call id. `-1` for all calls |

##### JSON Response
> Not available yet

#### Received notifications list
```http
  GET /AreaAlunno/notifiche?androidDevice={deviceId}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `deviceId` | `string` | **Required**. Your device id |

##### JSON Response
```json
[
  {
    "messaggio": "",
    "data": 1726067644000
  }
]
```

### Misc

#### Register firebase token (login required)

```http
  GET /register/regFirebaseNew/{firebaseToken}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `firebaseToken` | `string` | **Required**. Your Firebase token |

##### JSON Response
```
ok
```

## Help - feedback
You can contact me on:

Discord: https://go.matteosillitti.it/discord

Telegram: https://go.matteosillitti.it/telegram

Mail: <a href="mailto:mail@matteosillitti.it">me@matteosillitti.it</a>

## License

[GNU GPLv3](https://choosealicense.com/licenses/gpl-3.0/)

## Support me

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/matt05)

[![buy-me-a-coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/Matt0550)

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://paypal.me/sillittimatteo)

[contributors-shield]: https://img.shields.io/github/contributors/Matt0550/RegistroArchimede-API-docs.svg?style=for-the-badge
[contributors-url]: https://github.com/Matt0550/RegistroArchimede-API-docs/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/Matt0550/RegistroArchimede-API-docs.svg?style=for-the-badge
[forks-url]: https://github.com/Matt0550/RegistroArchimede-API-docs/network/members
[stars-shield]: https://img.shields.io/github/stars/Matt0550/RegistroArchimede-API-docs.svg?style=for-the-badge
[stars-url]: https://github.com/Matt0550/RegistroArchimede-API-docs/stargazers
[license-shield]: https://img.shields.io/github/license/Matt0550/RegistroArchimede-API-docs.svg?style=for-the-badge
[license-url]: https://github.com/Matt0550/RegistroArchimede-API-docs/blob/master/LICENSE
[discord-shield]: https://img.shields.io/discord/828990499507404820?style=for-the-badge
[discord-url]: https://go.matteosillitti.it/discord