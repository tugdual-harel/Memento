# MEMENTO

---------------------------

## Sommaire
* Liens [#link](https://denikine.github.io/Memento/#liens)
* Ruby on Rails [#link](https://denikine.github.io/Memento/#ruby-on-rails)
* Postgresql [#link](https://denikine.github.io/Memento/#postgresql)
* Git [#link](https://denikine.github.io/Memento/#git)
* Linux [#link](https://denikine.github.io/Memento/#linux)
* HTML [#link](https://denikine.github.io/Memento/#html)
* CSS [#link](https://denikine.github.io/Memento/#css)
* PHP [#link](https://denikine.github.io/Memento/#php)
* Markdown [#link](https://denikine.github.io/Memento/#markdown)

--------------------------------

## Liens
* Stack Edit
[https://stackedit.io/app#](https://stackedit.io/app#)
* Memento online
[https://denikine.github.io/Memento/](https://denikine.github.io/Memento/)
* Mediabox
[https://mediabox.2-ride.com/dashboards/products](https://mediabox.2-ride.com/dashboards/products)
* Mediabox Github
[https://github.com/bsignoret2/2ride-mediabox](https://github.com/bsignoret2/2ride-mediabox)
* Convert HTML to HAML
[https://htmltohaml.com/](https://htmltohaml.com/)
* Ultimate Website Generator
[https://webcode.tools/](https://webcode.tools/)

---------------------------------

## Ruby on Rails

### Code utilisé en projets
* ProductType find_by et création si non existant
```ruby
    product_type = ProductType.find_by(name: "Casque Shark")
    product_type ||= ProductType.create(name: "Casque Shark")
```
* Product find and destroy
```ruby
    Product.find(9).product_assets.update_all(entity_id: 11)
    Product.find(9).destroy
```
* Connexion au FTP et téléchargement d'un fichier csv
```ruby
    c = Ftp::Base.connection
    data = c.download!("/home/bertrand/public_html/#{Rails.env}/input/bering_segura_products.csv")
```
* Création d'un array
```ruby
    product_feature_array = [
          ["FAMILLE_PRODUIT", u[8]],
          ["SOUS-FAMILLE", u[9], u[92], u[93], u[94], u[95]],
          ["WHEIGHT", u[83]],
          ["NOUVEAUTE", product.is_new],
          ["HAUTE VISIBILITE", ActiveModel::Type::Boolean.new.cast(u[100])],
        ]
```
* Temps d'exécution
```ruby
    start_time = Time.now
    #code
    #code
    #code
    end_time = Time.now
    puts "Time elapsed: #{end_time - start_time}s"
```
* If
```ruby
    product_feature_array.each do |couple|
      if condition
        
      end
```
* Affichage
```ruby
    ActiveRecord::Base.logger.level = 1 # non-affichage des lignes inutiles de la console
      ProductType.all.each do |product_type|
        puts "#{product_type.title},#{product_type.products.map}"
```
* Utilisation de **Pluck**
```ruby
    if !product_type.nil?
       product_features_names = product_type.product_features.pluck(:name)
       attributes.push(product_type.product_features.pluck(:title))
    end
```
* Langue
```ruby
    I18n.locale = :fr
```
* Mode édition postgresql
```bash
    psql nom-de-la-base
```
* Ajout champs avec rails
```ruby
    rails g migration addVisibleOnDigitalCatalog2ToProducts visible_on_digital_catalog_2:boolean
    rake db:migrate
```
* Modification password accès db
```ruby
    rails c
    User.first
    User.first.update(password:"password",password_confirmation:"password")
```
* Création d'un bouton sur une nouvelle page dashboards/actions
 * Insertion de la méthode get dans config/routes.rb
```ruby
    get 'dashboards/actions'
```
 * Création du fichier views/dashboards/actions.html.haml et utilisation de [https://htmltohaml.com/](https://htmltohaml.com/)
 * Dans controllers/dashboards_controller.rb, insérer :
```ruby
    def actions

    end
```
* Génération d'un CSV
 * Dans models/product_color.rb
 ```ruby
 def self.to_csv(product_type_name = nil)
    attributes = %w{reference product_type_title product_name}
    if product_type_name.present?
      product_type = ProductType.find_by(name: product_type_name)
      if !product_type.nil?
        product_features_names = product_type.product_features.pluck(:name)
        attributes.push(product_type.product_features.pluck(:title))
      end
    end
    CSV.generate(headers: true) do |csv|
      csv << attributes

      all.each do |product_color|
        row = Array.new
        product = product_color.product
        row.push(product_color.reference)
        row.push(product_color.product.product_type.title)
        row.push(product.name)
        if !product_type.nil?
          product_features_names.each do |product_feature_name|
            if product.product_feature_value(product_feature_name).present?
              row.push(product.product_feature_value(product_feature_name))
            else
              row.push(product_color.product_feature_value(product_feature_name))
            end
          end
        end
        csv << row
      end
    end
  end
```
 * Dans controller/product_colors_controller.rb
 ```ruby
 def index
    @product_colors = ProductColor
    @product_colors = @product_colors.where(product_id: Product.where(brand_id: Brand.find_by(name: params[:brand]).id).pluck(:id)) if params[:brand].present?
    @product_colors = @product_colors.where(product_id: Product.where(product_type_id: ProductType.find_by(name: params[:product_type_name]).id).pluck(:id)) if params[:product_type_name].present?
    @product_colors = @product_colors.where(visible_on_mediabox: true) if params[:visible_on_mediabox].present?
    @product_colors = @product_colors.where(visible_on_b2c: true) if params[:visible_on_b2c].present?
    @product_colors = @product_colors.where(visible_on_b2b: true) if params[:visible_on_b2b].present?
    @product_colors = @product_colors.where(visible_on_digital_catalog: true) if params[:visible_on_digital_catalog].present?

    respond_to do |format|
      format.csv { send_data @product_colors.to_csv(product_type_name = params[:product_type_name]), filename: "product_colors-#{Date.today}.csv" }
    end
  end
 ```
 * Dans config/sidekiq.yml
 ajouter `csv-export` dans la liste des queues
 * Dans products.html.haml
 ```haml
 %tr.d-flex
    %th Export
    %td= link_to "Product Size", product_sizes_path(format: "csv", brand_name: params[:brand_name], product_type_name: params[:product_type_name], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog]), class: "btn btn-primary btn-sm"
    %td= link_to "Product Color", product_colors_path(format: "csv", brand_name: params[:brand_name], product_type_name: params[:product_type_name], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog]), class: "btn btn-primary btn-sm"
    %td= link_to "Export in FTP ProductColorPicture", dashboards_products_path(brand_name: params[:brand_name], product_type_name: params[:product_type_name], display_product_features: params[:display_product_features], display_sizes: params[:display_sizes], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog], export_product_color_assets: true), class: "btn btn-primary btn-sm"
  ```

### Cheat Sheet 1
* Nouvelle application
```ruby
# Installation de la gemme rails
gem install rails
# Génération d'une nouvelle app rails avec support postgres
rails new my_app --database=postgresql
# Initialisation dela bdd
rake db:create
# Démarrage du serveur rails
rails s
```
* Routes
```ruby
# Création d'une route qui lie une url à l'action d'un controller
# config/routes.rb
get 'welcome' => 'pages#home'
# shorthand for connection a route to a controller/action
# config/routes.rb
get 'photos/show'
# équivalent à
get 'photos/show', :to 'photos#show'
get 'photos/show' => 'photos/show'
# création automatique des routes pour une RESTful resource
#confi/routes.rb
resources :photos
```
* Controllers
```ruby
rails g controller photos
# generate a new controller with default actions, routes and views
rails g controller Photos index show
```
* Models
```ruby
# generate a model and create a migration for the table
rails g model Photos
# generate a model and create a migration with table columns
rails g model Photo path:string caption:text
# The migration automatically created for the above command :
class CreatePhotos < ActiveRecord::Migration
  def change
    create_table :photos do |t|
      t.string :path
      t.text :caption
 
      t.timestamps null: false
    end
  end
end
```
* Migrations
```ruby
# migration data types
:boolean
:date
:datetime
:decimal
:float
:integer
:primary_key
:references
:string
:text
:time
:timestamp
```
* Scaffolding
```ruby
rails g scaffold Photo path:string caption:text
rake db:migrate
```
* Rake
```ruby
#view all the routes in an application
rake routes
# seed the database with sample data from db/seeds.rb
rake db:seed
#run any pending migrations
rake db:migrate
# rollback the last migration performed
rake db:rollback
```
* Path Helpers
```ruby
# creating a path helper for a route
get '/photos/:id', to: 'photos#show', as: 'photo'
# app/controllers/photos_controller.rb
@photo = Photo.find(17)
# view for the action
<%= link_to 'Photo Record', photo_path(@photo) %>
# config/routes.rb
resources :photos
```
* Form Helpers
```ruby
# app/controllers/photos_controller.rb
def new
  @photo = Photo.new
end
```
```html
# ERB view
<%= form_for @photo, url: {action: "create"}, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :path %>
  <%= f.text_area :caption, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```
```html
<!-- HTML output -->
<form accept-charset="UTF-8" action="/photos/create" method="post" class="nifty_form">
  <input id="photos_path" name="photo[path]" type="text" />
  <textarea id="photos_caption" name="photo[caption]" cols="60" rows="12"></textarea>
  <input name="commit" type="submit" value="Create" />
</form>
```
```ruby
<%= form_tag("/search", method: "get") do %>
  <%= label_tag(:q, "Search for:") %>
  <%= text_field_tag(:q) %>
  <%= submit_tag("Search") %>
<% end %>
```
```html
<form accept-charset="UTF-8" action="/search" method="get">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <label for="q">Search for:</label>
  <input id="q" name="q" type="text" />
  <input name="commit" type="submit" value="Search" />
</form>
```

### Cheat Sheet 2
* Création d'une nouvelle app
```ruby
rails new name
# create and name new app

bundle install
# install the gem's version specified in the Gemfile.lock

bundle install - without production
# exclure les gems du groupe production

bundle update
# update les gem dependencies aux dernières versions
```
* Environnements et navigateurs
```ruby
rails server
rails s
# access the app via browser

rails console
rails c
# access app via dev environment

rails console - sandbox
# test code without changing data

rails console test
# run the console in test environment

reload!
# use if changed source code and want changes reflected in the console without restart

rails server - environment production
# run rails app in prod mode
```
* Generate
```ruby
rails generate scaffold Post name: string title: string content: text

rails generate controller Posts or rails g controller Posts:

rails generate model Post:

rails generate model Post name: string title: string 
```
* Migration
```ruby
rails db:migrate
# runs the migration of a model and ist attributes

rails generate migration migration_description
# easiest way to chang something in the bdd schema is to generate the migration

rails db:migrate:reset
# drop the bdd information and run migration on a new one

rails db:seed
# loads data from db/seeds.rb
```
* When messing things up
```ruby
rails destroy model Post
# almost anything created with the generate command can be destroyed with the destroy command

rails db:rollback
# undoes the last migration, you can edit the file and run rails db:migrate again

rails db:migrate VERSION=0
# use this to rollback all migrations back to a target migration with a version number
```
* Testing
```ruby
rails test
rails t
# run the test suite to verifiy if our test apsses or not

rails generate integration_test site_layout
#  integration tests are used to test how various parts of your application interact. Here we are creating an integration test named “site_layout.test.rb” inside the “test/integration” folder.

rails test: integration
# only runs a specific section of tests, in this case, it will only run the integration tests.
```
* Routes
```ruby
rails routes
# get complete list of the available routes in your application
```
* Deploying with Heroku
```ruby
# utiliser si la branche actuelle n'est pas une master branch : git push heroku yourbranch:master
git status
git add -A
git commit -m "Description of commit"
git push
rails test
git push heroku
heroku pg:reset DATABASE
heroku run rails db:migrate
heroku run rails db:seed
heroku open
```
* Launch Sidekiq
```ruby
bundle exec sidekiq -C config/sidekiq.yml
```

### Cheat Sheet 3
* General concepts
```ruby
# comment
# comment text

# variables
variable = some_value

# console output
puts something

# call a method
object.method(arguments)

# define a method
def name(parameter)
    # method body
end

# equality
object == other

# inequality
object != other

# decisions with if
if condition
    # happens when true
else
    # happens when false
end

# constants
CONSTANT = some_value
```
* Numbers
```ruby
#normal number
number_of_your_choice
0
-11
42

# decimals
main.decimal
3.2
-5.0

# basic math
n operator m
2 + 3 = 5
8 * 7 = -2

# comparison
n operator m
12 > 3 => true
```
* Strings
```ruby
# create
'a string'
'hello world'

# interpolation
"a string and an #{expression}"
"Email : #{user.email}"
"The total is #{2+2}"

# length
string.length
"Hello".length = 5

# concatenate
string + string 2
"Hello "+ "reader"

# substitute
string.gsub(a_string, subtstitute)

#access
string[position]
"Hello"[1] # => "e"
```
* Arrays
```ruby 

```

--------------------

## Postgresql
### Création d'une table avec Postgresql (similaire à mysql)
```sql
    CREATE TABLE persons (
      id SERIAL,
      first_name VARCHAR(50),
      last_name VARCHAR(50),
      dob DATE,
      email VARCHAR(255),
      PRIMARY KEY (id)
    )
```
### Création user & base avec postgres
```sql
    sudo -u postgres psql
    CREATE USER "guy_on_stackoverflow" with password 'keepitonthedl';
    CREATE DATABASE "dcaclab_development" owner "guy_on_stackoverflow";
```
### Importer un CSV dans une table
```sql
    COPY persons(first_name, last_name, dob, email)
    FROM 'C:\sampledb\persons.csv'
    DELIMITER ','
    CSV HEADER;
```
### Création nouveau champ dans postgresql
```psql
    ALTER TABLE nom-de-la-table
    ADD COLUMN nom-de-la-colonne type-de-donnée;
```
### Infos à renseigner dans database.yml
```yml
    development:
      adapter: postgresql
      encoding: unicode
      database: kickrstack_development
      host: localhost
      pool: 5
      username: kickrstack
      password: secret
```

--------------------------



## Git

### Création nouvelle branche
```git
    git checkout develop
    git pull
    git checkout -b nouvelle-branche
    git push -u origin nouvelle-branche
```

### Publication branche locale
```git
    git checkout branche-locale
    git status
    git commit -am "indication de la modif"
    git push
    git push -u origin branche-locale
```


-----------------------

## Terminal
### Sublime text open with terminal
```bash
    subl
    ```
### Table Plus open with terminal
```bash
    tableplus
    ```
### Cheat Sheet
```bash
# change directory
cd directory
cd my_app
cd my_app/app/controllers

# list contents directory
ls directory
# windows : dir directory
ls
ls my_app

# directory you are currently in
pwd

# create a new directory
mkdir name
mkdir rails

# delete a file
rm file
# windows : del file

# delete a directory
rm -r folder
# windows : rd folder

# strarting a program
program arguments
firefox
firefox railsgirlsberlin.de
irb

# abort the program
ctrl+C
```



--------------------------

## HTML
### Template de base
```html
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Untitled</title>
        <meta name="description" content="This is an example of a meta description.">
        <link rel="stylesheet" type="text/css" href="theme.css">

        <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="HandheldFriendly" content="true">

        <!--[if lt IE 9]>
      <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
        <![endif]-->
      </head>
      <body>
        
      </body>
    </html>
```
### Metas
```html
    <meta charset="utf-8">
    <title>Titre de la page</title>
    <meta name="description" content="Description de la page en quelques mots">
    <meta name="robots" content="index, follow">
    <meta name="author" content="auteur de la page">
```
### Formulaire HTML
```html
    <form role="form">
      <div class="form-group">
        <label for="email">Email address:</label>
        <input type="email" class="form-control" id="email">
      </div>
      <div class="form-group">
        <label for="pwd">Password:</label>
        <input type="password" class="form-control" id="pwd">
      </div>
      <div class="checkbox">
        <label><input type="checkbox"> Remember me</label>
      </div>
      <button type="submit" class="btn btn-default">Submit</button>
    </form>
```
#### Vérification adresse mail
```html
  <input type="text" title="email" required pattern="[^@]+@[^@]+\.[a-zA-Z]{2,6}" />
```
#### Bouton
```html
    <button type="button" name="myButton" formtarget="_self">Click me!</button>
```
#### Checkbox 
```html
    <label><input type="checkbox" name="myCheckbox"> Check me!</label>
```
#### Radio button
```html
    <label><input type="radio" name="myCheckbox"> Check me!</label>
```
#### Choix de couleur
```html
    <input type="color" name="myColorField">
```
#### Date
```html
    <input type="date" name="myDateTime">
```
#### Email
```html
    <input type="email" name="myEmailField">
```
#### Import de fichier
```html
    <input type="file" name="myFileBrowser">
```
#### Image bouton
```html
    <input type="image" src="img/seagull.jpg" name="myImageButton" alt="Submit" width="70" height="70">
```
#### Choix d'un nombre
```html
    <input type="number" name="myInput" value="0" min="0" max="100" step="1">
```
#### Choix d'une échelle
```html
    <input type="range" name="myInput" value="0" min="0" max="100" step="1">
```
#### Champ mot de passe 
```html
    <input type="password" name="myPassword">
```
#### Champ de recherche
```html
    <input type="search" name="mySearch" placeholder="Search...">
```
#### Bouton submit
```html
    <input type="submit" value="Submit" name="mySubmit">
```
#### Champ téléphone
```html
    <input type="tel" name="myTel" placeholder="(123) 555-9876">
```
#### Champ texte
```html
    <input type="text" name="myText">
```
#### Champ textarea
```html
    <textarea name="myTextarea" placeholder="Leave a comment." cols="30" rows="15" required></textarea>
```
#### Champ URL
```html
    <input type="url" name="myUrl">
```
### Médias
#### Audio
```html
    <audio controls>
      <source src="media/sound.ogg" type="audio/ogg">
      <source src="media/sound.mp3" type="audio/mpeg">
      <source src="media/sound.wav" type="audio/wav">
    </audio>
```
#### Image
```html
    <img src="img/ottawa.jpg" alt="My Image" height="170" width="390">
```
#### Vidéo
```html
    <video controls autoplay loop muted height="220" width="390">
      <source src="media/video.mp4" type="video/mp4">
      <source src="media/video.ogv" type="video/ogg">
      <source src="media/video.webm" type="video/webm">
    </video>
```
### Texte
#### Texte bi-directionnel
```html
    <bdo dir="rtl">This text is backwards!</bdo>
    <bdo dir="rtl">This text is backwards!</bdo>
```
#### Bold
```html
    <b>This text is bold.</b>
```
#### Texte cité
```html
    <cite>This is cited text.</cite>
```
#### Code
```html
    <code>printf("This is computer code.\n");</code>
```
#### Texte barré
```html
    <s>This text is incorrect.</s>
```
#### Italique
```html
    <i>This is italic text.</i>
```
#### Surligné
```html
    <mark>This is highlighted text.</mark>
```
#### Citation courte
```html
    <q>This is a quote.</q>
```
#### Citation longue
```html
    <blockquote>This is a quote.</blockquote>
```
#### Superscript et subscript
```html
    <sup>This is superscript.</sup>
    <sub>This is superscript.</sub>
```
#### Souligné
```html
    <u>This text is underlined.</u>
```
### Autres
#### Détails
```html
    <details open>
      <summary>This is a short summary.</summary>
      <p>These are the additional details.</p>
    </details>
```
#### Dialog
```html
    <dialog open>
      <p>Some text...</p>
    </dialog>
```
#### Hyperlink
```html
    <a href="http://" target="_blank">click here</a>
```
#### IFrame
```html
    <iframe src="http://apple.com" name ="myIframe"></iframe>
```
#### Meter
```html
    <meter value="0.6">60%</meter>
```
#### Progress bar
```html
    <progress value="67" max="100">67%</meter>
```
#### Baselink
```html
    <base href="http://" target="_blank">
```

--------------------------


## CSS
### Animation
#### Animation
```css
    animation: myMove 2s linear 0s 1 normal;
```
```css
    @keyframes myMove {
    0% { left: 0px; }
    100% { left: 200px; }
    }
```
### Background
#### Background Color
```css
   background-color: #ff0000;
```
#### Background Gradient
```css
    background: linear-gradient(123deg, rgba(0, 0, 0, 0.79), rgba(255, 255, 255, 0.45));
```
#### Background Image
```css
    background: #000000 url(img/ottawa.jpg) repeat left top;
    background-size: cover;
```
### Border
#### Border
* Bordure uniforme
```css
    border: 2px solid #000000;
    border: 2px dotted #000000;
    border: 2px dashed #000000;
    border: 2px double #000000;
    border: 2px groove #000000;
    border: 2px ridge #000000;
    border: 2px inset #000000;
    border: 2px outset rgba(0, 0, 0, 0.72);
```
* Côtés différents
```css
    border-left: 28px outset rgba(0, 0, 0, 0.72);
    border-top: 2px solid rgba(0, 0, 0, 1);
    border-right: 2px solid rgba(0, 0, 0, 1);
    border-bottom: 2px solid rgba(0, 0, 0, 1);
```

#### Border Image
* Stretch
```css
    border: 15px solid transparent;
    -webkit-border-image: url('img/border-image.png') 30 30 stretch;
```
* Repeat
```css
    border: 15px solid transparent;
    -webkit-border-image: url('img/border-image.png') 30 30 repeat;
```

#### Border Radius
* Radius uniforme
```css
    border-radius: 28px;
```
* Radius différents
```css
    border-radius: 43px 28px 76px 55px;
```

#### Box resize
```css
    resize: both;
    overflow: auto;
```
#### Box shadow
```css
    box-shadow: 4px 4px 15px 1px #000000;
```
#### Outline
```css
    outline: 4px solid rgba(255, 247, 13, 0.64);
```
#### Overflow (X,Y)
```css
    overflow: auto;
    overflow: visible;
    overflow: hidden;
    overflow: scroll;
```
#### Visiblity
```css
    visibility: visible;
    visibility: hidden;
```
### Color
#### Opacity
```css
    opacity: 0.62;
```
### Filter
#### Blur
```css
    -webkit-filter: blur(4px);
    filter: blur(4px);
```
#### Brightness
```css
    -webkit-filter: brightness(2.7);
    filter: brightness(2.7);
```
#### Contrast
```css
    -webkit-filter: contrast(2.7);
    filter: contrast(2.7);
```
#### Grayscale
```css
    -webkit-filter: grayscale(0.58);
    filter: grayscale(0.58);
```
#### Hue-rotate
```css
    -webkit-filter: hue-rotate(180deg);
    filter: hue-rotate(180deg);
```
#### Invert
```css
    -webkit-filter: invert(0.88);
    filter: invert(0.88);
```
#### Saturate
```css
    -webkit-filter: saturate(5);
    filter: saturate(5);
```
#### Sepia
```css
    -webkit-filter: sepia(1);
    filter: sepia(1);
```
### Layout
#### Columns
```css
    -moz-column-count: 2;
    -webkit-column-count: 2;
    column-count: 2;
```
#### Display
```css
    display: block;
    display: inline;
    display: inline-block;
    display: list-item;
    display: table;
    display: none;
```
### List
#### List style
```css
    list-style: disc inside;
    list-style: decimal inside;
    list-style: upper-latin inside;
    list-style: upper-roman outside;
```
### Miscellaneous
#### Cursor
```css
    cursor: default;
    cursor: pointer;
    cursor: col-resize;
    cursor: grab;
    cursor: grabbing;
    cursor: nw-resize;
```
### Text
#### Letter spacing
```css
    letter-spacing: 13px;
```
#### Line height
```css
    line-height: 2;
```
#### Tab size
```css
    -moz-tab-size: 8;
    -o-tab-size: 8;
    tab-size: 8;
```
#### Text align
```css
    text-align: left;
    text-align: center;
    text-align: right;
    text-align: justify;
```
#### Text decoration
```css
    text-decoration: none;
    text-decoration: underline;
    text-decoration: overline;
    text-decoration: line-through;
```
#### Text indent
```css
    text-indent: 17px;
```
#### Text shadow
```css
    text-shadow: 2px 2px 7px rgba(56, 27, 27, 0.7);
```
#### Text transform
```css
    text-transform: capitalize;
    text-transform: uppercase;
    text-transform: lowercase;
```
#### White space
```css
    white-space: normal;
    white-space: nowrap;
    white-space: pre;
    white-space: pre-line;
    white-space: pre-wrap;
```
#### Word break
```css
    word-break: break-all;
    word-break: normal;
    word-break: keep-all;
```
#### Word spacing
```css
word-spacing: 30px;
```
#### Word wrap
```css
word-wrap: normal;
word-wrap: break-word;
```
### Transform
#### Perspective
* Elément parent
```css
position: relative;
-webkit-perspective: 150px;
perspective: 150px;
```
* Elément enfant
```css
-webkit-transform: rotateX(10deg) rotateY(-5deg);
transform: rotateX(10deg) rotateY(-5deg);
```

### Transition
#### Transition
```css
-webkit-transform: scale(1) rotate(2deg) skewY(-5deg) skewX(10deg);
transform: scale(1) rotate(2deg) skewY(-5deg) skewX(10deg);
```

--------------------------

## Markdown

### Paragraphes
    Ceci est un paragraphe de texte.
    Ceci est un autre paragraphe de texte !
Ceci est un paragraphe de texte.
Ceci est un autre paragraphe de texte !
### Gras
    **mot en gras**
**mot en gras**
### Italique
    *italique*
*italique*
### Titres
    # titre 1
    ## titre 2
    ### titre 3
    #### titre 4
    ##### titre 5
    ###### titre 6
### Listes
    * puce 1
    * puce 2
    ** puce 2.1
    ** puce 2.2
      * puce 2.2.1
      * puce 2.2.2
* puce 1
* puce 2
  * puce 2.2.1
  * puce 2.2.2

### Listes numérotées 
    1. item 1
    2. item 2
    3. item 3
1. item 1
2. item 2
3. item 3

### Citations
    > Voici la citation citée par le citateur
> Voici la citation citée par le citateur
> Voici la citation citée par le citateur
> Voici la citation citée par le citateur
### Bloc de code
         il suffit de précéder le code par 2 tabs
    il suffit de précéder le code par 2 tabs
### Code en ligne
    `code en ligne`
`code en ligne`
### Liens
    Voici le lien de [mon Portfolio](https://tugdualharel.fr) pour vous rendre compte de mon talent de génie.
Voici le lien de [mon Portfolio](https://tugdualharel.fr) pour vous rendre compte de mon talent de génie.# MEMENTO

---------------------------

## Sommaire
* Liens [#link](https://denikine.github.io/Memento/#liens)
* Ruby on Rails [#link](https://denikine.github.io/Memento/#ruby-on-rails)
* Postgresql [#link](https://denikine.github.io/Memento/#postgresql)
* Git [#link](https://denikine.github.io/Memento/#git)
* Linux [#link](https://denikine.github.io/Memento/#linux)
* HTML [#link](https://denikine.github.io/Memento/#html)
* CSS [#link](https://denikine.github.io/Memento/#css)
* PHP [#link](https://denikine.github.io/Memento/#php)
* Markdown [#link](https://denikine.github.io/Memento/#markdown)

--------------------------------

## Liens
* Stack Edit
[https://stackedit.io/app#](https://stackedit.io/app#)
* Memento online
[https://denikine.github.io/Memento/](https://denikine.github.io/Memento/)
* Mediabox
[https://mediabox.2-ride.com/dashboards/products](https://mediabox.2-ride.com/dashboards/products)
* Mediabox Github
[https://github.com/bsignoret2/2ride-mediabox](https://github.com/bsignoret2/2ride-mediabox)
* Convert HTML to HAML
[https://htmltohaml.com/](https://htmltohaml.com/)
* Ultimate Website Generator
[https://webcode.tools/](https://webcode.tools/)

---------------------------------

## Ruby on Rails

### Code utilisé en projets
* ProductType find_by et création si non existant
```ruby
    product_type = ProductType.find_by(name: "Casque Shark")
    product_type ||= ProductType.create(name: "Casque Shark")
```
* Product find and destroy
```ruby
    Product.find(9).product_assets.update_all(entity_id: 11)
    Product.find(9).destroy
```
* Connexion au FTP et téléchargement d'un fichier csv
```ruby
    c = Ftp::Base.connection
    data = c.download!("/home/bertrand/public_html/#{Rails.env}/input/bering_segura_products.csv")
```
* Création d'un array
```ruby
    product_feature_array = [
          ["FAMILLE_PRODUIT", u[8]],
          ["SOUS-FAMILLE", u[9], u[92], u[93], u[94], u[95]],
          ["WHEIGHT", u[83]],
          ["NOUVEAUTE", product.is_new],
          ["HAUTE VISIBILITE", ActiveModel::Type::Boolean.new.cast(u[100])],
        ]
```
* Temps d'exécution
```ruby
    start_time = Time.now
    #code
    #code
    #code
    end_time = Time.now
    puts "Time elapsed: #{end_time - start_time}s"
```
* If
```ruby
    product_feature_array.each do |couple|
      if condition
        
      end
```
* Affichage
```ruby
    ActiveRecord::Base.logger.level = 1 # non-affichage des lignes inutiles de la console
      ProductType.all.each do |product_type|
        puts "#{product_type.title},#{product_type.products.map}"
```
* Utilisation de **Pluck**
```ruby
    if !product_type.nil?
       product_features_names = product_type.product_features.pluck(:name)
       attributes.push(product_type.product_features.pluck(:title))
    end
```
* Langue
```ruby
    I18n.locale = :fr
```
* Mode édition postgresql
```bash
    psql nom-de-la-base
```
* Ajout champs avec rails
```ruby
    rails g migration addVisibleOnDigitalCatalog2ToProducts visible_on_digital_catalog_2:boolean
    rake db:migrate
```
* Modification password accès db
```ruby
    rails c
    User.first
    User.first.update(password:"password",password_confirmation:"password")
```
* Création d'un bouton sur une nouvelle page dashboards/actions
 * Insertion de la méthode get dans config/routes.rb
```ruby
    get 'dashboards/actions'
```
 * Création du fichier views/dashboards/actions.html.haml et utilisation de [https://htmltohaml.com/](https://htmltohaml.com/)
 * Dans controllers/dashboards_controller.rb, insérer :
```ruby
    def actions

    end
```
* Génération d'un CSV
 * Dans models/product_color.rb
 ```ruby
 def self.to_csv(product_type_name = nil)
    attributes = %w{reference product_type_title product_name}
    if product_type_name.present?
      product_type = ProductType.find_by(name: product_type_name)
      if !product_type.nil?
        product_features_names = product_type.product_features.pluck(:name)
        attributes.push(product_type.product_features.pluck(:title))
      end
    end
    CSV.generate(headers: true) do |csv|
      csv << attributes

      all.each do |product_color|
        row = Array.new
        product = product_color.product
        row.push(product_color.reference)
        row.push(product_color.product.product_type.title)
        row.push(product.name)
        if !product_type.nil?
          product_features_names.each do |product_feature_name|
            if product.product_feature_value(product_feature_name).present?
              row.push(product.product_feature_value(product_feature_name))
            else
              row.push(product_color.product_feature_value(product_feature_name))
            end
          end
        end
        csv << row
      end
    end
  end
```
 * Dans controller/product_colors_controller.rb
 ```ruby
 def index
    @product_colors = ProductColor
    @product_colors = @product_colors.where(product_id: Product.where(brand_id: Brand.find_by(name: params[:brand]).id).pluck(:id)) if params[:brand].present?
    @product_colors = @product_colors.where(product_id: Product.where(product_type_id: ProductType.find_by(name: params[:product_type_name]).id).pluck(:id)) if params[:product_type_name].present?
    @product_colors = @product_colors.where(visible_on_mediabox: true) if params[:visible_on_mediabox].present?
    @product_colors = @product_colors.where(visible_on_b2c: true) if params[:visible_on_b2c].present?
    @product_colors = @product_colors.where(visible_on_b2b: true) if params[:visible_on_b2b].present?
    @product_colors = @product_colors.where(visible_on_digital_catalog: true) if params[:visible_on_digital_catalog].present?

    respond_to do |format|
      format.csv { send_data @product_colors.to_csv(product_type_name = params[:product_type_name]), filename: "product_colors-#{Date.today}.csv" }
    end
  end
 ```
 * Dans config/sidekiq.yml
 ajouter `csv-export` dans la liste des queues
 * Dans products.html.haml
 ```haml
 %tr.d-flex
    %th Export
    %td= link_to "Product Size", product_sizes_path(format: "csv", brand_name: params[:brand_name], product_type_name: params[:product_type_name], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog]), class: "btn btn-primary btn-sm"
    %td= link_to "Product Color", product_colors_path(format: "csv", brand_name: params[:brand_name], product_type_name: params[:product_type_name], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog]), class: "btn btn-primary btn-sm"
    %td= link_to "Export in FTP ProductColorPicture", dashboards_products_path(brand_name: params[:brand_name], product_type_name: params[:product_type_name], display_product_features: params[:display_product_features], display_sizes: params[:display_sizes], visible_on_mediabox: params[:visible_on_mediabox], visible_on_b2c: params[:visible_on_b2c], visible_on_b2b: params[:visible_on_b2b], visible_on_digital_catalog: params[:visible_on_digital_catalog], export_product_color_assets: true), class: "btn btn-primary btn-sm"
  ```

### Cheat Sheet 1
* Nouvelle application
```ruby
# Installation de la gemme rails
gem install rails
# Génération d'une nouvelle app rails avec support postgres
rails new my_app --database=postgresql
# Initialisation dela bdd
rake db:create
# Démarrage du serveur rails
rails s
```
* Routes
```ruby
# Création d'une route qui lie une url à l'action d'un controller
# config/routes.rb
get 'welcome' => 'pages#home'
# shorthand for connection a route to a controller/action
# config/routes.rb
get 'photos/show'
# équivalent à
get 'photos/show', :to 'photos#show'
get 'photos/show' => 'photos/show'
# création automatique des routes pour une RESTful resource
#confi/routes.rb
resources :photos
```
* Controllers
```ruby
rails g controller photos
# generate a new controller with default actions, routes and views
rails g controller Photos index show
```
* Models
```ruby
# generate a model and create a migration for the table
rails g model Photos
# generate a model and create a migration with table columns
rails g model Photo path:string caption:text
# The migration automatically created for the above command :
class CreatePhotos < ActiveRecord::Migration
  def change
    create_table :photos do |t|
      t.string :path
      t.text :caption
 
      t.timestamps null: false
    end
  end
end
```
* Migrations
```ruby
# migration data types
:boolean
:date
:datetime
:decimal
:float
:integer
:primary_key
:references
:string
:text
:time
:timestamp
```
* Scaffolding
```ruby
rails g scaffold Photo path:string caption:text
rake db:migrate
```
* Rake
```ruby
#view all the routes in an application
rake routes
# seed the database with sample data from db/seeds.rb
rake db:seed
#run any pending migrations
rake db:migrate
# rollback the last migration performed
rake db:rollback
```
* Path Helpers
```ruby
# creating a path helper for a route
get '/photos/:id', to: 'photos#show', as: 'photo'
# app/controllers/photos_controller.rb
@photo = Photo.find(17)
# view for the action
<%= link_to 'Photo Record', photo_path(@photo) %>
# config/routes.rb
resources :photos
```
* Form Helpers
```ruby
# app/controllers/photos_controller.rb
def new
  @photo = Photo.new
end
```
```html
# ERB view
<%= form_for @photo, url: {action: "create"}, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :path %>
  <%= f.text_area :caption, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```
```html
<!-- HTML output -->
<form accept-charset="UTF-8" action="/photos/create" method="post" class="nifty_form">
  <input id="photos_path" name="photo[path]" type="text" />
  <textarea id="photos_caption" name="photo[caption]" cols="60" rows="12"></textarea>
  <input name="commit" type="submit" value="Create" />
</form>
```
```ruby
<%= form_tag("/search", method: "get") do %>
  <%= label_tag(:q, "Search for:") %>
  <%= text_field_tag(:q) %>
  <%= submit_tag("Search") %>
<% end %>
```
```html
<form accept-charset="UTF-8" action="/search" method="get">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <label for="q">Search for:</label>
  <input id="q" name="q" type="text" />
  <input name="commit" type="submit" value="Search" />
</form>
```

### Cheat Sheet 2
* Création d'une nouvelle app
```ruby
rails new name
# create and name new app

bundle install
# install the gem's version specified in the Gemfile.lock

bundle install - without production
# exclure les gems du groupe production

bundle update
# update les gem dependencies aux dernières versions
```
* Environnements et navigateurs
```ruby
rails server
rails s
# access the app via browser

rails console
rails c
# access app via dev environment

rails console - sandbox
# test code without changing data

rails console test
# run the console in test environment

reload!
# use if changed source code and want changes reflected in the console without restart

rails server - environment production
# run rails app in prod mode
```
* Generate
```ruby
rails generate scaffold Post name: string title: string content: text

rails generate controller Posts or rails g controller Posts:

rails generate model Post:

rails generate model Post name: string title: string 
```
* Migration
```ruby
rails db:migrate
# runs the migration of a model and ist attributes

rails generate migration migration_description
# easiest way to chang something in the bdd schema is to generate the migration

rails db:migrate:reset
# drop the bdd information and run migration on a new one

rails db:seed
# loads data from db/seeds.rb
```
* When messing things up
```ruby
rails destroy model Post
# almost anything created with the generate command can be destroyed with the destroy command

rails db:rollback
# undoes the last migration, you can edit the file and run rails db:migrate again

rails db:migrate VERSION=0
# use this to rollback all migrations back to a target migration with a version number
```
* Testing
```ruby
rails test
rails t
# run the test suite to verifiy if our test apsses or not

rails generate integration_test site_layout
#  integration tests are used to test how various parts of your application interact. Here we are creating an integration test named “site_layout.test.rb” inside the “test/integration” folder.

rails test: integration
# only runs a specific section of tests, in this case, it will only run the integration tests.
```
* Routes
```ruby
rails routes
# get complete list of the available routes in your application
```
* Deploying with Heroku
```ruby
# utiliser si la branche actuelle n'est pas une master branch : git push heroku yourbranch:master
git status
git add -A
git commit -m "Description of commit"
git push
rails test
git push heroku
heroku pg:reset DATABASE
heroku run rails db:migrate
heroku run rails db:seed
heroku open
```
* Launch Sidekiq
```ruby
bundle exec sidekiq -C config/sidekiq.yml
```

### Cheat Sheet 3
* General concepts
```ruby
# comment
# comment text

# variables
variable = some_value

# console output
puts something

# call a method
object.method(arguments)

# define a method
def name(parameter)
    # method body
end

# equality
object == other

# inequality
object != other

# decisions with if
if condition
    # happens when true
else
    # happens when false
end

# constants
CONSTANT = some_value
```
* Numbers
```ruby
#normal number
number_of_your_choice
0
-11
42

# decimals
main.decimal
3.2
-5.0

# basic math
n operator m
2 + 3 = 5
8 * 7 = -2

# comparison
n operator m
12 > 3 => true
```
* Strings
```ruby
# create
'a string'
'hello world'

# interpolation
"a string and an #{expression}"
"Email : #{user.email}"
"The total is #{2+2}"

# length
string.length
"Hello".length = 5

# concatenate
string + string 2
"Hello "+ "reader"

# substitute
string.gsub(a_string, subtstitute)

#access
string[position]
"Hello"[1] # => "e"
```
* Arrays
```ruby 

```

--------------------

## Postgresql
### Création d'une table avec Postgresql (similaire à mysql)
```sql
    CREATE TABLE persons (
      id SERIAL,
      first_name VARCHAR(50),
      last_name VARCHAR(50),
      dob DATE,
      email VARCHAR(255),
      PRIMARY KEY (id)
    )
```
### Création user & base avec postgres
```sql
    sudo -u postgres psql
    CREATE USER "guy_on_stackoverflow" with password 'keepitonthedl';
    CREATE DATABASE "dcaclab_development" owner "guy_on_stackoverflow";
```
### Importer un CSV dans une table
```sql
    COPY persons(first_name, last_name, dob, email)
    FROM 'C:\sampledb\persons.csv'
    DELIMITER ','
    CSV HEADER;
```
### Création nouveau champ dans postgresql
```sql
    ALTER TABLE nom-de-la-table
    ADD COLUMN nom-de-la-colonne type-de-donnée;
```
### Infos à renseigner dans database.yml
```yml
    development:
      adapter: postgresql
      encoding: unicode
      database: kickrstack_development
      host: localhost
      pool: 5
      username: kickrstack
      password: secret
```

--------------------------



## Git

### Création nouvelle branche
```git
    git checkout develop
    git pull
    git checkout -b nouvelle-branche
    git push -u origin nouvelle-branche
```

### Publication branche locale
```git
    git checkout branche-locale
    git status
    git commit -am "indication de la modif"
    git push
    git push -u origin branche-locale
```


-----------------------

## Terminal
### Sublime text open with terminal
    subl
### Table Plus open with terminal
    tableplus
### Cheat Sheet
```bash
# change directory
cd directory
cd my_app
cd my_app/app/controllers

# list contents directory
ls directory
# windows : dir directory
ls
ls my_app

# directory you are currently in
pwd

# create a new directory
mkdir name
mkdir rails

# delete a file
rm file
# windows : del file

# delete a directory
rm -r folder
# windows : rd folder

# strarting a program
program arguments
firefox
firefox railsgirlsberlin.de
irb

# abort the program
ctrl+C
```



--------------------------

## HTML
### Template de base
```html
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>Untitled</title>
        <meta name="description" content="This is an example of a meta description.">
        <link rel="stylesheet" type="text/css" href="theme.css">

        <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="HandheldFriendly" content="true">

        <!--[if lt IE 9]>
      <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
        <![endif]-->
      </head>
      <body>
        
      </body>
    </html>
```
### Metas
```html
    <meta charset="utf-8">
    <title>Titre de la page</title>
    <meta name="description" content="Description de la page en quelques mots">
    <meta name="robots" content="index, follow">
    <meta name="author" content="auteur de la page">
```
### Formulaire HTML
```html
    <form role="form">
      <div class="form-group">
        <label for="email">Email address:</label>
        <input type="email" class="form-control" id="email">
      </div>
      <div class="form-group">
        <label for="pwd">Password:</label>
        <input type="password" class="form-control" id="pwd">
      </div>
      <div class="checkbox">
        <label><input type="checkbox"> Remember me</label>
      </div>
      <button type="submit" class="btn btn-default">Submit</button>
    </form>
```
#### Vérification adresse mail
```html
  <input type="text" title="email" required pattern="[^@]+@[^@]+\.[a-zA-Z]{2,6}" />
```
#### Bouton
```html
    <button type="button" name="myButton" formtarget="_self">Click me!</button>
```
#### Checkbox 
```html
    <label><input type="checkbox" name="myCheckbox"> Check me!</label>
```
#### Radio button
```html
    <label><input type="radio" name="myCheckbox"> Check me!</label>
```
#### Choix de couleur
```html
    <input type="color" name="myColorField">
```
#### Date
```html
    <input type="date" name="myDateTime">
```
#### Email
```html
    <input type="email" name="myEmailField">
```
#### Import de fichier
```html
    <input type="file" name="myFileBrowser">
```
#### Image bouton
```html
    <input type="image" src="img/seagull.jpg" name="myImageButton" alt="Submit" width="70" height="70">
```
#### Choix d'un nombre
```html
    <input type="number" name="myInput" value="0" min="0" max="100" step="1">
```
#### Choix d'une échelle
```html
    <input type="range" name="myInput" value="0" min="0" max="100" step="1">
```
#### Champ mot de passe 
```html
    <input type="password" name="myPassword">
```
#### Champ de recherche
```html
    <input type="search" name="mySearch" placeholder="Search...">
```
#### Bouton submit
```html
    <input type="submit" value="Submit" name="mySubmit">
```
#### Champ téléphone
```html
    <input type="tel" name="myTel" placeholder="(123) 555-9876">
```
#### Champ texte
```html
    <input type="text" name="myText">
```
#### Champ textarea
```html
    <textarea name="myTextarea" placeholder="Leave a comment." cols="30" rows="15" required></textarea>
```
#### Champ URL
```html
    <input type="url" name="myUrl">
```
### Médias
#### Audio
```html
    <audio controls>
      <source src="media/sound.ogg" type="audio/ogg">
      <source src="media/sound.mp3" type="audio/mpeg">
      <source src="media/sound.wav" type="audio/wav">
    </audio>
```
#### Image
```html
    <img src="img/ottawa.jpg" alt="My Image" height="170" width="390">
```
#### Vidéo
```html
    <video controls autoplay loop muted height="220" width="390">
      <source src="media/video.mp4" type="video/mp4">
      <source src="media/video.ogv" type="video/ogg">
      <source src="media/video.webm" type="video/webm">
    </video>
```
### Texte
#### Texte bi-directionnel
```html
    <bdo dir="rtl">This text is backwards!</bdo>
    <bdo dir="rtl">This text is backwards!</bdo>
```
#### Bold
```html
    <b>This text is bold.</b>
```
#### Texte cité
```html
    <cite>This is cited text.</cite>
```
#### Code
```html
    <code>printf("This is computer code.\n");</code>
```
#### Texte barré
```html
    <s>This text is incorrect.</s>
```
#### Italique
```html
    <i>This is italic text.</i>
```
#### Surligné
```html
    <mark>This is highlighted text.</mark>
```
#### Citation courte
```html
    <q>This is a quote.</q>
```
#### Citation longue
```html
    <blockquote>This is a quote.</blockquote>
```
#### Superscript et subscript
```html
    <sup>This is superscript.</sup>
    <sub>This is superscript.</sub>
```
#### Souligné
```html
    <u>This text is underlined.</u>
```
### Autres
#### Détails
```html
    <details open>
      <summary>This is a short summary.</summary>
      <p>These are the additional details.</p>
    </details>
```
#### Dialog
```html
    <dialog open>
      <p>Some text...</p>
    </dialog>
```
#### Hyperlink
```html
    <a href="http://" target="_blank">click here</a>
```
#### IFrame
```html
    <iframe src="http://apple.com" name ="myIframe"></iframe>
```
#### Meter
```html
    <meter value="0.6">60%</meter>
```
#### Progress bar
```html
    <progress value="67" max="100">67%</meter>
```
#### Baselink
```html
    <base href="http://" target="_blank">
```

--------------------------


## CSS
### Animation
#### Animation
```css
    animation: myMove 2s linear 0s 1 normal;
```
```css
    @keyframes myMove {
    0% { left: 0px; }
    100% { left: 200px; }
    }
```
### Background
#### Background Color
```css
   background-color: #ff0000;
```
#### Background Gradient
```css
    background: linear-gradient(123deg, rgba(0, 0, 0, 0.79), rgba(255, 255, 255, 0.45));
```
#### Background Image
```css
    background: #000000 url(img/ottawa.jpg) repeat left top;
    background-size: cover;
```
### Border
#### Border
* Bordure uniforme
```css
    border: 2px solid #000000;
    border: 2px dotted #000000;
    border: 2px dashed #000000;
    border: 2px double #000000;
    border: 2px groove #000000;
    border: 2px ridge #000000;
    border: 2px inset #000000;
    border: 2px outset rgba(0, 0, 0, 0.72);
```
* Côtés différents
```css
    border-left: 28px outset rgba(0, 0, 0, 0.72);
    border-top: 2px solid rgba(0, 0, 0, 1);
    border-right: 2px solid rgba(0, 0, 0, 1);
    border-bottom: 2px solid rgba(0, 0, 0, 1);
```

#### Border Image
* Stretch
```css
    border: 15px solid transparent;
    -webkit-border-image: url('img/border-image.png') 30 30 stretch;
```
* Repeat
```css
    border: 15px solid transparent;
    -webkit-border-image: url('img/border-image.png') 30 30 repeat;
```

#### Border Radius
* Radius uniforme
```css
    border-radius: 28px;
```
* Radius différents
```css
    border-radius: 43px 28px 76px 55px;
```

#### Box resize
```css
    resize: both;
    overflow: auto;
```
#### Box shadow
```css
    box-shadow: 4px 4px 15px 1px #000000;
```
#### Outline
```css
    outline: 4px solid rgba(255, 247, 13, 0.64);
```
#### Overflow (X,Y)
```css
    overflow: auto;
    overflow: visible;
    overflow: hidden;
    overflow: scroll;
```
#### Visiblity
```css
    visibility: visible;
    visibility: hidden;
```
### Color
#### Opacity
```css
    opacity: 0.62;
```
### Filter
#### Blur
```css
    -webkit-filter: blur(4px);
    filter: blur(4px);
```
#### Brightness
```css
    -webkit-filter: brightness(2.7);
    filter: brightness(2.7);
```
#### Contrast
```css
    -webkit-filter: contrast(2.7);
    filter: contrast(2.7);
```
#### Grayscale
```css
    -webkit-filter: grayscale(0.58);
    filter: grayscale(0.58);
```
#### Hue-rotate
```css
    -webkit-filter: hue-rotate(180deg);
    filter: hue-rotate(180deg);
```
#### Invert
```css
    -webkit-filter: invert(0.88);
    filter: invert(0.88);
```
#### Saturate
```css
    -webkit-filter: saturate(5);
    filter: saturate(5);
```
#### Sepia
```css
    -webkit-filter: sepia(1);
    filter: sepia(1);
```
### Layout
#### Columns
```css
    -moz-column-count: 2;
    -webkit-column-count: 2;
    column-count: 2;
```
#### Display
```css
    display: block;
    display: inline;
    display: inline-block;
    display: list-item;
    display: table;
    display: none;
```
### List
#### List style
```css
    list-style: disc inside;
    list-style: decimal inside;
    list-style: upper-latin inside;
    list-style: upper-roman outside;
```
### Miscellaneous
#### Cursor
```css
    cursor: default;
    cursor: pointer;
    cursor: col-resize;
    cursor: grab;
    cursor: grabbing;
    cursor: nw-resize;
```
### Text
#### Letter spacing
```css
    letter-spacing: 13px;
```
#### Line height
```css
    line-height: 2;
```
#### Tab size
```css
    -moz-tab-size: 8;
    -o-tab-size: 8;
    tab-size: 8;
```
#### Text align
```css
    text-align: left;
    text-align: center;
    text-align: right;
    text-align: justify;
```
#### Text decoration
```css
    text-decoration: none;
    text-decoration: underline;
    text-decoration: overline;
    text-decoration: line-through;
```
#### Text indent
```css
    text-indent: 17px;
```
#### Text shadow
```css
    text-shadow: 2px 2px 7px rgba(56, 27, 27, 0.7);
```
#### Text transform
```css
    text-transform: capitalize;
    text-transform: uppercase;
    text-transform: lowercase;
```
#### White space
```css
    white-space: normal;
    white-space: nowrap;
    white-space: pre;
    white-space: pre-line;
    white-space: pre-wrap;
```
#### Word break
```css
    word-break: break-all;
    word-break: normal;
    word-break: keep-all;
```
#### Word spacing
```css
word-spacing: 30px;
```
#### Word wrap
```css
word-wrap: normal;
word-wrap: break-word;
```
### Transform
#### Perspective
* Elément parent
```css
position: relative;
-webkit-perspective: 150px;
perspective: 150px;
```
* Elément enfant
```css
-webkit-transform: rotateX(10deg) rotateY(-5deg);
transform: rotateX(10deg) rotateY(-5deg);
```

### Transition
#### Transition
```css
-webkit-transform: scale(1) rotate(2deg) skewY(-5deg) skewX(10deg);
transform: scale(1) rotate(2deg) skewY(-5deg) skewX(10deg);
```

--------------------------

## Markdown

### Paragraphes
    Ceci est un paragraphe de texte.
    Ceci est un autre paragraphe de texte !
Ceci est un paragraphe de texte.
Ceci est un autre paragraphe de texte !
### Gras
    **mot en gras**
**mot en gras**
### Italique
    *italique*
*italique*
### Titres
    # titre 1
    ## titre 2
    ### titre 3
    #### titre 4
    ##### titre 5
    ###### titre 6
### Listes
    * puce 1
    * puce 2
    ** puce 2.1
    ** puce 2.2
      * puce 2.2.1
      * puce 2.2.2
* puce 1
* puce 2
  * puce 2.2.1
  * puce 2.2.2

### Listes numérotées 
    1. item 1
    2. item 2
    3. item 3
1. item 1
2. item 2
3. item 3

### Citations
    > Voici la citation citée par le citateur
> Voici la citation citée par le citateur
> Voici la citation citée par le citateur
> Voici la citation citée par le citateur
### Bloc de code
         il suffit de précéder le code par 2 tabs
    il suffit de précéder le code par 2 tabs
### Code en ligne
    `code en ligne`
`code en ligne`
### Liens
    Voici le lien de [mon Portfolio](https://tugdualharel.fr) pour vous rendre compte de mon talent de génie.
Voici le lien de [mon Portfolio](https://tugdualharel.fr) pour vous rendre compte de mon talent de génie.
### Images
    ![titre](lien-de-l-image.png)
![titre](lien-de-l-image.png)
### Barre de séparation
    -----------------
-----------------
### Images
    ![titre](lien-de-l-image.png)
![titre](lien-de-l-image.png)
### Barre de séparation
    -----------------
-----------------
<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoidGl0bGU6IE1FTUVOVE9cbmF1dGhvcj
ogVHVnZHVhbFxudGFnczogJ21lbWVudG8sIHJ1YnksIHNxbCwg
bWFya2Rvd24sIGh0bWwsIGNzcywgcGhwJ1xuc3RhdHVzOiBpbi
Bwcm9ncmVzc1xuZXh0ZW5zaW9uczpcbiAgcHJlc2V0OiBjb21t
b25tYXJrXG4iLCJoaXN0b3J5IjpbMjI3NzY4MDcyLC00OTc3OT
g3NDksMTc4MzkwOTEzLDE4OTM2NDQxMjQsLTE4NDY4NjU0NzEs
LTg1NDg4MjY3NiwtMTMzODk2NTU5OCwtNTM3MjM5NTU3LC0xNT
kyNTY2MzczLDExMzYyMjQzMDNdfQ==
-->