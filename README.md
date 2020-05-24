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

## Errado:

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

## Certo:

```js

```
***
### Interface Segregation Principle (Princípio da Segregação da Interface)

 - Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar.
***
### Dependency Inversion Principle (Princípio da inversão da dependência)

 - Dependa de abstrações e não de implementações.
 
