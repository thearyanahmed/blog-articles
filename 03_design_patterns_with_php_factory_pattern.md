So the client doesn't need to think about the complex logic working behind the scene.

It's simply like a factory, we use products built in a factory but we don't need to know how its done. That is not our concern. Lets say we want to implement payment in our app, it will handle Paypal and Stripe payment to start with.

It is needless to say that the classes, logic and data we send to stripe would be different from paypal. So, let's start with an Interface. Interfaces are magic.
Defining our interface

```php

<?php
  
  
namespace Thearyanahmed\\DesignPatterns\\Factory;

interfacePaymentProcessorInterface {
	public function generateSessionToken() :string;
}
```

Our interface has two functions, one to generate a token, another to make the payment using that token.
Implementing the logic

Lets create our Stripe and Paypal classes that implements our PaymentProessorInterface.

```php

<?php
  
  
namespace Thearyanahmed\\DesignPatterns\\Factory;

class StripeimplementsPaymentProcessorInterface
{

  publicfunctiongenerateSessionToken():string
  {
		return 'stripe_' . bin2hex(openssl_random_pseudo_bytes(3));
  }

  public function processPayment(string $token) :void
  {
   		print_r('processing payment with ' . $token);
  }
}

```

and for paypal

```php


<?php
  
namespace Thearyanahmed\\DesignPatterns\\Factory;

class PaypalimplementsPaymentProcessorInterface
{

  	public function generateSessionToken():string
    {
        $this->aComplexFunctionDuringSessionCreation();

		return 'paypal_' . random_int(1,20) . '_some_other_logic_' . random_int(21,40);
    }

	public function processPayment(string $token):void
    {
        print_r('processing payment with ' . $token);
    }

    private function aComplexFunctionDuringSessionCreation()
    {
        print_r("Doing some complex function when creating session token\\n");
    }
}
```

To explain in short, Stripe is using bin2hex and adding 3 bytes with a string as a token. Paypal is concating random_int(1,20) with some string. It simply shows that the logic are different.

In paypal's generateSessionToken function it also calls a 'aComplexFunctionDuringSessionCreation' that does something else where we don't need anything like that in Stripe.
The factory

We have the implement and heavy logic ready, now lets create a factory.

```php


<?php
  
namespace Thearyanahmed\\DesignPatterns\\Factory;


use Thearyanahmed\\DesignPatterns\\AbstractFactory\\AbstractPaymentProcessorFactory;

class InternationalPaymentProcessorFactoryextendsAbstractPaymentProcessorFactory
{
  
	public function getPaymentProcessor(string $paymentGateway) :PaymentProcessorInterface
    {
	  
		if($paymentGateway === 'paypal') {
		  returnnew Paypal();
        }

		if($paymentGateway === 'stripe') {
			returnnew Stripe();
        }

		throw new \\Exception('Unsupported payment gateway');
    }
}
```

Our factory is called InternationalPaymentProcessorFactory that extends AbstractPaymentProcessorFactory. You can ignore the AbstractPaymentProcessorFactory for now.

First, based on the string being paypal or stripe, it will return us the object of that class.

In it we have a simple one method, getPaymentProcessor() that takes a string and returns an object which implements the PaymentProcessorInterface.

Let's think about that for a moment.

Our Stripe implementation has 2 methods while our Paypal has 3. It can have as many it likes and each of them has their own set of logic.

But, when we are using this to handle a transaction, we don't necessarily need to know function to call. Stripe could have called that function makeStripePayment instead of processPayment.

But that's why we have interface, to set some rules. So, as a client, you know it returns an instance of PaymentProcessorInterface and we can only look at that interface.

And we know it has two methods, so which ever class is returned to us from the factory ( getPaymentProcessor() ) we know that object will have those two ( generateSessionToken and processPayment ) methods.

So, this is our client end code,

```

$gateway = (new InternationalPaymentProcessorFactory)->getPaymentProcessor('paypal');

$token = $gateway->generateSessionToken();

print_r("We got token " .  $token . " \\n");

print_r("Trying to make the payment...\\n");

$gateway->processPayment($token);
```

We are calling the InternationalPaymentProcessor factory to give us an object that will handle the processing.

The output will be something like
```


Doing some complex functionwhen creating session token
We got token paypal_12_some_other_logic_21
Trying to make the payment...
processing payment with paypal_12_some_other_logic_21
```

If we change the string from paypal to stripe,
```

We got token stripe_2d36d9
Trying to make the payment...
processing payment with stripe_2d36d9
```

As a client, we didn't have to worry about anything, just pass in the value which payment gateway we wanted. Paypal or Stripe.

But the code is still the same, we didn't have to change anything.

This way, we could keep adding as many payment gateways we like, each of them could and probably will have it's own logic and way of handling things.

But would the client need to worry about anything else?

No, just like things change in a factory but as consumers we don't need to know about them, with FactoryPattern, we don't need to worry about any of them.

The full code can be found at https://github.com/thearyanahmed/design-patterns-with-php