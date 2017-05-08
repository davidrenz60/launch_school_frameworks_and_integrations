1. Name all the 7 (or 8) routes exposed by the resources keyword in the
routes.rb file. Also name the 4 named routes, and how the request is routed to the controller/action.

    example using a `posts` resouce:

    route, controller#action, named route (if applicable)

    ```
    GET /posts, to: posts#index, as: posts_path
    GET /posts/:id, to: posts#show, as: post_path
    GET /posts/new, to: posts#new, as: new_post_path
    POST /posts, to: posts#create
    GET /posts/:id/edit, to: posts#edit, as: edit_post_path
    PATCH/PUT /posts/:id, to: posts#update
    DELETE /posts/:id, to: posts#destroy
    ```

2. What is REST and how does it relate to the resources routes?

    REST stands for representational state transfer. It is a pattern used to create, read, update, and delete resources (CRUD) using an http request response cycle. The resources routes allow these routes to be mapped to the specific controller action to set up this pattern. 

3.  What's the major difference between model backed and non-model backed form helpers?
    
    In a model backed form, all of the form helpers are associated with an object. The fields need to be a column in the db, or a virtual attribute.
    With a model backed form, when the data is submitted, all necessary data will appear in the params hash under a key as the name of the model. This hash can be used to mass assign to make a new object, or update and existing one.

4. How does form_for know how to build the `<form>` element?
    
    `form_for` takes an object as an argument. With this, attributes on that object can be used to create labels and various inputs. Depending on the controller action, if the model object is a new record or an existing record, form_for will know what action to take for the form element.

5. What's the general pattern we use in the actions that handle submission of model-backed forms (ie, the create and update actions)?
        
    example using `posts` again

    ```ruby
    @post = Post.create(post_params) # this will use update in the update action

    if @post.save
        # success message
        redirect_to posts_path # or whatever path makes sense to redirect
    else
        render :new # this will be :edit in the update action
    end
    ```

6. How exactly do Rails validations get triggered? Where are the errors saved? How do we show the validation messages on the user interface?

    Validations are triggered when a model is attempted to be saved to the database that fails a validation. The errors are saved on the object, so it is important to not redirect in order to still have access to the errors. The object will have an `errors` property, which has `full_messages` method. This can be used to iterate through and view all validations that failed.

7. What are Rails helpers?

    Rails helpers are methods that can be used in views. This allows application logic to not clutter the view's code. Generally they should be used for data presentation. Helpers are defined in the `application_helper.rb`file. 

8. What are Rails partials?
    
    Partials are reusable pieces of erb code. If there are multiple places where code is reused, it may be a good place to make a partial. The file name should be prefixed with an underscore by convention.


9. When do we use partials vs helpers?

    Partials should be used for larger pieces of templating code that can be extracted and used in different views. Helpers should be reserved for formatting and presentation of data within views.

10. When do we use non-model backed forms?
    
    When there is no object that needs to be associated with the form.

