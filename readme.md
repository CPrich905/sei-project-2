# General Assembly SEI Project 2: MovieBored
## Introduction
The second project of the course was to pair-code a React application that consumed a public API.

We chose to recreate a movie database website.


## Team
Shane - [shaneshibu · GitHub](https://github.com/shaneshibu) <br />
Charles - [CPrich905 · GitHub](https://github.com/CPrich905)

## Technologies used
* HTML5
* SCSS and Bulma
* JavaScript (ECMAScript6)
* React.js
* Git
* GitHub
* The Movie Database API

## Dependencies
* Axios

## Timeframe
2 Days (May 2019)

## Deployment
The project is currently deployed at https://tmdb2.herokuapp.com/

## Overview
Having decided to use the API from themoviedb.org, we set about recreating an IMDB-type website where users could navigate to a movie page by searching for its name. Our intention was to create a simple layout showing cast, crew, reviews, similar films and associated media.

This website is essentially made up of two parts:
 - Searching for movies
 - Displaying a movie

### Searching for a movie

Each time the value of the input field changes a search function is executed. The function makes a GET request to the TMDB API using the input string as a search parameter. The JSON response is then saved as a variable in the React state object and displayed as search results on the page.

```javascript
searchMoviesOrTv() {

  const query = this.state.query

  if (query.trim().length!==0) {

    axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}`)
      .then(res => this.setState({ movies: res.data.results }))
      .catch(err => console.log(err))
  }
}
```

![tmdb2 Main Screen](src/assets/searchmovie.gif)

### Displaying a movie
From the search results page, when a user clicks on a movie, they are taken to that particular movie's page. On the movie page, a GET request is made to the API for each section that needs to be populated: the hero, cast, reviews, media and recommendation sections. The responses from these requests are saved in the state object and then rendered to the page.  

![tmdb2 Movie Page](src/assets/displaymovie.gif)

## Wins
- Dynamically changing the background image of the Hero div
Setting the initial image was relatively simple, we just set the css style of backgroundImage equal to the url of the first image of that movies background images array. However if multiple images were included with the API return, we also wanted to showcase these. For films with multiple images, we used to code below to take the provided array of images from the API and cycle through them, adding a time out to smooth the change over (see code below).

![setBackground function](./dist/assets/readme/setbackground.png?raw=true "Title")

## Challenges
 - The one major challenge we faced was working with multiple axios Get requests. Having to work with several asynchronous functions delayed loading times which often led to problems displaying items on the page when the variables that contained those items where still empty. We got around this problem by using conditional rendering. This way JSX elements that needed data from the API would only be rendered after the request had been completed and the data stored in the React state.

```javascript
render() {
  return(

    <section>
      <div className="hero is-dark is-fullheight-with-navbar is-paddingless">
        {this.state.heroLoaded &&
          <MoviesHero movie={this.state.movie}/>}
      </div>
      <hr />
      <div className="content">
  )
}
```
In this case the MoviesHero component would only render if the heroLoaded object was a truthy value, i.e. not equal to null.

- Displaying the cast  
While cast and recommended movies were provided by our API, not all of the cast profiles or recommended films had an associated image. Without these images, the formatting for the scroll bars we wished to use became irregular, necessitating the use of a placeholder image.
Both cast and recommendations were built using a functional component with cast and recommendations passed from the main show page. The class component for the cast is shown below.

![Alt text](./dist/assets/readme/cast.png?raw=true "Title")


## Future development
- **Cast Pages**  
While each movie page does include cast and crew information, MovieBored doesn't include individual actor pages. We didn't have time to include these within the time time limit, but if we were to improve this project, this is the first place we would start.
- **Search for TV Shows**  
While we completed our Minimum Viable Product of a films database, the TMDB API also had the capability to search TV listings. We did start to implement this as we completed our original targets ahead of schedule, however we didn't manage to complete it.

- **User Profiles**
If we were to continue this project, subsequent features could include an option for users to create a profile, saving data such as favourite actors and films. However, this would require a back-end which was outside the scope of the original assignment.
