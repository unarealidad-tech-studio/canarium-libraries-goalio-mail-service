# GoalioMailService

Sends Email using a Google Account defined in the configuration.

## Configuration

Create a file `goaliomailservice.local.php` in you appinstance's `config/autoload/` directory.

Paste the following in the file:

```php
$google_username = '';
$google_password = '';

/**
 * You do not need to edit below this line
 */
return array(
    'goaliomailservice' => array(
        'transport_class' => 'Zend\Mail\Transport\Smtp',

        'options_class' => 'Zend\Mail\Transport\SmtpOptions',

        'options' => array(
            'host' => 'smtp.gmail.com',
            'connection_class' => 'login',
            'connection_config' => array(
                'ssl' => 'tls',
                'username' => $google_username,
                'password' => $google_password
            ),
            'port' => 587
        )
    )
);
```

Add the value for `$google_username` and `$google_password` with the credentials from your google account. Take note that this is a local file and will not be commited to the repository.

That is all you need to send email in canarium.

## Sending Email

Once initialized, the service `goaliomailservice_message` will be available for you to retrive via Service Manager. Example:

```php
$mailService = $this->getServiceLocator()->get('goaliomailservice_message');
```
Now send an email

```php

  $message = $mailService->createHtmlMessage(
      $$from_email,
      $to_email,
      $subject,
      'index/email',
      $params
  );

  $mailService->send($message);

```

## Available Methods

### createHtmlMessage

Return a HTML message ready to be sent

```php
public function createHtmlMessage($from, $to, $subject, $nameOrModel, $values = array()):MailMessage;
```

Parameter | Description
--------- | ----------
$from | A string containing the sender e-mail address, or if array with keys email and name
$to | An array containing the recipients of the mail
$subject | Subject of the mail
$nameOrModel | Either the template to use, or a ViewModel
$values | Values to use when the template is rendered. The variables passed will be assessible to the template specified in $nameOrModel


### createTextMessage

Return a text message ready to be sent


```php
public function createTextMessage($from, $to, $subject, $nameOrModel, $values = array()):MailMessage;
```

Parameter | Description
--------- | ----------
$from | A string containing the sender e-mail address, or if array with keys email and name
$to | An array containing the recipients of the mail
$subject | Subject of the mail
$nameOrModel | Either the template to use, or a ViewModel
$values | Values to use when the template is rendered. The variables passed will be assessible to the template specified in $nameOrModel


### send

Sends the MailMessage

```php
public function send(MailMessage $message)
```
