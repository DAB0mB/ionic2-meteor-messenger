## First Ionic Component

Now that we're finished with the initial setup, we can start building our app.

An application created by Ionic's CLI will have a very clear methodology. The app is made out of pages, each page is made out of 3 files:

- `.html` - A view template file written in HTML based on Angular2's new [template engine](https://angular.io/docs/ts/latest/guide/template-syntax.html).
- `.scss` - A stylesheet file written in a CSS pre-process language called [SASS](https://sass-lang.com).
- `.ts` - A script file written in Typescript.

By default, the application will be created with 3 pages - `about`, `home` and `contact`. Since our app's flow doesn't contain any of them, we first gonna clean them up by running the following commands:

    $ rm -rf src/pages/about
    $ rm -rf src/pages/home
    $ rm -rf src/pages/contact

Second, we will remove their declaration in the app module:

{{{diff_step 2.1 src/app/app.module.ts}}}

And finally, will their usage in that tabs component:

{{{diff_step 2.2}}}

Now we're gonna define 4 tabs: `chats`, `contacts`, `favorites` and `recents`. In this tutorial we want to focus only on the messaging system, therefore we only gonna implement the chats tab, the rest is just for the layout:

{{{diff_step 2.3}}}

Our next step would be implementing the `chats` tab; First let's start by adding `moment` as a dependency - a utility library in JavaScript which will help us parse, validate, manipulate, and display dates:

    $ npm install --save moment

We will start by implementing a view stub, just so we can get the idea of Ionic's components:

{{{diff_step 2.5}}}

Once creating an Ionic page it's recommended to use the following layout:

- &lt;ion-header&gt; - The header of the page. Will usually contain content that should be bounded to the top like navbar.
- &lt;ion-content&gt; - The content of the page. Will usually contain it's actual content like text.
- &lt;ion-footer&gt; - The footer of the page. Will usually contain content that should be bounded to the bottom like toolbars.

To display a view, we will need to have a component as well, whose basic structure should look like so:

{{{diff_step 2.6}}}

The logic is simple. Once the component is created we gonna a define dummy chats array just so we can test our view. As you can see we're using the `moment` package to fabricate date values. The `Observable.of` syntax is used to create an Observable with a single value.

Now let's load the newly created component in the module definition so our Angular 2 app will recognize it:

{{{diff_step 2.7}}}

Let's define it on the tabs component:

{{{diff_step 2.8}}}

And define it as the root tab, which means that once we enter the tabs view, this is the initial tab which is gonna show up:

{{{diff_step 2.9}}}

One of the tab's attributes is wrapped with \[square brackets\]. This is part of Angular2's new template syntax and it means that the `root` property of the HTML element is bound to the `chatsTab` property of the component.

## TypeScript Interfaces

Now, because we use TypeScript, we can define our own data-types and use then in our app, which will give you a better auto-complete and developing experience in most IDEs. In our application, we have 2 models at the moment: a `chat` model and a `message` model. We will define their interfaces in a file located under the path `/modes/whatsapp-models.d.ts`:

{{{diff_step 2.10}}}

The `d.ts` extension stands for `declaration - TypeScipt`, which basically tells the compiler that this is a declaration file, just like C++'s header files. In addition, we will need to add a reference to our models declaration file, so the compiler will recognize it:

{{{diff_step 2.11}}}

Now we can finally use the chat model in the `ChatsPage`:

{{{diff_step 2.12}}}

## Ionic Themes

Ionic 2 provides us with a comfortable theming system which is based on SASS variables. The theme definition file is located in `src/theme/variable.scss`. Since we want our app to have a "Whatsappish" look, we will define a new SASS variable called `whatsapp` in the variables file:

{{{diff_step 2.13}}}

The `whatsapp` color can be used by adding an attribute called `color` with a value `whatsapp` to any Ionic component:

{{{diff_step 2.14}}}

Now let's implement the chats view properly in the `ChatsView` by showing all available chat items:

{{{diff_step 2.15}}}

We use `ion-list` which Ionic translate into a list, and use `ion-item` for each one of the items in the list. A chat item includes an image, the receiver's name, and its recent message.

> The `async` pipe is used to iterate through data which should be fetched asynchronously, in this case, observables

Now, in order to finish our theming and styling, let's create a stylesheet file for our component:

{{{diff_step 2.16}}}

Ionic will load newly defined stylesheet files automatically, so you shouldn't be worried for importations.

## External Angular 2 Modules

Since Ionic 2 uses Angular 2 as the layer view, we can load Angular 2 modules just like any other plain Angular 2 application. One module that may come in our interest would be the `angular2-moment` module, which will provide us with the ability to use `moment`'s utility functions in the view using pipes.

It requires us to install `angular2-moment` module using the following command:

    $ npm install --save angular2-moment

Now we will need to declare this module in the app's main component:

{{{diff_step 2.18}}}

Which will make `moment` available as a pack of pipes, as mentioned earlier:

{{{diff_step 2.19}}}

## Ionic Touch Events

Ionic provides us with components which can handle mobile events like: slide, tap and pinch. In the `chats` view we're going to take advantage of the slide event, by implementing sliding buttons using the `ion-item-sliding` component:

{{{diff_step 2.20}}}

This implementation will reveal a `remove` button once we slide a chat item to the left. The only thing left to do now would be implementing the chat removal event handler in the chats component:

{{{diff_step 2.21}}}
