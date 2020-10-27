
# MEMENTO



## Liens

### Mediabox
[https://mediabox.2-ride.com/dashboards/products](https://mediabox.2-ride.com/dashboards/products)



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



--------------------------


## HTML

### Formulaire HTML

#### Bouton

```html
    <button type="button" name="myButton" formtarget="_self">Click me!</button>
```

#### Checkbox 
```html
    <label><input type="checkbox" name="myCheckbox"> Check me!</label>
```

#### Radio button
```HTML
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
b25tYXJrXG4iLCJoaXN0b3J5IjpbNjA1MjI3NDI2LC01MzcyMz
k1NTcsLTE1OTI1NjYzNzMsMTEzNjIyNDMwM119
-->