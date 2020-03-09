# React + GitHub API project

In this project, we're going to take a small, existing React application and add new features to it.

Here's what the application will look like once you are done:

![react github project](https://github.com/moloi/React-GitHub-Project-API-/blob/master/Display%20Page.PNG)

The code you are given for the project implements the search form and the loading of basic user info. You'll have to do all the rest.

Let's take a look at the code that's already there. Many of the starter files should already be familiar to you if you completed [the previous workshop](https://github.com/ziad-saab/react-intro-workshop).

* `package.json`: Configuration file for NPM, contains dependencies and project metadata
* `.gitignore`: Files that should be ignored by Git. `node_modules` can always be regenerated
* `public/index.html`: File that gets served thru Webpack after having been filled in
* `src/index.js`: This file is the entry point for the app. It puts our app on the screen!
* `src/components/*`: All the components of our application.
* `src/index.css`: The styles for our app. Check it out to see how your starter app is being styled, and add to it to complete the project. Notice that we don't `<link>` this CSS from the index? How does this work?? Make sure you understand!!!

**To get started coding on this project, remember the following steps**:

1. `npm install` the first time you clone this repo
2. `npm start` anytime you want to start developing. This will watch your JS files and re-run webpack when there are changes
3. Start coding!

In `index.js` we have the following route structure:

```javascript
<Route path="/" component={App}>
  <IndexRoute component={Search}/>
  <Route path="user/:username" component={User}/>
</Route>
```

The top route says to load the `App` component. Looking at the code of `App.jsx`, you'll see that its `render` method outputs `{this.props.children}`. If the URL happens to be only `/`, then React Router will render an `<App/>` instance, and will pass it a `<Search/>` as its child. If the route happens to be `/user/:username`, React Router will display `<App/>` but will pass it `<User />` as a child.

When the `Search` component is displayed, it has a form and a button. When the form is submitted, we use React Router's `browserHistory` to **programmatically change the URL**. Look at the `Search`'s `_handleSubmit` method to see how that happens.

Once we navigate to the new URL, React Router will render a `User` component. Looking at the `componentDidMount` method of the `User`, you'll see that it does an AJAX call using `this.props.params.username`. The reason why it has access to this prop is because the Router passed it when it mounted the component.

The AJAX call is made to `https://api.github.com/users/{USERNAME}` and returns the following information:

```json
{
  "login": "gaearon",
  "id": 810438,
  "avatar_url": "https://avatars.githubusercontent.com/u/810438?v=3",
  "gravatar_id": "",
  "url": "https://api.github.com/users/gaearon",
  "html_url": "https://github.com/gaearon",
  "followers_url": "https://api.github.com/users/gaearon/followers",
  "following_url": "https://api.github.com/users/gaearon/following{/other_user}",
  "gists_url": "https://api.github.com/users/gaearon/gists{/gist_id}",
  "starred_url": "https://api.github.com/users/gaearon/starred{/owner}{/repo}",
  "subscriptions_url": "https://api.github.com/users/gaearon/subscriptions",
  "organizations_url": "https://api.github.com/users/gaearon/orgs",
  "repos_url": "https://api.github.com/users/gaearon/repos",
  "events_url": "https://api.github.com/users/gaearon/events{/privacy}",
  "received_events_url": "https://api.github.com/users/gaearon/received_events",
  "type": "User",
  "site_admin": false,
  "name": "Dan Abramov",
  "company": "Facebook",
  "blog": "http://twitter.com/dan_abramov",
  "location": "London, UK",
  "email": "dan.abramov@me.com",
  "hireable": null,
  "bio": "Created: Redux, React Hot Loader, React DnD. Now helping make @reactjs better at @facebook.",
  "public_repos": 176,
  "public_gists": 48,
  "followers": 10338,
  "following": 171,
  "created_at": "2011-05-25T18:18:31Z",
  "updated_at": "2016-07-28T14:41:02Z"
}
```
[GitHub API documentation for Users](https://developer.github.com/v3/users/)

In the `render` method of the `User` component, we are displaying the user info based on the received result, and we have three links that don't lead anywhere for the moment:

![links](http://i.imgur.com/3CFG1ir.png)

If you click on followers, notice that the URL of the page changes to `/users/:username/followers`. If you have your dev tools open, React Router will give you an error message telling you that this route does not exist.

**The goal of this workshop** is to implement the three links above. To do this, we'll start by implementing the followers page together with step by step instructions. Then, your job will be to implement the two remaining screens and fix any bugs.

## Implementing the Followers page
When clicking on the followers link in the UI, notice that the URL changes to `/user/:username/followers`. Currently this results in a "not found" route. Let's fix this.

![followers page](https://github.com/moloi/React-GitHub-Project-API-/blob/master/Display%20Results.PNG)

