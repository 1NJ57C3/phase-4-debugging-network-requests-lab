# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted
  - How I debugged:
    - This time (Going out of the way to debug methodically):
    1. Find and open new `toy form` in client
    2. Attempt to `submit` blank form
    3. Read `error type` in `Console`
    4. Error `500` > Check `Network` tab in `Inspect` tools
    5. Identify top-level error in `traces` (`Application Trace`)
    6. Read ``"app/controllers/toys_controller.rb:10 in `create'"``  
    7. Fix `Toy`~~`s`~~`.create`  
    8. Retest with dummy data
- Update the number of likes for a toy  
  - How I debugged:  
    1. Attempt to `Like`  
    2. Check `Console` for error message  
    3. `SyntaxError: Unexpected end of JSON input` -- JSON not being returned  
      - error points at ToyCard.js:21:1, which is actually our fetch  
      - if I didn't know error code prior, I'd either Google or check log in terminal running`rails s` 
        - Error code `204 No Content`
    4. Fix missing `json` response -- `render json: toy, status: :ok`
    5. Retest button
- Donate a toy to Goodwill (and delete it from our database)
  - How I debugged:
    1. Click `Donate to GoodWill` button in Client
    2. Check `Console` for error message -- 404
    3. `Terminal` where `rails s` is running for error -- No route match
    4. Navigate to `config/routes.rb`
    5. Enable `delete` route by adding `:destroy` to `resources`'s `only:` array
    6. Retest