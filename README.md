Este proyecto esta basado en la solucion de un test donde se require listar la informacion de peliculas
y a su ves poder consultar el detalle de alguna pelicula.
Para poder probarlo:
- Clonar el proyecto
- Para los datos del login usar el nombre de una pelicula y el password sera el valor del imdbID
para obterner estos datos se debe ingresar a https://omdbapi.com/ y realizar una busqueda de cualqueir pelicula
y luego usar para el inicio del login el dato title para el user y para el password imdbID
por ejemplo:

resultado de la busqueda de la pelicula blade
{"Title":"BS High","Year":"2023","Rated":"TV-MA","Released":"23 Aug 2023","Runtime":"95 min","Genre":"Documentary","Director":"Travon Free, Martin Desmond Roe","Writer":"N/A","Actors":"Korey Coleman, Joe Maimone, Quincy Talmadge","Plot":"Follows the investigation which occurred when the Bishop Sycamore Centurions, a presumed high school football team from Columbus, Ohio, took on perennial prep powerhouse, IMG Academy.","Language":"English","Country":"United States","Awards":"1 nomination","Poster":"https://m.media-amazon.com/images/M/MV5BMDQxNDhkZjQtZTFlOS00MGViLWEzY2UtOTg2NTRiOTFmNGM1XkEyXkFqcGdeQXVyMTAyMjQ3NzQ1._V1_SX300.jpg","Ratings":[{"Source":"Internet Movie Database","Value":"7.2/10"}],"Metascore":"N/A","imdbRating":"7.2","imdbVotes":"1,408","imdbID":"tt21929748","Type":"movie","DVD":"N/A","BoxOffice":"N/A","Production":"N/A","Website":"N/A","Response":"True"}

user = blade
password = tt0120611

luego de ingresar verificar la funcionalidad del home donde tendremos una lista de peliculas y podremos ver el detalle y tambien realizar un busqueda basandonos en el nombre de la pelicula, se puede navegar usando la paginacion