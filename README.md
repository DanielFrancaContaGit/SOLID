# Welcome to the SOLID wiki!


## Single Responsiblity Principle (Princípio da responsabilidade única)

 ### Uma classe deve ter um, e somente um motivo, para mudar.

### Errado:

```js
class Order{
    public function calculateTotalSum(){/*...*/}
    public function getItems(){/*...*/}
    public function getItemCount(){/*...*/}
    public function addItem($item){/*...*/}
    public function deleteItem($item){/*...*/}

    public function printOrder(){/*...*/}
    public function showOrder(){/*...*/}

    public function load(){/*...*/}
    public function save(){/*...*/}
    public function update(){/*...*/}
    public function delete(){/*...*/}
}
```
### Certo
```js
class Order{
    public function calculateTotalSum(){/*...*/}
    public function getItems(){/*...*/}
    public function getItemCount(){/*...*/}
    public function addItem($item){/*...*/}
    public function deleteItem($item){/*...*/}
}

class OrderRepository{
    public function load($orderID){/*...*/}
    public function save($order){/*...*/}
    public function update($order){/*...*/}
    public function delete($order){/*...*/}
}

class OrderViewer{
    public function printOrder($order){/*...*/}
    public function showOrder($order){/*...*/}
}

```
***
## Open-Closed Principle (Princípio Aberto-Fechado)

 ### Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação

### Errado:
```js
class ContratoClt{
    public function salario(){
        //...
    }
}

class Estagio{
    public function bolsaAuxilio(){
        //...
    }
}

class FolhaDePagamento{
    protected $saldo;
    
    public function calcular($funcionario){
        if ( $funcionario instanceof ContratoClt ) {
            $this->saldo = $funcionario->salario();
        } else if ( $funcionario instanceof Estagio) {
            $this->saldo = $funcionario->bolsaAuxilio();
        }
    }
}
```
### Certo:

```js
interface Remuneravel{
    public function remuneracao();
}

class ContratoClt implements Remuneravel{
    public function remuneracao(){
        //...
    }
}

class Estagio implements Remuneravel{
    public function remuneracao(){
        //...
    }
}

class FolhaDePagamento{
    protected $saldo;
    
    public function calcular(Remuneravel $funcionario){
        $this->saldo = $funcionario->remuneracao();
    }
}
```
***
## Liskov Substitution Principle (Princípio da substituição de Liskov)

 ### Uma classe derivada deve ser substituível por sua classe base.

### Errado:

```php
# - Sobrescrevendo um método que não faz nada...
class Voluntario extends ContratoDeTrabalho{
    public function remuneracao(){
        // não faz nada
    }
}


# - Lançando uma exceção inesperada...
class MusicPlay{
    public function play($file){
        // toca a música   
    }
}

class Mp3MusicPlay extends MusicPlay{
    public function play($file){
        if (pathinfo($file, PATHINFO_EXTENSION) !== 'mp3') {
            throw new Exception;
        }
        
        // toca a música
    }
}


# - Retornando valores de tipos diferentes...
class Auth{
    public function checkCredentials($login, $password){
        // faz alguma coisa
        
        return true;
    }
}

class AuthApi extends Auth{
    public function checkCredentials($login, $password){
        // faz alguma coisa
        
        return ['auth' => true, 'status' => 200];
    }
}
```

### Certo:

```js
 class A {
    public function getNome(){
        echo 'Meu nome é A';
    }
}

class B extends A { 
    public function getNome(){
        echo 'Meu nome é B';
    }
}

$objeto1 = new A;
$objeto2 = new B;

function imprimeNome(A $objeto){
    return $objeto->getNome();
}

imprimeNome($objeto1); // Meu nome é A
imprimeNome($objeto2); // Meu nome é B
```
***
## Interface Segregation Principle (Princípio da Segregação da Interface)

 ### Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar.
 
 ### Errado:
 
 ```php
interface Aves{
    public function setLocalizacao($longitude, $latitude);
    public function setAltitude($altitude);
    public function renderizar();
}

class Papagaio implements Aves{
    public function setLocalizacao($longitude, $latitude) {
        //Faz alguma coisa
    }
    
    public function setAltitude($altitude) {
        //Faz alguma coisa   
    }
    
    public function renderizar() {
        //Faz alguma coisa
    }
}

class Pinguim implements Aves{
    public function setLocalizacao($longitude, $latitude) {
        //Faz alguma coisa
    }
    
    // A Interface Aves está forçando a Classe Pinguim a implementar esse método.
    // Isso viola o príncipio ISP
    public function setAltitude($altitude) {
        //Não faz nada...  Pinguins são aves que não voam!
    }
    
    public function renderizar() {
        //Faz alguma coisa
    }
}
```
 
### Certo:

```php
interface Aves {
    public function setLocalizacao($longitude, $latitude);
    public function renderizar();
}

interface AvesQueVoam extends Aves {
    public function setAltitude($altitude);
}

class Papagaio implements AvesQueVoam {
    public function setLocalizacao($longitude, $latitude) {
        //Faz alguma coisa
    }
    
    public function setAltitude($altitude) {
        //Faz alguma coisa   
    }
    
    public function renderizar() {
        //Faz alguma coisa
    }
}

class Pinguim implements Aves {
    public function setLocalizacao($longitude, $latitude) {
        //Faz alguma coisa
    }
    
    public function renderizar() {
        //Faz alguma coisa
    }
}
``` 
 
***
## Dependency Inversion Principle (Princípio da inversão da dependência)

 ### Dependa de abstrações e não de implementações.
 
 ### Errado:
 
 ```php
use MySQLConnection;

class PasswordReminder {
    private $dbConnection;
    
    public function __construct() {       
        $this->dbConnection = new MySQLConnection();           
    }
    
    // Faz alguma coisa
}
```

### Certo:

```php

use MySQLConnection;

class PasswordReminder {
    private $dbConnection;
    
    public function __construct(MySQLConnection $dbConnection) {       
        $this->dbConnection = $dbConnection;           
    }
    
    // Faz alguma coisa
}
```
 
