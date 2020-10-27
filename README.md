
# MEMENTO




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



### ProductType find_by et création si non existant
```ruby
    product_type = ProductType.find_by(name: "Casque Shark")
    product_type ||= ProductType.create(name: "Casque Shark")
```
### Product find and destroy
```ruby
    Product.find(9).product_assets.update_all(entity_id: 11)
    Product.find(9).destroy
```

### Connexion au FTP et téléchargement d'un fichier csv
```ruby
    c = Ftp::Base.connection
    data = c.download!("/home/bertrand/public_html/#{Rails.env}/input/bering_segura_products.csv")
```

### Création d'un array
```ruby
    product_feature_array = [
          ["FAMILLE_PRODUIT", u[8]],
          ["SOUS-FAMILLE", u[9], u[92], u[93], u[94], u[95]],
          ["WHEIGHT", u[83]],
          ["NOUVEAUTE", product.is_new],
          ["HAUTE VISIBILITE", ActiveModel::Type::Boolean.new.cast(u[100])],
        ]
```

### Temps d'exécution
```ruby
    start_time = Time.now
    #code
    #code
    #code
    end_time = Time.now
    puts "Time elapsed: #{end_time - start_time}s"
```

### If
```ruby
    product_feature_array.each do |couple|
      if condition
        
      end
```

### Affichage
```ruby
    ActiveRecord::Base.logger.level = 1 # non-affichage des lignes inutiles de la console
      ProductType.all.each do |product_type|
        puts "#{product_type.title},#{product_type.products.map}"
```

### Langue
```ruby
    I18n.locale = :fr
```

### Mode édition postgresql
```ruby
    psql nom-de-la-base
```

### Ajout champs avec rails
```ruby
    rails g migration addVisibleOnDigitalCatalog2ToProducts visible_on_digital_catalog_2:boolean
    rake db:migrate
```

### Modification password accès db
    rails c
    User.first
    User.first.update(password:"password",password_confirmation:"password")

### Création d'un bouton sur une nouvelle page dashboards/actions
1. Insertion de la méthode get dans config/routes.rb
```ruby
    get 'dashboards/actions'
```
2. Création du fichier views/dashboards/actions.html.haml et utilisation de [https://htmltohaml.com/](https://htmltohaml.com/)
3. Dans controllers/dashboards_controller.rb, insérer :
```ruby
    def actions

    end
```

### Création d'un bouton d'export en csv
1. 




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



## Linux



### Sublime text open with terminal
    subl

### Table Plus open with terminal
    tableplus

### Nano

#### Validation
    ctrl+o



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
overflow: auto
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



#### Transform
```css

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
** puce 2.1
** puce 2.2
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




<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoidGl0bGU6IE1FTUVOVE9cbmF1dGhvcj
ogVHVnZHVhbFxudGFnczogJ21lbWVudG8sIHJ1YnksIHNxbCwg
bWFya2Rvd24sIGh0bWwsIGNzcywgcGhwJ1xuc3RhdHVzOiBpbi
Bwcm9ncmVzc1xuZXh0ZW5zaW9uczpcbiAgcHJlc2V0OiBjb21t
b25tYXJrXG4iLCJoaXN0b3J5IjpbMTc4MzkwOTEzLDE4OTM2ND
QxMjQsLTE4NDY4NjU0NzEsLTg1NDg4MjY3NiwtMTMzODk2NTU5
OCwtNTM3MjM5NTU3LC0xNTkyNTY2MzczLDExMzYyMjQzMDNdfQ
==
-->