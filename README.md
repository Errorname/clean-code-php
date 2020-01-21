# Clean Code PHP

Traduction en français de [Clean Code PHP](https://github.com/jupeter/clean-code-php) par [Jupeter](https://github.com/jupeter/). Si vous trouvez une erreur dans l'orthographe, l'écriture ou la traduction, n'hésitez pas à faire un pull request !

## Table des matières

  1. [Introduction](#introduction)
  2. [Variables](#variables)
 	 * [Utiliser des noms de variables expressifs et prononçables](#utiliser-des-noms-de-variables-expressifs-et-prononçables)
	 * [Utiliser le même vocabulaire pour le même type de variable](#utiliser-le-même-vocabulaire-pour-le-même-type-de-variable)
	 * [Utiliser des noms qui peuvent être recherchés (partie 1)](#utiliser-des-noms-qui-peuvent-être-recherchés-partie-1)
	 * [Utiliser des noms qui peuvent être recherchés (partie 2)](#utiliser-des-noms-qui-peuvent-être-recherchés-partie-2)
	 * [Utiliser des variables expressives](#utiliser-des-variables-expressives)
     * [Éviter d'imbriquer trop profondément et renvoyer tôt (partie 1)](#Éviter-dimbriquer-trop-profondément-et-renvoyer-tôt-partie-1)
     * [Éviter d'imbriquer trop profondément et renvoyer tôt (partie 2)](#Éviter-dimbriquer-trop-profondément-et-renvoyer-tôt-partie-2)
     * [Éviter les cartes mentales](#Éviter-les-cartes-mentales)
     * [Ne pas ajouter de contexte inutile](#ne-pas-ajouter-de-contexte-inutile)
     * [Utiliser des arguments par défaut à la place des court-circuits ou des conditions](#utiliser-des-arguments-par-défaut-à-la-place-des-court-circuits-ou-des-conditions)
  3. [Fonctions](#fonctions)
	 * [Arguments de fonction (idéalement 2 ou moins)](#arguments-de-fonction-idéalement-2-ou-moins)
	 * [Les fonctions doivent faire une seule chose](#les-fonctions-doivent-faire-une-seule-chose)
     * [Les noms de fonction doivent dire ce qu'elles font](#les-noms-de-fonction-doivent-dire-ce-quelles-font)
     * [Les fonctions ne doivent avoir qu'un seul niveau d'abstraction](#les-fonctions-ne-doivent-avoir-quun-seul-niveau-dabstraction)
     * [Ne pas utiliser de flags comme paramètres de fonction](#ne-pas-utiliser-de-flags-comme-paramètres-de-fonction)
	 * [Éviter les effets secondaires](#Éviter-les-effets-secondaires)
     * [Ne pas écrire de fonctions globales](#ne-pas-écrire-de-fonctions-globales)
     * [Ne pas utiliser le pattern Singleton](#ne-pas-utiliser-le-pattern-singleton)
     * [Encapsuler les conditions](#encapsuler-les-conditions)
	 * [Éviter les conditions négatives](#Éviter-les-conditions-négatives)
     * [Éviter les conditions](#Éviter-les-conditions)
     * [Éviter la vérification de type (partie 1)](#Éviter-la-vérification-de-type-partie-1)
     * [Éviter la vérification de type (partie 2)](#Éviter-la-vérification-de-type-partie-2)
	 * [Retirer le code mort](#retirer-le-code-mort)
  4. [Objets et Structures de Données](#objets-et-structures-de-données)
	 * [Utiliser l'encapsulation en objet](#utiliser-lencapsulation-en-objet)
	 * [Faire des objets avec des membres privés/protégés](#faire-des-objets-avec-des-membres-privésprotégés)
  5. [Classes](#classes)
	 * [Préférer la composition à l'héritage](#préférer-la-composition-à-lhéritage)
	 * [Éviter le chaînage des méthodes](#Éviter-le-chaînage-des-méthodes)
  6. [SOLID](#solid)
     * [Principe de Responsabilité Unique](#principe-de-responsabilité-unique)
     * [Principe Ouvert/Fermé](#principe-ouvertfermé)
     * [Principe de Substitution de Liskov](#principe-de-substitution-de-liskov)
     * [Principe de Ségrégation des Interfaces](#principe-de-ségrégation-des-interfaces)
     * [Principe d'Inversion des Dépendances](#principe-dinversion-des-dépendances)
  7. [Ne vous répétez pas (DRY)](#ne-vous-répétez-pas-dry)
  8. [Traductions](#traductions)

## Introduction

Principes d'ingénierie logicielle, tirés du livre de Robert C. Martin [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), adapté au PHP. Ce n'est pas un guide de style. C'est un guide pour produire des logiciels lisibles, réutilisables, et refactorables en PHP.

Tous les principes ne doivent pas être strictement respectés, et seront encore moins universellement acceptés. Il s'agit de lignes directrices et rien de plus, mais ce sont des lignes directrices codifiées sur de nombreuses années d'expérience collective par les auteurs de *Clean Code*.

Inspiré par [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)

Bien que de nombreux développeurs utilisent encore PHP 5, la plupart des exemples de cet article ne fonctionnent qu'avec PHP 7.1+.

## Variables

### Utiliser des noms de variables expressifs et prononçables

**Mauvais:**

```php
$ymdstr = $moment->format('y-m-d');
```

**Bien:**

```php
$currentDate = $moment->format('y-m-d');
```

**[⬆ retour en haut](#table-des-matières)**

### Utiliser le même vocabulaire pour le même type de variable

**Mauvais:**

```php
getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
```

**Bien:**

```php
getUser();
```

**[⬆ retour en haut](#table-des-matières)**

### Utiliser des noms qui peuvent être recherchés (partie 1)

Nous lirons plus de code que nous n'en écrirons jamais. Il est important que le code que nous écrivons soit lisible et puisse être recherché. En ne donnant *pas* de noms de variables significatif pour notre programme, nous nuisons à nos lecteurs. Rendez vos noms recherchable.

**Mauvais:**

```php
// À quoi peut bien correspondre ce 448 ?
$result = $serializer->serialize($data, 448);
```

**Bien:**

```php
$json = $serializer->serialize($data, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
```

### Utiliser des noms qui peuvent être recherchés (partie 2)

**Mauvais:**

```php
// À quoi peut bien correspondre ce 4 ?
if ($user->access & 4) {
    // ...
}
```

**Bien:**

```php
class User
{
    const ACCESS_READ = 1;
    const ACCESS_CREATE = 2;
    const ACCESS_UPDATE = 4;
    const ACCESS_DELETE = 8;
}

if ($user->access & User::ACCESS_UPDATE) {
    // do edit ...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Utiliser des variables expressives

**Mauvais:**

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches[1], $matches[2]);
```

**Pas mal:**

C'est mieux, mais nous restons très dépendant du regex.

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

[, $city, $zipCode] = $matches;
saveCityZipCode($city, $zipCode);
```

**Bien:**

Diminution de la dépendance du regex en nommant les sous-patterns.

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(?<city>.+?)\s*(?<zipCode>\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches['city'], $matches['zipCode']);
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter d'imbriquer trop profondément et renvoyer tôt (partie 1)

Trop d'instructions si/sinon peuvent rendre votre code difficile à suivre. L'explicite est toujours préférable à l'implicite.

**Mauvais:**

```php
function isShopOpen($day): bool
{
    if ($day) {
        if (is_string($day)) {
            $day = strtolower($day);
            if ($day === 'friday') {
                return true;
            } elseif ($day === 'saturday') {
                return true;
            } elseif ($day === 'sunday') {
                return true;
            } else {
                return false;
            }
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```

**Bien:**

```php
function isShopOpen(string $day): bool
{
    if (empty($day)) {
        return false;
    }

    $openingDays = [
        'friday', 'saturday', 'sunday'
    ];

    return in_array(strtolower($day), $openingDays, true);
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter d'imbriquer trop profondément et renvoyer tôt (partie 2)

**Mauvais:**

```php
function fibonacci(int $n)
{
    if ($n < 50) {
        if ($n !== 0) {
            if ($n !== 1) {
                return fibonacci($n - 1) + fibonacci($n - 2);
            } else {
                return 1;
            }
        } else {
            return 0;
        }
    } else {
        return 'Not supported';
    }
}
```

**Bien:**

```php
function fibonacci(int $n): int
{
    if ($n === 0 || $n === 1) {
        return $n;
    }

    if ($n > 50) {
        throw new \Exception('Not supported');
    }

    return fibonacci($n - 1) + fibonacci($n - 2);
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter les cartes mentales

Ne forcez pas les lecteurs de votre code à traduire ce que la variable veut dire. L'explicite est toujours préférable à l'implicite.

**Mauvais:**

```php
$l = ['Austin', 'New York', 'San Francisco'];

for ($i = 0; $i < count($l); $i++) {
    $li = $l[$i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Wait, what is `$li` for again?
    dispatch($li);
}
```

**Bien:**

```php
$locations = ['Austin', 'New York', 'San Francisco'];

foreach ($locations as $location) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch($location);
}
```

**[⬆ retour en haut](#table-des-matières)**

### Ne pas ajouter de contexte inutile

Si le nom de votre classe/objet vous renseigne sur sa nature, ne le répetez pas dans le nom de vos variables.

**Mauvais:**

```php
class Car
{
    public $carMake;
    public $carModel;
    public $carColor;

    //...
}
```

**Bien:**

```php
class Car
{
    public $make;
    public $model;
    public $color;

    //...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Utiliser des arguments par défaut à la place des court-circuits ou des conditions

**Pas bien:**

Ce n'est pas bien car `$breweryName` peut être `NULL`.

```php
function createMicrobrewery($breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**Pas mal:**

Cette façon est plus compréhensible que la version précédente, et elle contrôle mieux la valeur de la variable.

```php
function createMicrobrewery($name = null): void
{
    $breweryName = $name ?: 'Hipster Brew Co.';
    // ...
}
```

**Bien:**

Si vous ne supportez que PHP7+, vous pouvez utiliser le [type hinting](http://php.net/manual/fr/functions.arguments.php#functions.arguments.type-declaration) (typage d'objet) et être sûr que `$breweryName` ne sera pas `NULL`.

```php
function createMicrobrewery(string $breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**[⬆ retour en haut](#table-des-matières)**

## Fonctions

### Arguments de fonction (idéalement 2 ou moins)

Limiter le nombre de paramètres de fonctions est extrêmement important car cela facilite le test de votre fonction. Le fait d'en avoir plus de trois conduit à une explosion combinatoire où vous devez tester des tonnes de cas différents avec chaque argument distinct.

Zéro argument est le cas idéal. Un ou deux arguments est acceptable et trois devrait être évités. Au delà, il faudrait revoir la fonction. Habituellement, si vous avez plus de deux arguments, votre fonction essaie d'en faire trop. Si ce n'est pas le cas, un objet de plus haut niveau suffira en tant qu'argument dans la plupart du temps.

**Mauvais:**

```php
function createMenu(string $title, string $body, string $buttonText, bool $cancellable): void
{
    // ...
}
```

**Bien:**

```php
class MenuConfig
{
    public $title;
    public $body;
    public $buttonText;
    public $cancellable = false;
}

$config = new MenuConfig();
$config->title = 'Foo';
$config->body = 'Bar';
$config->buttonText = 'Baz';
$config->cancellable = true;

function createMenu(MenuConfig $config): void
{
    // ...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Les fonctions doivent faire une seule chose

C'est de loin la règle la plus importante en génie logiciel. Lorsque les fonctions font plus d'une chose, elles sont plus difficiles à composer, tester et raisonner. Lorsque vous pouvez isoler une fonction en une seule action, elles peuvent être facilement remaniées et votre code sera beaucoup plus propre. Si vous ne retirez rien d'autre de ce guide que ceci, vous serez en avance sur de nombreux développeurs.

**Mauvais:**
```php
function emailClients(array $clients): void
{
    foreach ($clients as $client) {
        $clientRecord = $db->find($client);
        if ($clientRecord->isActive()) {
            email($client);
        }
    }
}
```

**Bien:**

```php
function emailClients(array $clients): void
{
    $activeClients = activeClients($clients);
    array_walk($activeClients, 'email');
}

function activeClients(array $clients): array
{
    return array_filter($clients, 'isClientActive');
}

function isClientActive(int $client): bool
{
    $clientRecord = $db->find($client);

    return $clientRecord->isActive();
}
```

**[⬆ retour en haut](#table-des-matières)**

### Les noms de fonction doivent dire ce qu'elles font

**Mauvais:**

```php
class Email
{
    //...

    public function handle(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// C'est quoi ? Un handle pour le message ? On écrit dans un fichier maintenant ?
$message->handle();
```

**Bien:**

```php
class Email
{
    //...

    public function send(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// Clair et évident
$message->send();
```

**[⬆ retour en haut](#table-des-matières)**

### Les fonctions ne doivent avoir qu'un seul niveau d'abstraction

Si vous avez plus d'un niveau d'abstraction, votre fonction fait probablement trop de chose. Séparer les fonctions amène à plus de réusabilité et du test plus facile.

**Mauvais:**

```php
function parseBetterJSAlternative(string $code): void
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            // ...
        }
    }

    $ast = [];
    foreach ($tokens as $token) {
        // lex...
    }

    foreach ($ast as $node) {
        // parse...
    }
}
```

**Mauvais aussi:**

Nous avons divisé une partie des fonctionnalités, mais la fonction `parseBetterJSAlternative()` est encore trop complexe et pas testable.

```php
function tokenize(string $code): array
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            $tokens[] = /* ... */;
        }
    }

    return $tokens;
}

function lexer(array $tokens): array
{
    $ast = [];
    foreach ($tokens as $token) {
        $ast[] = /* ... */;
    }

    return $ast;
}

function parseBetterJSAlternative(string $code): void
{
    $tokens = tokenize($code);
    $ast = lexer($tokens);
    foreach ($ast as $node) {
        // parse...
    }
}
```

**Bien:**

La meilleure solution est d'enlever les dépendances de la fonction `parseBetterJSAlternative()`.

```php
class Tokenizer
{
    public function tokenize(string $code): array
    {
        $regexes = [
            // ...
        ];

        $statements = explode(' ', $code);
        $tokens = [];
        foreach ($regexes as $regex) {
            foreach ($statements as $statement) {
                $tokens[] = /* ... */;
            }
        }

        return $tokens;
    }
}

class Lexer
{
    public function lexify(array $tokens): array
    {
        $ast = [];
        foreach ($tokens as $token) {
            $ast[] = /* ... */;
        }

        return $ast;
    }
}

class BetterJSAlternative
{
    private $tokenizer;
    private $lexer;

    public function __construct(Tokenizer $tokenizer, Lexer $lexer)
    {
        $this->tokenizer = $tokenizer;
        $this->lexer = $lexer;
    }

    public function parse(string $code): void
    {
        $tokens = $this->tokenizer->tokenize($code);
        $ast = $this->lexer->lexify($tokens);
        foreach ($ast as $node) {
            // parse...
        }
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

### Ne pas utiliser de flags comme paramètres de fonction

Les flags (ou marqueurs) indiquent aux utilisateurs que la fonction fait plus d'une seule chose. Les fonctions ne doivent faire qu'une seule chose. Divisez vos fonctions si vous suivez différent chemins d'éxécutions en fonction d'un booléen.

**Mauvais:**

```php
function createFile(string $name, bool $temp = false): void
{
    if ($temp) {
        touch('./temp/'.$name);
    } else {
        touch($name);
    }
}
```

**Bien:**

```php
function createFile(string $name): void
{
    touch($name);
}

function createTempFile(string $name): void
{
    touch('./temp/'.$name);
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter les effets secondaires

Une fonction produit un effet secondaire si elle ne fait rien d'autre que prendre une valeur et renvoyer une ou plusieurs valeurs. Un effet secondaire peut être l'écriture dans un fichier, la modification d'une variable globale ou le transfert accidentel de tout votre argent à un étranger.

Il arrive que vous ayez occasionnellement besoin d'avoir des effets secondaires dans un programme. Comme dans l'exemple précédent, vous aurez peut-être besoin d'écrire dans un fichier. Ce que vous devriez faire, c'est de centraliser là où vous le ferez. N'ayez pas plusieurs fonctions et classes qui écrivent dans un même fichier. Ayez un, et seulement un, service qui le fait.

Le point principal est d'éviter les pièges classique comme partager un état entre des objets sans aucune structure, utiliser des types de données mutable qui peuvent être écrits par n'importe quoi, ou bien ne pas centraliser là où vos effets secondaires se produisent. Si vous le faites, vous serez plus heureux que la grande majorité des autres programmeurs.

**Mauvais:**

```php
// Variable globale référencé par la fonction suivante.
// Si nous avions une autre fonction qui l'utilise, ce serait maintenant un tableau et ça pourrait le casser.
$name = 'Ryan McDermott';

function splitIntoFirstAndLastName(): void
{
    global $name;

    $name = explode(' ', $name);
}

splitIntoFirstAndLastName();

var_dump($name); // ['Ryan', 'McDermott'];
```

**Bien:**

```php
function splitIntoFirstAndLastName(string $name): array
{
    return explode(' ', $name);
}

$name = 'Ryan McDermott';
$newName = splitIntoFirstAndLastName($name);

var_dump($name); // 'Ryan McDermott';
var_dump($newName); // ['Ryan', 'McDermott'];
```

**[⬆ retour en haut](#table-des-matières)**

### Ne pas écrire de fonctions globales

Polluer l'espace globale est une mauvaise pratique dans de nombreux langages car cela pourrait entrer en conflit avec une autre bibliothèque et l'utilisateur de votre API pourrait ne rien en savoir jusqu'à ce qu'il obtienne une exception en production. Prenons un exemple: vous voulez un tableau de configuration. Vous pourriez écrire une fonction globale comme `config()`, mais cela pourrait se heurter à une autre bibliothèque qui essayait de faire la même chose.

**Mauvais:**

```php
function config(): array
{
    return  [
        'foo' => 'bar',
    ]
}
```

**Bien:**

```php
class Configuration
{
    private $configuration = [];

    public function __construct(array $configuration)
    {
        $this->configuration = $configuration;
    }

    public function get(string $key): ?string
    {
        return isset($this->configuration[$key]) ? $this->configuration[$key] : null;
    }
}
```

Charge la configuration et crée l'instance de la classe `Configuration`

```php
$configuration = new Configuration([
    'foo' => 'bar',
]);
```

Et maintenant vous devrez utiliser l'instance de `Configuration` dans votre application.

**[⬆ retour en haut](#table-des-matières)**

### Ne pas utiliser le pattern Singleton

Singleton est un [anti-pattern](https://fr.wikipedia.org/wiki/Singleton_(patron_de_conception)). Paraphrasé de Brian Button:
 1. Ils sont principalement utilisé comme des **instances globales**, pourquoi ce n'est pas bien ? Car **vous cachez les dépendances** de votre application dans votre code, au lieu de les proposer au travers d'interfaces. Rendre quelque chose global pour éviter de le faire circuler est un "[code smell](https://fr.wikipedia.org/wiki/Code_smell)" (ou "mauvaise odeur").
 2. Ils enfreignent le [principe de responsabilité unique](#principe-de-responsabilité-unique): car ils sont **responsables de leur propre création et cycle de vie**.
 3. Ils provoquent intrinsèquement le [couplage](https://fr.wikipedia.org/wiki/Couplage_(informatique)) étroit du code. Cela rend leur simulation pour le **test plutôt difficile** dans de nombreux cas.
 4. Ils transportent un état durant le cycle de vide de l'application. Encore une difficulté pour le test car vous pourrez arriver dans une situation où **les tests auront besoin d'un ordre précis**, ce qui est totalement déconseillé pour les tests unitaires. Pourquoi ? Car chaque test unitaire devrait être indépendant des autres.

Vous pourrez aussi trouver de bonnes reflexions par [Misko Hevery](http://misko.hevery.com/about/) sur [la racine du problème](http://misko.hevery.com/2008/08/25/root-cause-of-singletons/).

**Mauvais:**

```php
class DBConnection
{
    private static $instance;

    private function __construct(string $dsn)
    {
        // ...
    }

    public static function getInstance(): DBConnection
    {
        if (self::$instance === null) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    // ...
}

$singleton = DBConnection::getInstance();
```

**Bien:**

```php
class DBConnection
{
    public function __construct(string $dsn)
    {
        // ...
    }

     // ...
}
```

Créez une instance de la classe `DBConnection` et configurez le avec [DSN](http://php.net/manual/fr/pdo.construct.php#refsect1-pdo.construct-parameters).

```php
$connection = new DBConnection($dsn);
```

Et maintenant, vous devrez utiliser l'instance de `DBConnection` dans votre application.

**[⬆ retour en haut](#table-des-matières)**

### Encapsuler les conditions

**Mauvais:**

```php
if ($article->state === 'published') {
    // ...
}
```

**Bien:**

```php
if ($article->isPublished()) {
    // ...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter les conditions négatives

**Mauvais:**

```php
function isDOMNodeNotPresent(\DOMNode $node): bool
{
    // ...
}

if (!isDOMNodeNotPresent($node))
{
    // ...
}
```

**Bien:**

```php
function isDOMNodePresent(\DOMNode $node): bool
{
    // ...
}

if (isDOMNodePresent($node)) {
    // ...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter les conditions

Cela semble à une tâche impossible. En entendant cela pour la première fois, la plupart des gens disent: "Comment suis-je sensé faire quoi que ce soit sans instructions `if` ?" La réponse est que vous pouvez utiliser le polymorphisme pour arriver au même résultat dans la plupart des cas. La seconde question est souvent: "D'accord, c'est super, mais pourquoi je voudrais faire ça ?" La réponse vient d'un précédent concept que nous avons vu: une fonction ne devrait faire qu'une seule chose. Lorsque vous avez des classes et des fonctions avec des instructions `if`, vous informez à vos utilisateurs que votre fonction fait plus d'une chose. Rappellez-vous: ne faites qu'une seule chose.

**Mauvais:**

```php
class Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        switch ($this->type) {
            case '777':
                return $this->getMaxAltitude() - $this->getPassengerCount();
            case 'Air Force One':
                return $this->getMaxAltitude();
            case 'Cessna':
                return $this->getMaxAltitude() - $this->getFuelExpenditure();
        }
    }
}
```

**Bien:**

```php
interface Airplane
{
    // ...

    public function getCruisingAltitude(): int;
}

class Boeing777 implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getPassengerCount();
    }
}

class AirForceOne implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude();
    }
}

class Cessna implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getFuelExpenditure();
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter la vérification de type (partie 1)

PHP est non-typé, ce qui signifie que vos fonctions peuvent prendre n'importe quel type d'argument. Parfois, vous êtes enivrés par cette liberté et il devient tentant de faire des vérifications de type dans vos fonctions. Il y a de nombreuses façon d'éviter de le faire. La première façon est d'avoir des APIs consistantes.

**Mauvais:**

```php
function travelToTexas($vehicle): void
{
    if ($vehicle instanceof Bicycle) {
        $vehicle->pedalTo(new Location('texas'));
    } elseif ($vehicle instanceof Car) {
        $vehicle->driveTo(new Location('texas'));
    }
}
```

**Bien:**

```php
function travelToTexas(Traveler $vehicle): void
{
    $vehicle->travelTo(new Location('texas'));
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter la vérification de type (partie 2)

Si vous travaillez avec des valeurs primitives de base comme les chaînes de caractères, les entiers, ou les tableaux, que vous utilisez PHP 7+ et que vous ne pouvez pas utiliser le polymorphisme mais que vous ressentez toujours le besoin de faire une vérification de type, vous devriez considérer la [déclaration de type](http://php.net/manual/fr/functions.arguments.php#functions.arguments.type-declaration) ou le mode strict. Cela vous offre un typage statique en plus de la syntaxe PHP standard. Le problème de la vérification de type manuelle est que cela nécessitera tellement de code supplémentaire que la fausse sécurité que vous obtenez ne compense pas la perte en lisbilité. Gardez votre PHP propre, écrivez de bons tests, et ayez de bonnes revues de code. Sinon, faites tout cela, mais avec une déclation strict des types ou en mode strict.

**Mauvais:**

```php
function combine($val1, $val2): int
{
    if (!is_numeric($val1) || !is_numeric($val2)) {
        throw new \Exception('Must be of type Number');
    }

    return $val1 + $val2;
}
```

**Bien:**

```php
function combine(int $val1, int $val2): int
{
    return $val1 + $val2;
}
```

**[⬆ retour en haut](#table-des-matières)**

### Retirer le code mort

Le code mort est aussi mauvais que le code dupliqué. Il n'y a aucune raison de le garder dans votre code. Si ce n'est pas appelé, débarrassez-vous en ! Il sera toujours disponible dans votre historique de version si vous en avez besoin.

**Mauvais:**

```php
function oldRequestModule(string $url): void
{
    // ...
}

function newRequestModule(string $url): void
{
    // ...
}

$request = newRequestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**Bien:**

```php
function requestModule(string $url): void
{
    // ...
}

$request = requestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**[⬆ retour en haut](#table-des-matières)**

## Objets et Structures de Données

### Utiliser l'encapsulation en objet

En PHP, vous pouvez affecter les mots clés `public`, `protected` et `private` sur les méthodes. En l'utilisant, vous pouvez controler les modifications des propriétés d'un objet.

* Si vous souhaites faire une certaine action en plus d'accéder à la propriété d'un objet, vous n'aurez pas besoin de modifier tous vos accesseurs dans votre code.
* Permet d'ajouter une validation simple quand vous effectuer un `set`.
* Encapsule la représentation interne.
* Facile pour ajouter l'enregistrement des logs et la gestion des erreurs quand vous récupérez ou modifiez des attributs.
* En héritant cette classe, vous pourrez remplacer les fonctionnalités par défaut.
* Vous pouvez charger paresseusement les propriétés de votre objet, depuis un serveur par exemple.

En outre, cela fait parti du [Principe Ouvert/Fermé](#principe-ouvertfermé).

**Mauvais:**

```php
class BankAccount
{
    public $balance = 1000;
}

$bankAccount = new BankAccount();

// Achète des chaussures...
$bankAccount->balance -= 100;
```

**Bien:**

```php
class BankAccount
{
    private $balance;

    public function __construct(int $balance = 1000)
    {
      $this->balance = $balance;
    }

    public function withdraw(int $amount): void
    {
        if ($amount > $this->balance) {
            throw new \Exception('Amount greater than available balance.');
        }

        $this->balance -= $amount;
    }

    public function deposit(int $amount): void
    {
        $this->balance += $amount;
    }

    public function getBalance(): int
    {
        return $this->balance;
    }
}

$bankAccount = new BankAccount();

// Achète des chaussures...
$bankAccount->withdraw($shoesPrice);

// Récupère le solde
$balance = $bankAccount->getBalance();
```

**[⬆ retour en haut](#table-des-matières)**

### Faire des objets avec des membres privés/protégés

* Les méthodes et propriétés `public` sont les plus dangereuses au changement, car du code extérieur
peut facilement compter sur elles et vous ne pouvez pas controler quel code compte dessus.
**Les modifications dans les classes sont dangereuses pour tous les utilisateurs de la classe.**
* Le modificateur `protected` est aussi dangereux que `public`, car ils sont disponible à la portée
de n'importe quelle classe fille. Ainsi, la seule différence entre public et protected est dans
le méchanisme d'accès, mais l'encapsulation reste la même.
**Les modifications dans les classes sont dangereuses pour toutes les descendantes.**
* Le modificateur `private` guaranti que le le code est
**dangereux à modifier seulement dans la limite d'une classe unique**
(vous êtes en sécurité pour les modifications et vous n'avez pas
l'[effet Jenga](http://www.urbandictionary.com/define.php?term=Jengaphobia&defid=2494196)).

Ainsi, utilisez `private`par défaut et `public/protected` si vous avez besoin de donner l'accès à des classes externes.

Pour plus d'informations, vous pouvez lire l'[article de blog](http://fabien.potencier.org/pragmatism-over-theory-protected-vs-private.html) sur ce sujet écrit par [Fabien Potencier](https://github.com/fabpot).

**Mauvais:**

```php
class Employee
{
    public $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->name; // Employee name: John Doe
```

**Bien:**

```php
class Employee
{
    private $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->getName(); // Employee name: John Doe
```

**[⬆ retour en haut](#table-des-matières)**

## Classes

### Préférer la composition à l'héritage

Comme il est indiqué dans le célèbre livre [*Design Patterns*](https://fr.wikipedia.org/wiki/Design_Patterns) par le "Gang of Four", vous devriez privilégier les compositions à l'héritage, quand vous le pouvez. Il y a de nombreuses bonnes raisons pour utiliser l'héritage et de nombreuses bonnes raisons pour utiliser la composition. Le point important de cette maxime est que si votre instinct va vers l'héritage, essayez de voir si la composition ne pourrait pas mieux modéliser votre problème. Dans certains cas, ça peut.

Vous pourriez vous demander alors: "Quand dois-je utiliser l'héritage ?" Cela dépend du problème courant, mais voici une liste raisonnable de quand l'héritage a plus de sens que la composition:

1. Votre héritage représente une relation "est-un" et non une relation "a-un" (Humain->Animal vs. Utilisateur->DetailsUtilisateur).
2. Vous pouvez réutiliser du code de la classe de base (Les humains peuvent se déplacer comme les animaux).
3. Vous voulez effectuer des modifications globale aux classes dérivées en changeant la classe de base. (Changer la dépense calorique de tous les animaux quand ils se déplacent).

**Mauvais:**

```php
class Employee
{
    private $name;
    private $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    // ...
}

// Pas bien car Employee "a" des données de taxes
// EmployeeTaxData n'est pas un type d'Employee

class EmployeeTaxData extends Employee
{
    private $ssn;
    private $salary;

    public function __construct(string $name, string $email, string $ssn, string $salary)
    {
        parent::__construct($name, $email);

        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}
```

**Bien:**

```php
class EmployeeTaxData
{
    private $ssn;
    private $salary;

    public function __construct(string $ssn, string $salary)
    {
        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}

class Employee
{
    private $name;
    private $email;
    private $taxData;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function setTaxData(string $ssn, string $salary)
    {
        $this->taxData = new EmployeeTaxData($ssn, $salary);
    }

    // ...
}
```

**[⬆ retour en haut](#table-des-matières)**

### Éviter le chaînage des méthodes

La [**désignation chaînée**](https://fr.wikipedia.org/wiki/D%C3%A9signation_cha%C3%AEn%C3%A9e) (ou fluent interface) est une API orienté objet qui vise à améliorer la lisibilité du code source en utilisant le [chaînage de méthodes](https://en.wikipedia.org/wiki/Method_chaining).

Même si il peut y avoir certains contextes, notamment les constructeurs d'objets, où ce pattern réduit la verbosité du code (par exemple le [PHPUnit Mock Builder](https://phpunit.de/manual/current/en/test-doubles.html) ou le [Doctrine Query Builder](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)), il vient la plupart du temps avec de nombreux problèmes:

1. Casse l'[Encapsulation](https://fr.wikipedia.org/wiki/Encapsulation_(programmation))
2. Casse les [Décorateurs](https://fr.wikipedia.org/wiki/D%C3%A9corateur_(patron_de_conception))
3. Complique le [mocking](https://fr.wikipedia.org/wiki/Mock_(programmation_orient%C3%A9e_objet)) dans la suite de tests
4. Complique la lecture des différences dans les commits

Pour plus d'informations, vous pouvez lire l'[article de blog](https://ocramius.github.io/blog/fluent-interfaces-are-evil/) sur le sujet écrit par [Marco Pivetta](https://github.com/Ocramius).

**Mauvais:**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): self
    {
        $this->make = $make;

	// NOTE: Renvoi this pour le chaînage
        return $this;
    }

    public function setModel(string $model): self
    {
        $this->model = $model;

        // NOTE: Renvoi this pour le chaînage
        return $this;
    }

    public function setColor(string $color): self
    {
        $this->color = $color;

        // NOTE: Renvoi this pour le chaînage
        return $this;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = (new Car())
  ->setColor('pink')
  ->setMake('Ford')
  ->setModel('F-150')
  ->dump();
```

**Bien:**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): void
    {
        $this->make = $make;
    }

    public function setModel(string $model): void
    {
        $this->model = $model;
    }

    public function setColor(string $color): void
    {
        $this->color = $color;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = new Car();
$car->setColor('pink');
$car->setMake('Ford');
$car->setModel('F-150');
$car->dump();
```

**[⬆ retour en haut](#table-des-matières)**

## SOLID

**SOLID** est un acronyme mnémonique introduit par Michael Feathers pour les cinq premiers principes nommé par Robert Martin, indiquant les cinq principes de base du design et de la programmation orienté objet.

 * [S: Single Responsibility Principle (Responsabilité Unique)](#principe-de-responsabilité-unique)
 * [O: Open/Closed Principle (Ouvert/Fermé)](#principe-ouvertfermé)
 * [L: Liskov Substitution Principle (Substitution de Liskov)](#principe-de-substitution-de-liskov)
 * [I: Interface Segregation Principle (Ségrégation des Interfaces)](#principe-de-ségrégation-des-interfaces)
 * [D: Dependency Inversion Principle (Inversion des Dépendances)](#principe-dinversion-des-dépendances)

### Principe de Responsabilité Unique

Comme indiqué dans Clean Code, "Il ne devrait pas y avoir plus d'une seule raison pour une classe d'être modifiée". Il est tentant de fourrer plein de fonctionnalités dans une classe, comme quand vous ne pouvez utiliser qu'une seule valise pour votre vol en avion. Le problème avec ça est que votre classe ne sera pas conceptuellement cohérente et cela lui donnera de nombres raisons de changer. Minimiser le nombre de fois où vous devez modifier une classe est important. C'est important car si trop de fonctionnalités sont présentes dans une classe et vous en modifiez une partie, cela peut être difficile de comprendre comment cela va impacter les autres modules dépendants dans votre code.

**Mauvais:**

```php
class UserSettings
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function changeSettings(array $settings): void
    {
        if ($this->verifyCredentials()) {
            // ...
        }
    }

    private function verifyCredentials(): bool
    {
        // ...
    }
}
```

**Bien:**

```php
class UserAuth
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function verifyCredentials(): bool
    {
        // ...
    }
}

class UserSettings
{
    private $user;
    private $auth;

    public function __construct(User $user)
    {
        $this->user = $user;
        $this->auth = new UserAuth($user);
    }

    public function changeSettings(array $settings): void
    {
        if ($this->auth->verifyCredentials()) {
            // ...
        }
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

### Principe Ouvert/Fermé

Tel qu'énoncé par Bertrand Meye, "les entitées logicielles (classes, modules, fonctions, etc.) devraient être ouverts aux extension, mais fermé aux modifications." Qu'est ce que cela veut dire cependant ? Ce principe énonce essentiellement que vous devriez autoriser les utilisateurs à ajouter de nouvelles fonctionnalités sans les autoriser à modifier le code existant.

**Mauvais:**

```php
abstract class Adapter
{
    protected $name;

    public function getName(): string
    {
        return $this->name;
    }
}

class AjaxAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'ajaxAdapter';
    }
}

class NodeAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'nodeAdapter';
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        $adapterName = $this->adapter->getName();

        if ($adapterName === 'ajaxAdapter') {
            return $this->makeAjaxCall($url);
        } elseif ($adapterName === 'httpNodeAdapter') {
            return $this->makeHttpCall($url);
        }
    }

    private function makeAjaxCall(string $url): Promise
    {
        // request and return promise
    }

    private function makeHttpCall(string $url): Promise
    {
        // request and return promise
    }
}
```

**Bien:**

```php
interface Adapter
{
    public function request(string $url): Promise;
}

class AjaxAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class NodeAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        return $this->adapter->request($url);
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

### Principe de Substitution de Liskov

Il s'agit d'un terme effrayant pour un concept vraiment simple. Il définit formellement que "Si S est un sous-type de T, alors les objets de type T peuvent être remplacés par des objets de type S (càd, les objets de type S peuvent remplacer les objets de type T) sans altérer aucunes des propriétés désirées du programme (exactitude, tâche effectuée, etc.)." C'est un définition encore plus effrayante.

La meilleure explication est que si vous avez une classe mère et une classe fille, alors elles peuvent être interchangées sans produire de résultats incorrect. Cela est peut-être encore un peu confus, alors regardant l'exemple classique Carré-Rectangle. Mathématiquement, un carré est un rectangle, mais si vous le modelisez avec une relation "est-un" par l'héritage, vous pouvez rapidement avoir des problèmes.

**Mauvais:**

```php
class Rectangle
{
    protected $width = 0;
    protected $height = 0;

    public function render(int $area): void
    {
        // ...
    }

    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Rectangle
{
    public function setWidth(int $width): void
    {
        $this->width = $this->height = $width;
    }

    public function setHeight(int $height): void
    {
        $this->width = $this->height = $height;
    }
}

/**
 * @param Rectangle[] $rectangles
 */
function renderLargeRectangles(array $rectangles): void
{
    foreach ($rectangles as $rectangle) {
        $rectangle->setWidth(4);
        $rectangle->setHeight(5);
        $area = $rectangle->getArea(); // MAUVAIS: Va renvoyer 25 pour Square. Devrait être 20.
        $rectangle->render($area);
    }
}

$rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles($rectangles);
```

**Bien:**

```php
abstract class Shape
{
    protected $width = 0;
    protected $height = 0;

    abstract public function getArea(): int;

    public function render(int $area): void
    {
        // ...
    }
}

class Rectangle extends Shape
{
    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Shape
{
    private $length = 0;

    public function setLength(int $length): void
    {
        $this->length = $length;
    }

    public function getArea(): int
    {
        return pow($this->length, 2);
    }
}

/**
 * @param Rectangle[] $rectangles
 */
function renderLargeRectangles(array $rectangles): void
{
    foreach ($rectangles as $rectangle) {
        if ($rectangle instanceof Square) {
            $rectangle->setLength(5);
        } elseif ($rectangle instanceof Rectangle) {
            $rectangle->setWidth(4);
            $rectangle->setHeight(5);
        }

        $area = $rectangle->getArea();
        $rectangle->render($area);
    }
}

$shapes = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles($shapes);
```

**[⬆ retour en haut](#table-des-matières)**

### Principe de Ségrégation des Interfaces

Ce principe stipule que "Les clients ne devraient pas être forcés de dépendre d'interfaces qu'ils n'utilisent pas."

Un bon exemple à ne pas faire est celui des classes qui requièrent de larges objets de paramétrage. Il est avantageux de ne pas demander aux clients de configurer un grand nombre d'options, car la plupart du temps, ils n'auront pas besoin de tous les paramètres. Les rendre optionnels aide à éviter d'avoir une "interface grasse".

**Mauvais:**

```php
interface Employee
{
    public function work(): void;

    public function eat(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // .... travaille
    }

    public function eat(): void
    {
        // .... mange durant la pause déjeuner
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... travaille encore plus
    }

    public function eat(): void
    {
        //.... un robot ne peut pas manger, mais il doit implémenter cette méthode
    }
}
```

**Bien:**

Tous les travailleurs ne sont pas des employés, mais chaque employé est un travailleur.

```php
interface Workable
{
    public function work(): void;
}

interface Feedable
{
    public function eat(): void;
}

interface Employee extends Feedable, Workable
{
}

class Human implements Employee
{
    public function work(): void
    {
        // .... travaille
    }

    public function eat(): void
    {
        //.... mange durant la pause déjeuner
    }
}

// robot can only work
class Robot implements Workable
{
    public function work(): void
    {
        // .... travaille
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

### Principe d'Inversion des Dépendances

Ce principe énonce deux choses essentielles:
1. Les modules de haut niveau ne devraient pas dépendre des modules de bas niveau. Les deux devraient dépendre des abstractions.
2. Les abstractions ne devraient pas dépendre des détails. Les détails devraient dépendre des abstractions.


Cela peut être difficile à comprendre au début, mais si vous avez travaillé avec des frameworks PHP (comme Symfony), vous avez vu une implémentation de ce principe sous la forme de Dependency Injection (DI). Bien qu'il ne s'agisse pas de concepts identiques, le principe d'Inversion des Dépendances empêche les modules de haut niveau de connaître les détails de ses modules de bas niveau et de les configurer. Il peut y parvenir grâce à l'injection des dépendances. L'énorme avantage est de réduire le couplage entre les modules. Le couplage est un très mauvais schéma de développement car il rend votre code difficile à refactoriser.

**Mauvais:**

```php
class Employee
{
    public function work(): void
    {
        // .... travaille
    }
}

class Robot extends Employee
{
    public function work(): void
    {
        //.... travaille encore plus
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**Bien:**

(Notez l'utilisation de l'interface Employee)

```php
interface Employee
{
    public function work(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // .... travaille
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... travaille encore plus
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

## Ne vous répétez pas (DRY)

Essayez de suivre le principe [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (Don't Repeat Yourself).

Faites de votre mieux pour éviter le double du code. Dupliquer le code est mauvais parce que cela signifie qu'il y a plus d'un endroit pour modifier quelque chose si vous avez besoin de changer une certaine logique.

Imaginez si vous dirigez un restaurant et que vous tenez à jour votre inventaire: toutes vos tomates, oignons, ail, épices, etc. Si vous avez plusieurs listes sur lesquelles vous gardez ceci, toutes les listes doivent être mises à jour lorsque vous servez un plat avec des tomates. Si vous n'avez qu'une seule liste, il n'y a qu'un seul endroit à mettre à jour!

Souvent, vous avez du code en double parce que vous avez deux ou plusieurs choses légèrement différentes, qui ont beaucoup en commun, mais leurs différences vous obligent à avoir deux ou plusieurs fonctions distinctes qui font la plupart des mêmes choses. Supprimer du code en double signifie créer une abstraction qui peut gérer cet ensemble de choses différentes avec une seule fonction/module/classe.

Il est essentiel de bien comprendre l'abstraction, c'est pourquoi vous devez suivre les principes SOLID énoncés dans la section [Classes](#classes). De mauvaises abstractions peuvent être pires que du code en double, alors attention! Ceci dit, si vous pouvez faire une bonne abstraction, faites-le! Ne vous répétez pas, sinon vous vous retrouverez à mettre à jour plusieurs endroits à chaque fois que vous voudrez changer une chose.

**Mauvais:**

```php
function showDeveloperList(array $developers): void
{
    foreach ($developers as $developer) {
        $expectedSalary = $developer->calculateExpectedSalary();
        $experience = $developer->getExperience();
        $githubLink = $developer->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}

function showManagerList(array $managers): void
{
    foreach ($managers as $manager) {
        $expectedSalary = $manager->calculateExpectedSalary();
        $experience = $manager->getExperience();
        $githubLink = $manager->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**Bien:**

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        $expectedSalary = $employee->calculateExpectedSalary();
        $experience = $employee->getExperience();
        $githubLink = $employee->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**Très bien:**


Il est préférable d'utiliser une version compacte du code.

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        render([
            $employee->calculateExpectedSalary(),
            $employee->getExperience(),
            $employee->getGithubLink()
        ]);
    }
}
```

**[⬆ retour en haut](#table-des-matières)**

## Traductions

Ce guide est aussi disponible dans d'autres langues:

* :us: **Anglais:**
   * [jupeter/clean-code-php](https://github.com/jupeter/clean-code-php)
* :cn: **Chinois:**
   * [php-cpm/clean-code-php](https://github.com/php-cpm/clean-code-php)
* :ru: **Russe:**
   * [peter-gribanov/clean-code-php](https://github.com/peter-gribanov/clean-code-php)
* :es: **Espagnol:**
   * [fikoborquez/clean-code-php](https://github.com/fikoborquez/clean-code-php)
* :brazil: **Portugais:**
   * [fabioars/clean-code-php](https://github.com/fabioars/clean-code-php)
   * [jeanjar/clean-code-php](https://github.com/jeanjar/clean-code-php/tree/pt-br)
* :thailand: **Thaïlandais:**
   * [panuwizzle/clean-code-php](https://github.com/panuwizzle/clean-code-php)

**[⬆ retour en haut](#table-des-matières)**
