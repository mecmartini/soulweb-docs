# Component based theme using UI-Patterns

[UI Patterns](https://www.drupal.org/project/ui_patterns) can expose stand-alone theme components (patterns) to Drupal and enables them to be used with the core Layout and Views module and well-known contrib modules like Field Group, Display Suite and Panels.

Here is explained how to create a component library by an example that includes two components:

* `Button One`
* `Paragraph One` (include `Button One`)

## Requirements

The `Drupal` modules required are:

* [Component Libraries](https://www.drupal.org/project/components)
* [Display Suite](https://www.drupal.org/project/ds)
* [Paragraphs](https://www.drupal.org/project/paragraphs)
* [UI Patterns](https://www.drupal.org/project/ui_patterns)

#### 1. Create the components library

We must define our components library into the `[THEME_NAME].info.yml` of our theme:

    component-libraries:
      atoms:
        paths:
          - components/atoms
      paragraphs:
        paths:
          - components/paragraphs

where `atoms` and `paragraphs` are the names of the component libraries we are defining, and in `paths` we define the folder that contains the components of the library.

Then we must create the folders of the paths defined above. So, inside our theme directory, create the folders:

* components/atoms
* components/paragraphs

#### 2. Create the component button-one

A `.ui_patterns.yml` file is used to define the components provided by a theme or a module and expose them to `Drupal`. `UI Patterns` will use all `.ui_patterns.yml` files it finds in a theme’s or module’s directory structure, so it is possible to create component-specific files!

Let's create, inside our theme directory, the file `[THEME_NAME].ui_patterns.yml` with the following content to define the `button-one` component:

    button_one:
      label: Button one
      description: A button template.
      fields:
        button_label:
          type: string
          label: Button label
          description: The label to print into the button.
          preview: Label
        button_url:
          type: string
          label: Button URL
          description: The URL for the button.
          preview: http://www.soulweb.it
      use: '@atoms/buttons/button-one/button-one.html.twig'

We must now create the template file declared above. So, in our component library directory (`component/atoms`), let's create the folder `buttons/button-one` and create the file `button-one.html.twig` inside it.

#### 2. Create the component paragraph-one

Let's add to `[THEME_NAME].ui_patterns.yml` the definition of `paragraph-one` component:

    paragraph_one:
      label: Paragraph one
      description: A paragraph template.
      fields:
        image:
          type: image
          label: Image
          description: An image.
          preview: '<img src="http://lorempixel.com/640/480/nature/1/" title="An image" />'
        text:
          type: string
          label: Text
          description: A text.
          preview: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis eget vulputate tellus, vel aliquam sapien. Donec convallis nibh eu volutpat egestas. Nullam enim metus, placerat eleifend condimentum id, sodales in arcu. Quisque vel lacus odio. Nullam vel massa quis lectus ullamcorper placerat. Cras at turpis nec nisl porttitor posuere quis et massa. Etiam mattis leo nec laoreet venenatis. Donec dictum nunc eget tristique molestie. Duis vel mi justo. Vivamus sagittis justo erat, eu tempor elit malesuada in. Praesent sem lorem, aliquet lobortis rhoncus sed, sagittis vitae nunc. Phasellus convallis dui a augue hendrerit, non scelerisque augue mollis. Vivamus congue, mi sit amet rhoncus finibus, nisi nisi posuere mi, id dictum nibh augue nec arcu.
        button_label:
          type: string
          label: Button label
          description: The label to print into the button.
          preview: Label
        button_url:
          type: string
          label: Button URL
          description: The URL for the button.
          preview: http://www.soulweb.it
      use: '@paragraphs/paragraph-one/paragraph-one.html.twig'

We must now create the template file declared above. So, in our component library directory (`component/paragraphs`), let's create the folder `paragraph-one` and create the file `paragraph-one.html.twig` inside it.

#### 4. Create asset library for component button-one

We must define our asset library into the `[THEME_NAME].libraries.yml` of our theme:

    button-one:
      version: 1.x
      css:
        theme:
          components/atoms/buttons/button-one.css: {}
      js:
        components/atoms/buttons/button-one.js: {}

