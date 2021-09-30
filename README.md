# Movies SPA de busqueda de peliculas

## *[Deploy - Ver en web](https://movies-search-nine.vercel.app/)*

Es una Single Page Aplication, hecha con React y Redux, en la que se puede:

<li>Buscar peliculas o juegos en el input , busca por cada carácter que se introduce.
<li>Guarda en Favoritos, las películas al presionar el botón favoritos.
<li>Elimina de favoritos, con botón de eliminar o simplemente con volver a tocar el boton favoritos.
<li>Muestra el detalle de la película, con hacer click en la caratula.

La tome para aplicar conocimientos en React Redux, es una aplicación que nos dan como ejercicio en *[Henry](https://www.soyhenry.com)*, le hice modificaciones particulares para diferenciarla y montarla en mi portfolio.
Para verla en funcionamiento es tan simple con hacer click *[aqui](https://movies-sand.vercel.app/
)*


# Esto que sigue es un Instructivo de como implementar redux que seguire acomodando.

# Redux OMDB APP

## Instalar React Redux
```js
npm install redux react-redux --save
//Si se van a hacer consultas alguna API hace falta installar
npm install --save redux-thunk
```

### Root reducer el reducer

  ##Crear archivo index.js combine reducers
```js
import { combineReducers } from "redux";

const rootReducers = combineReducers({
    //user: userReducer,
});

export default rootReducers;
```
  
### Crear store
  ## En carpeta Store, un index

```js
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk'
import rootReducer from './reducers';

//Dev tools
const composeEnhancers = (typeof window !== 'undefined' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__) || compose;

//Store
const store = createStore(
    rootReducer,
    composeEnhancers(applyMiddleware(thunk))
);

export default store;
```
  
### Conectamos la store a la app,en archivo index

```js
import store from './store';
import { Provider } from 'react-redux';

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
);
```

  
##Crear reduccer
const initialState = {
    moviesFavorite: JSON.parse(localStorage.getItem('movies')) || [],
    moviesLoaded: [],
    movieDetail: {}
  };

```js
function <nombre-reducer>Reducer(state = initialState, action) {
  if (action.type === "ADD_MOVIE_FAVORITE") {
      return {
        ...state,
        movies: state.movies.concat(action.payload)
      }
  }
  if (action.type === "GET_MOVIES") {
      return {
        ...state,
        moviesLoaded: action.payload.Search
      };
  }
  return state;
}

export default rootReducer;
```
  

  
  
### Comenzamos por actions

```js
export function addMovieFavorite(payload) {
  return { type: "ADD_MOVIE_FAVORITE", payload };
}

export function getMovies(titulo) {
  return function(dispatch) {
    return fetch("http://www.omdbapi.com/?apikey=20dac387&s=" + titulo)
      .then(response => response.json())
      .then(json => {
        dispatch({ type: "GET_MOVIES", payload: json });
      });
  };
}
```

