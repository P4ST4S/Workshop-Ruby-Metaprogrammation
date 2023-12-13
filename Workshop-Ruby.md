# Workshop Ruby - les bases de la Métaprogrammation

## Prérequis

- [Ruby](https://www.ruby-lang.org/fr/documentation/installation/)

## Etape 1 : Introduction à la métaprogrammation

### Qu'est-ce que la métaprogrammation ?

La métaprogrammation est une technique de programmation qui consiste à écrire des programmes qui manipulent d'autres programmes (ou eux-mêmes) comme des données, ou qui produisent de tels programmes. C'est une forme d'abstraction qui permet de décrire des [algorithmes génériques](https://a3soft.fr/algorithme-sequentiel/).

### Pourquoi la métaprogrammation en Ruby ?

La métaprogrammation est une technique très utilisée en Ruby. Elle permet de créer des programmes plus génériques et plus flexibles. Elle permet également de créer des programmes plus courts et plus lisibles.

## Etape 2 : Introduction aux concepts clés en Ruby

### Objets et classes 

**Exercice :** Dans un fichier `Person.ru`, créez une classe `Person` avec des méthodes pour définir et obtenir le nom et l'âge. Utilisez [define_method](https://ruby-doc.org/3.2.2/Module.html#method-i-define_method) pour ces méthodes plutôt que la définition habituelle.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  [:name, :age].each do |prop|
    define_method("#{prop}") { instance_variable_get("@#{prop}") }
    define_method("#{prop}=") { |val| instance_variable_set("@#{prop}", val) }
  end
end
```

</details>

### Méthodes et Constantes

**Exercice :** Ajoutez une méthode à la classe `Person` pour afficher toutes les constantes et méthodes de la classe. Utilisez les méthodes [constants](https://ruby-doc.org/3.2.2/Module.html#method-i-constants) et [instance_methods](https://ruby-doc.org/3.2.2/Module.html#method-i-instance_methods) de Ruby.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def self.details
    puts "Constantes: #{self.constants}"
    puts "Méthodes: #{self.instance_methods(false)}"
  end
end
```

</details>

### Portée et visibilité

**Exercice :** Modifiez la classe `Person` pour y inclure des méthodes [privées](https://ruby-doc.org/3.2.2/Module.html#method-i-private) et [protégées](https://ruby-doc.org/3.2.2/Module.html#method-i-protected), et démontrez leur comportement en créant des instances de `Person`.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def public_method
    "Ceci est une méthode publique."
  end

  private

  def private_method
    "Ceci est une méthode privée."
  end

  protected

  def protected_method
    "Ceci est une méthode protégée."
  end
end
```

</details>

Maintenant essayez d'utiliser [respond_to?](https://ruby-doc.org/3.2.2/Object.html#method-i-respond_to-3F) pour vérifier si une instance de `Person` peut appeler une méthode privée ou protégée.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def interact_with_another_person(other_person)
    if other_person.respond_to?(:protected_method)
      "Interagir avec une méthode protégée: #{other_person.protected_method}"
    else
      "Impossible d'interagir avec une méthode protégée."
    end
  end
end
```

</details>

#### Correction complète

Voici une correction complète de l'étape 2. Vous pouvez l'utiliser pour vérifier votre travail.

En combinant tous ces éléments, nous obtenons la classe complète suivante :

<details><summary markdown="span">Voir la correction complète</summary>

```ruby
class Person
  [:name, :age].each do |prop|
    define_method("#{prop}") { instance_variable_get("@#{prop}") }
    define_method("#{prop}=") { |val| instance_variable_set("@#{prop}", val) }
  end

  def self.details
    puts "Constantes: #{self.constants}"
    puts "Méthodes: #{self.instance_methods(false)}"
  end

  def public_method
    "Ceci est une méthode publique."
  end

  def interact_with_another_person(other_person)
    if other_person.respond_to?(:protected_method)
      "Interagir avec une méthode protégée: #{other_person.protected_method}"
    else
      "Impossible d'interagir avec une méthode protégée."
    end
  end

  private

  def private_method
    "Ceci est une méthode privée."
  end

  protected

  def protected_method
    "Ceci est une méthode protégée."
  end
end
```

</details>

## Etape 3 : Techniques de base en métaprogrammation

### Définition dynamique de méthodes

**Exercice :** Étendez la classe `Person` pour ajouter dynamiquement des méthodes basées sur un tableau de traits, comme `[:smile, :frown]`, qui retournent simplement une chaîne de caractères descriptive. Testez votre implémentation en créant une instance de `Person` et en appelant les méthodes dynamiques.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  [:smile, :frown].each do |action|
    define_method(action) do
      "Person #{action}s"
    end
  end
end
```

</details>

### Utilisation de la méthode `method_missing` et `respond_to_missing?`

**Exercice :** Modifiez la classe `Person` pour qu'elle utilise [method_missing](https://ruby-doc.org/core-2.6.3/BasicObject.html#method-i-method_missing) pour répondre aux méthodes non définies en affichant un message personnalisé. Assurez-vous également d'implémenter [respond_to_missing?](https://ruby-doc.org/3.2.2/Object.html#method-i-respond_to_missing-3F). Testez votre implémentation en appelant une méthode non définie sur une instance de `Person`.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def method_missing(method_name, *args, &block)
    "La méthode #{method_name} n'existe pas"
  end

  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?('dynamic_') || super
  end
end
```

</details>

### Accès et modification des variables d'instance

**Exercice :** Ajoutez une méthode à la classe `Person` pour lister et modifier les valeurs des variables d'instance de manière dynamique.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def set_property(property_name, value)
    instance_variable_set("@#{property_name}", value)
  end

  def get_property(property_name)
    instance_variable_get("@#{property_name}")
  end
end
```

</details>

#### Correction complète

Voici une correction complète de l'étape 3. Vous pouvez l'utiliser pour vérifier votre travail.

En intégrant ces concepts, la classe Person complète ressemblerait à ceci :

<details><summary markdown="span">Voir la correction complète</summary>

```ruby
class Person
  [:name, :age, :smile, :frown].each do |prop|
    define_method("#{prop}") { instance_variable_get("@#{prop}") }
    define_method("#{prop}=") { |val| instance_variable_set("@#{prop}", val) }
  end

  def self.details
    puts "Constantes: #{self.constants}"
    puts "Méthodes: #{self.instance_methods(false)}"
  end

  def public_method
    "Ceci est une méthode publique."
  end

  def method_missing(method_name, *args, &block)
    "La méthode #{method_name} n'existe pas"
  end

  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?('dynamic_') || super
  end

  def set_property(property_name, value)
    instance_variable_set("@#{property_name}", value)
  end

  def get_property(property_name)
    instance_variable_get("@#{property_name}")
  end

  private

  def private_method
    "Ceci est une méthode privée."
  end

  protected

  def protected_method
    "Ceci est une méthode protégée."
  end
end
```

</details>

## Etape 4 : Techniques avancées et applications

### Evaluation et exécution de code dynamique

**Exercice :** Ajoutez une méthode à la classe `Person` pour évaluer dynamiquement du code Ruby fourni en tant que chaîne de caractères. Utilisez avec prudence [eval](https://ruby-doc.org/3.2.2/Kernel.html#method-i-eval) pour cela.

<details><summary markdown="span">Correction</summary>

```ruby
class Person
  def execute_dynamic_code(code)
    eval(code)
  end
end
```

</details>

### Hooks et Métaprogrammation avancée

**Exercice :** Modifiez la classe `Person` pour qu'elle inclue un hook `included` qui ajoute une méthode spécifique à tout module qui l'inclut.

<details><summary markdown="span">Correction</summary>

```ruby
module Greetings
  def self.included(base)
    base.class_eval do
      def greet
        "Bonjour du module #{self.class.name}"
      end
    end
  end
end

class Person
  include Greetings
end
```

</details>

#### Correction complète

Voici une correction complète de l'étape 4. Vous pouvez l'utiliser pour vérifier votre travail.

En incorporant ces techniques avancées, la classe Person serait maintenant :

<details><summary markdown="span">Voir la correction complète</summary>

```ruby
module Greetings
  def self.included(base)
    base.class_eval do
      def greet
        "Bonjour du module #{self.class.name}"
      end
    end
  end
end

class Person
  include Greetings

  [:name, :age, :smile, :frown].each do |prop|
    define_method("#{prop}") { instance_variable_get("@#{prop}") }
    define_method("#{prop}=") { |val| instance_variable_set("@#{prop}", val) }
  end

  def execute_dynamic_code(code)
    eval(code)
  end

  def method_missing(method_name, *args, &block)
    "La méthode #{method_name} n'existe pas"
  end

  def respond_to_missing?(method_name, include_private = false)
    method_name.to_s.start_with?('dynamic_') || super
  end

  def set_property(property_name, value)
    instance_variable_set("@#{property_name}", value)
  end

  def get_property(property_name)
    instance_variable_get("@#{property_name}")
  end

  private

  def private_method
    "Ceci est une méthode privée."
  end

  protected

  def protected_method
    "Ceci est une méthode protégée."
  end
end
```

</details>

## Etape 6 : Pour aller plus loin

Vous pouvez vous amuser à créer des classes et des modules qui utilisent les techniques de métaprogrammation que vous avez apprises dans ce workshop. Voici quelques idées :

- Créez un `DSL (Domain-Specific Language)` simple pour une configuration d'utilisateur. Utilisez la métaprogrammation pour définir des méthodes de configuration basées sur un DSL.
- Créer un mini-framework `MVC (Modèle-Vue-Contrôleur)` qui utilise la métaprogrammation pour définir des routes et des actions de contrôleur basées sur un DSL.
