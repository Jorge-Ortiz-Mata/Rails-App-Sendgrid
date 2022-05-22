# Blog App.

## Sendgrid.

This application uses Sendgrid as a 3° party service to confirm users' emails.
In order to set it up, follow the intructions below:

1. Sign up for a free account in Sendgrid.
2. Create a Sender identity in Settings/Sender Authentication.
3. Once the sender is verified, keep the email that you use in order to use it as a validation.
4. Create an API Key in Settings/API Keys. Generate a new one with the full access and save it.
5. Add your API Key to your Rails credentials.
6. In config/development.rb add the following code (Don't forget to delete the duplicate mailers lines in this file.):

```
    config.action_mailer.raise_delivery_errors = true
    config.action_mailer.perform_deliveries = true
    config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
    config.action_mailer.delivery_method = :smtp
    config.action_mailer.smtp_settings = {
        :user_name => 'apikey',
        :password => Rails.application.credentials.sendgrid_secret_key,
        :domain => 'localhost:3000',
        :address => 'smtp.sendgrid.net',
        :port => 587,
        :authentication => :plain,
        :enable_starttls_auto => true
    }
```

7. In config/production.rb add the following code (Don't forget to delete the duplicate mailers lines in this file.):

```
  config.action_mailer.perform_caching = false
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.perform_deliveries = true
  config.action_mailer.default_url_options = { host: 'blog-yorch.herokuapp.com', protocol: "https" }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    :user_name => 'apikey',
    :password => Rails.application.credentials.sendgrid_secret_key, 
    :domain => 'blog-yorch.herokuapp.com',
    :address => 'smtp.sendgrid.net',
    :port => 587,
    :authentication => :plain,
    :enable_starttls_auto => true
}
```

8. The last part, in config/initializers/devise.rb you have to add the email that you used in order to verify your Sender identity:

```
  config.mailer_sender = 'sender.identity@company.com'
```


