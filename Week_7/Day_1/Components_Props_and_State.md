# Components, Props, and State

* Consider creating our Tweeter app again

* What are the components in the app?
  * Header, Profile, Nav, Tweet, TweetList, TweetForm, TweetToggle
  * Of course, we have our App

* App gives data to Nav, Profile, TweetForm, and TweetList
  * Will call each of those components, not always giving dynamic data

* What would be dynamic data? The profile won't change, but the TweetForm will open and close with the "Write Tweet" toggle
  * **Nav** has a `prop` of `tweetForm`
    * It's the App's responsibility to decide if it's open or closed
    * Nav has no `state` (doesn't decide anything)

  * **Profile** has the `props` of `avatar-url` and `name`
    * Profile doesn't have `state`

  * **TweetList** has the `props` of `tweets` in an array
    * TweetList is not in charge of showing any tweets, so again this doesn't have `state`
    * TweetList gives array data to the **Tweet** component
      * Tweet will have `props` of `tweet` as objects (which is `.map()` loops through the TweetList tweets array)

  * **TweetForm** has `state` of formdata and setFormData
    * State always has the value and a way to change the value

  * App doesn't have `props`
    * `props` is given from a parent to a child
    * App doesn't have any parents
    * However, App has `state` for tweets, userInfo, toggle

---