# unicredit
Unicredit library for online payments

# Installation
```php
$ composer require monolog/monolog
```

Basic configuration

```php
<?php

use EchoWine\Unicredit\Unicredit;

# Initialize with configuration
Unicredit::ini([
    'terminal_id' => 'UNI_ECOM',
    'api_key' => 'UNI_TESTKEY',
    'currency' => 'EUR',
    'lang' => 'IT'
]);
```


Checkout page

```php
<?php

use EchoWine\Unicredit\Unicredit;

# Make a new instance
$uc = new Unicredit();

# Set redirect url
$uc -> urls([
    'verify' => 'http://localhost/verify.php',
    'error' => 'http://localhost/error.php'
]);

# Create a random ID for an order
$order_id = md5(time());

# Make a request
# Return the Payment ID
$transaction_id = $uc -> payment($order_id,'email@customer.com',10);

if($transaction_id){
    
    # IMPORTANT !!!    
    # Save $order_id and $transaction_id in DB or Cookie in order to retrieve in the next page

    # Redirect to the checkout
    $uc -> getUrl();

}else{
	
	# Get error
    $error = $uc -> getLastError();
}

```


Verify page

```php
<?php

use EchoWine\Unicredit\Unicredit;

# Retrieve $transaction_id and $order_id from DB/Cookie

# Make a new instance
$uc = new Unicredit();

$check = $uc -> verify($order_id,$transaction_id);

if($check){

    $response = $uc -> getResponse();

    if(empty($response -> error)){

        # Success!!!
        # Payments is now confirmed

    }else{

        # Some error
    }


}else{
    
    # Error: check failed
}


```
