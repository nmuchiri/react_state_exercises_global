You should already have the application from the previous exercise. If not, fork and clone the [react-film repo](../../../react-film/).

If you didn't follow along from the previous assignment, you can jump ahead to today's starter code by switching to the `components-and-props` branch.

You can run your app with `npm install && npm run start`.

## Your Mission

Our goal today is to add some events to our app. We'll keep the events simple for now - each event will simply print a message to the console.
- In the future, however, we'd like to be able to add films to our list of favorites, filter the films to see only our favorites, and see the details for a specific film. Our work today will make that possible.

![](images/film-2.png)

### Tasks - Part 1: Adding Favorites

#### Step 1: Add a new `Fave` component

Create a new component called `Fave` that will eventually handle whether a movie is a user's favorite. The `Fave` component's `render` method should return the following:

```html
<div className="film-row-fave add_to_queue">
  <p className="material-icons">add_to_queue</p>
</div>
```

Now, let's render this. In the `FilmRow` component, underneath the `film-summary` `div`, render the `Fave` component.

In your browser, the icon should appear at the bottom right corner of each film row.

#### Step 2: Define a `handleClick` function in `Fave`

Inside the `Fave` component, define a function called `handleClick`. The function should accept an event (`e`) as an argument. Simply log out a message like `"handling Fave click!"` for now.

Since you aren't using this anywhere, nothing should change.

#### Step 3: Add an `onClick` in `Fave`

Now that you have a function that handles the user clicking a movie, connect it to the UI. In the `div` of `Fave`'s `render` function, add a parameter of `onClick={this.handleClick}`.

In your browser's JavaScript console, you should see the message `handleClick` prints out when the `div` is clicked.

That's all! Our click is not yet adding favorites, but it is working. Later, we will modify our app so that when the Fave icon is clicked, our app adds or removes the selected movie from the user's favorites array.

### Tasks - Part 2: Handling Filter Toggling

Eventually, we'll want an "ALL" heading and a "FAVES" heading that are clickable links - when the user clicks "ALL", the left sidebar will show all movies; when the user clicks "FAVES", the left sidebar will show only their favorite movies.

#### Step 1: Define a `handleFilterClick` function in `FilmListing`

Let's first set up the function that will determine what movies are shown in the list. We'll need to be able to tell if we are showing all the movies or filtering down to just some of the movies.

In `FilmListing`, create a `handleFilterClick` function that takes a string `filter` as an argument. For now, just print a message that says `Setting filter to ` and the `filter` argument.

This new function isn't connected to a button in the UI yet, so nothing should change.

#### Step 2: Add provided markup to display the ALL/FAVES menu

Let's add some markup to the `FilmListing` component so that we can have something worth clicking. We'll keep the Films heading; underneath it, we'll add two categories of "ALL" and "FAVES". We're also setting up displaying the film's length, which we aren't using yet.

Change the `FilmListing` component to render this:

```html
<div className="film-list">
    <h1 className="section-title">FILMS</h1>
    <div className="film-list-filters">
        <div className="film-list-filter">
            ALL
            <span className="section-count">{this.props.films.length}</span>
        </div>
        <div className="film-list-filter">
            FAVES
            <span className="section-count">0</span>
        </div>
    </div>

    {allFilms}
</div>
```

If you check your browser, these subheadings should appear in the left column.

#### Step 3: Add `onClick` inside `FilmListing` to trigger filtering to `'faves'`

Now we have an ALL section and a FAVES section - we can hook that filtering function we just created up to it.

Add an `onClick` inside `FilmListing` so that when "FAVES" is clicked, it calls the `handleFilterClick` method you created with the parameter `'faves'`.

**Hint**: This will look like this:
`onClick={() => this.handleFilterClick('faves')}`

Try clicking FAVES - does it print to the console?

#### Step 4: Add `onClick` inside `FilmListing` to trigger filtering to `'all'`

Now FAVES is clickable, so the next step is to make ALL clickable as well.

Add an `onClick` inside `FilmListing` so that when "ALL" is clicked, it calls the `handleFilterClick` method with argument `'all'`.

Now, you should see a message in the console when we click either option. Eventually, instead of viewing a message, clicking either option will display the correct list of movies to the user.

### Tasks - Part 3: Handling Film Details

We aren't yet going to be creating the large detailed view of the film that will be displayed on the right column, but we're going to start setting up for it.

#### Step 1: Define a function `handleDetailsClick` inside `FilmRow`

Inside `FilmRow`, define a function called `handleDetailsClick`. The function should accept a `film` as an argument. Print out `Fetching details for ` and the film title to the console.

Since this function isn't connected to the UI yet, nothing will change.

#### Step 2: Add a click handler to call the new function with film object

Now, let's connect `handleDetailsClick`. Add an `onClick` to `FilmRow` so that our message gets printed whenever we click on a film row - don't forget to pass the argument.

You should be able to check this in your console by clicking any film row.

#### Step 3: Add `stopPropagation` to `handleFave` event handler

Hold up! Notice that you are now seeing two messages every time you click on the Fave icon/button. This is tricky, but the reason is because the event is propagating upward to the `FilmRow`. To make it so only one message appears, you'll need to stop the event propagation.

To do this, inside the `Fave` component's `handleClick` function, add the line `e.stopPropagation()`.

Try clicking the Fave icon/button - there's only one message now.

### Tasks - Part 4: Add State to the `Fave` Component

We have now triggered the events we'll need to update our app. Data in a React app that changes (like adding a film to the Faves list) is called state. Let's add some simple state to some of our components to see how it works.

#### Step 1: Create a constructor for the `Fave` component

The first state we'll add will be whether a currently selected film is a user's favorite.

Add a constructor method to the `Fave` component. Remember that any time you add a constructor to a class-based component, you have to call the `super` method (and pass it `props`).

#### Step 2: Bind your event handler to this component

`this` binding is a very tricky topic in JavaScript, but there are tricks to using it. To make our event handler work properly, we'll need to bind `this` to it. Add this line to your `Fave` component's constructor:

```js
// This binding is necessary to make `this` work in the callback
this.handleClick = this.handleClick.bind(this)
```

This will be required for each event handler defined on any of your class-based components - so add it for all the `onClick` methods in your classes, in that class' constructor (make sure to change `handleClick` to the name of the function in that class).

#### Step 3: Set the initial state

By default, a film is not a user's favorite.

Back to the `Fave` component, set `this.state` to an object with the key `isFave` and the value `false`. This will set up the initial state of the component.

#### Step 4: Set the state in your event handler

When the user clicks the Fave icon/button to add or remove a film from their favorites list, the app should change the film's `isFave` state to reflect that.

Inside of the `handleClick` method on the `Fave` component, use `this.setState` to toggle the value of `isFave`. Toggle means we always want to set the new value to the opposite of the current value.

**Hint:** One way to do this is with `isFave: !this.state.isFave`.

#### Step 5: Set the `className` on `div` based on the `IsFave` state

We now want the `className` attribute on the div to dynamically update when the state is changed. Currently, the `className` on the `div` is `add_to_queue`. However, if the film is already favorited, then the film is already in the queue. Therefore, when `isFave: true`, the `className` should instead be `remove_from_queue`.

You need to make this happen:
- When `isFave: true`, you want to add the class `remove_from_queue` and when `isFave: false` you want to add the class `add_to_queue`.
- You also want to change the text that's rendered in the `p` to be the same text as the class - `remove_from_queue` or `add_to_queue`.

Note: It will be easier to read if you determine which class to set first, store that value in a variable, then insert that variable into the `className` attribute.

**Hint** A more advanced and succinct way to write this function could be:
```js
const isFave = (this.state.isFave) ? 'remove_from_queue' : 'add_to_queue'
```

- You can drop this in the `render` method.
- This checks the current `isFave` state for true or false. If it's true, it sets the `const` variable `isFave` to `remove_from_queue`; when it's false, it sets the `const` variable `isFave` to `add_to_queue`.

Once you have this, clicking the "Add" icon in each row should change the icon displayed.

### Tasks - Part 4: Add State to `FilmListing` Component

Currently, we have the ALL and FAVE headings, but all films are always shown. Next, we'll add a state so that we can track if the user is currently filtering to view ALL movies or only their FAVE movies.

#### Step 1: Set the initial state

By default, a user will be viewing the entire list of movies.

In the `FilmListing` component, set `this.state` to an object with the key `filter` and the value `all`. This will set up the initial state of the component.

#### Step 2: Set the state in your event handler

The `handleFilterClick` method is the one that's called when a user clicks ALL or FAVES, so it's where you'll change the filter.

Inside of the `handleFilterClick` method on the `FilmListing` component, use `this.setState` to set the value of `filter` to the value passed to the event handler.

#### Step 3: Set the className on div based on filter state


To give the user a clue as to where they are, the ALL and FAVES `div`s should change color depending on which is active. In the CSS, we've already set the colors using a class; you'll need to dynamically change which class each `div` has.

We now want the `className` attribute on each `.film-list-filter` div to dynamically update when the state is changed. When `filter: all`, we want to add the class `is-active` to the `ALL` filter. When `filter: faves`, we want to add the class `is-active` to the `FAVES`.

**Hint** This can be done by adding a line similar to this (different for each `div`) into the `className` parameter. This line in particular checks if the `filter` state is currently "all"; if it is, it sets the value to `is-active`. If it isn't, it does nothing.
```js
{this.state.filter === 'all' ? 'is-active' : ''}
```

**Hint 2**: Still stuck? The overall `<div>` for "ALL" will look like this:
```js
<div className={`film-list-filter ${this.state.filter === 'all' ? 'is-active' : ''}`} onClick={() => this.handleFilterClick('all')}>
```