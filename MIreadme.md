1) En app.js siempre vamos a tener esto:

- const express = require('express');
- const app = express();
- const hbs = require('hbs');

- app.set('views', __dirname + '/views');
- app.set('view engine', 'hbs');
- app.use(express.static(__dirname + '/public'));

- app.listen(3000, () => console.log("My Spotify project running on port 3000 🎧 🥁 🎸 🔊"));

2) el árbol siempre será:

public (images,stylesheets)
views (layout.hbs, ...vistas...)
app.js

3) en layout.js es super importante pone el doctype y en el body {{{body}}}
de lo contrario nada aparecerá aunque hagas rutas.
# puedes mirar cómo es en el layout de este proyecto

4) además de {{{body}}}, en el layout podemos poner cosas que queremos en todas las vistas como una nav bar etc

5) Para salvaguardar datos:

    - 5.1) Creamos un archivo .env
    - 5.2) npm i dotenv
    - 5.3) en app.js : require('dotenv').config()
    - 5.4) las cambiamos: por ejemplo CLIENTID ponemos process.env.CLIENTID

6) creamos la ruta home (/) en app.js que renderiza la vista index.hbs
   6.1) la vista index.hbs tendrá un form con un método GET o POST
   - GET -
   Si usamos el método get enviaremos por la url la búsqueda y la recuperaremos con req.query.nameDelInput

   - POST - 
   Si lo hacemos con un post recuperamos la info con req.body y habrá que usar bodyparser para 
   que podamos leer los datos.
     
# Cómo usar Body parser:
   1) npm i body-parser
   2) en app.js hay que poner:
       const bodyParser = require('body-parser');
       app.use(bodyParser.urlencoded({ extended: true }));
   3) ya podemos usar req.body!


7) creamos la ruta (/artists) en app.js y su hbs:
  - Manera 1: GET -
 Si el form de index lo hicimos con GET, entonces hacemos una ruta app.get('/artists')
 y dentro de ella usamos la llamada a la API facilitada poniendo req.query.artistInput
 en el then renderizamos artists.hbs y le pasamos los datos que hemos guardado en una variable {artists}

  - Manera 2: POST -
(Recomendada)
 Si el form de index lo hicimos con POST, entonces hacemos una ruta app.post('/artists')
 y  dentro de ella usamos la llamada a la API facilitada poniendo req.body.artistInput
 (recuerda que hay que usar bodyparser, si no da error porque los datos no se pueden leer)
  en el then renderizamos artists.hbs y le pasamos los datos que hemos guardado en una variable {artists}


8) Creamos un partial para los artistas (básicamente es una plantilla reutilizable).
# Para poder usar los partials:
  en app.js  --> hbs.registerPartials(__dirname + '/views/partials');

  - CREAR UN PARTIAL: 
Básicamente es html y donde queramos colocar la info que le pasamos por el render correspondiente,
tendremos que poner {{artists.name}} {{artists.img}} ... dependiendo del objeto que pasemos claro

9 ) Una vez creado el partial, lo usamos en la vista de artists.hbs, para ello:
- 
  {{#each artists}} <--- bucle para iterar sobre el array con todos los artistas
    {{> artistCard this }} <--- este es mi partial. No te olvides del this!!!
  {{/each}}

10) ALBUMS de la artista:
  - creamos albumCard.hbs, el partial de los albums
  - creamos albums.hbs y hacemos lo mismo que en artists.hbs con su partial correspondiente.
  - creamos la ruta en app.js con la id: 

 app.get('/albums/:artistsId', (req,res,next)=>{}

   - hacemos la llamada a la api correspondiente pasando igual que en artists los datos a la vista albums.hbs



