# Making functionality compatible between two classes
This example illustrates the structure of the Adapter design pattern and focuses on the following questions:

* What classes does it consist of?
* What roles do these classes play?
* In what way the elements of the pattern are related?


## Target Class
The Target defines the domain-specific interface used by the client code.

```php

namespace RefactoringGuru\Adapter\Conceptual;

class Target
{
    public function request(): string
    {
        return "Target: The default target's behavior.";
    }
}

```

## Adaptee Class
 The Adaptee contains some useful behavior, but its interface is incompatible with the existing client code. The Adaptee needs some adaptation before the client code can use it.

```php
class Adaptee
{
    public function specificRequest(): string
    {
        return ".eetpadA eht fo roivaheb laicepS";
    }
}

```

## Adapter Class
The Adapter makes the Adaptee's interface compatible with the Target's interface.

```php
class Adapter extends Target
{
    private $adaptee;

    public function __construct(Adaptee $adaptee)
    {
        $this->adaptee = $adaptee;
    }

    public function request(): string
    {
        return "Adapter: (TRANSLATED) " . strrev($this->adaptee->specificRequest());
    }
}

```

## Client Code
The client code supports all classes that follow the Target interface.

```php
function clientCode(Target $target)
{
    echo $target->request();
}


echo "Client: I can work just fine with the Target objects:\n";
$target = new Target();
clientCode($target);
echo "\n\n";

$adaptee = new Adaptee();
echo "Client: The Adaptee class has a weird interface. See, I don't understand it:\n";
echo "Adaptee: " . $adaptee->specificRequest();
echo "\n\n";

echo "Client: But I can work with it via the Adapter:\n";
$adapter = new Adapter($adaptee);
clientCode($adapter);
```

OUTPUT
<pre>
Client: I can work just fine with the Target objects:
Target: The default target's behavior.

Client: The Adaptee class has a weird interface. See, I don't understand it:
Adaptee: .eetpadA eht fo roivaheb laicepS

Client: But I can work with it via the Adapter:
Adapter: (TRANSLATED) Special behavior of the Adaptee.
</pre>