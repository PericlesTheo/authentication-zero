# Authentication Zero

The purpose of authentication zero is to generate a pre-built authentication system into a rails application (web or api-only) that follows both security and rails best practices. By generating code into the user's application instead of using a library, the user has complete freedom to modify the authentication system so it works best with their app.

## Features

- Sign up
- Email and password validations
- Reset the user password and send reset instructions
- Authentication by cookie (html)
- Authentication by token (api)
- Remember me (html)
- Send e-mail when email is changed
- Send e-mail when password is changed
- Cancel my account
- Log out

## Security and best practices

- [Current attributes](https://api.rubyonrails.org/classes/ActiveSupport/CurrentAttributes.html): Abstract super class that provides a thread-isolated attributes singleton, which resets automatically before and after each request.
- [has_secure_password](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password): Adds methods to set and authenticate against a BCrypt password.
- [has_secure_token](https://api.rubyonrails.org/classes/ActiveRecord/SecureToken/ClassMethods.html#method-i-has_secure_token): Adds methods to generate unique tokens.
- [signed_id](https://api.rubyonrails.org/classes/ActiveRecord/SignedId.html): Returns a signed id that is tamper proof, so it's safe to send in an email or otherwise share with the outside world.
- [Signed cookies](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html): Returns a jar that'll automatically generate a signed representation of cookie value and verify it when reading from the cookie again.
- [Http only cookies](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html): A cookie with the httponly attribute is inaccessible to the JavaScript, this precaution helps mitigate cross-site scripting (XSS) attacks.
- [Log filtering](https://guides.rubyonrails.org/action_controller_overview.html#log-filtering): Parameters 'token' and 'password' are marked [FILTERED] in the log.
- [Callbacks](https://api.rubyonrails.org/classes/ActiveRecord/Callbacks.html): We use callbacks to send emails before changing an email or password.
- [Action mailer](https://api.rubyonrails.org/classes/ActionMailer/Base.html): Action Mailer allows you to send email from your application using a mailer model and views.

## Installation

Add this lines to your application's Gemfile:

```ruby
gem "bcrypt"
gem "authentication-zero"
```

Then run `bundle install`

You'll need to set the root path in your routes.rb, for this example let's use the following:

```ruby
root "home#index"
```

```
$ rails generate controller home index
```

Add these lines to your `app/views/home/index.html.erb`:

```html+erb
<p style="color: green"><%= notice %></p>

<p>Signed as <%= Current.user.email %></p>

<div>
  <%= link_to "Change email", edit_emails_path %>
</div>

<div>
  <%= link_to "Change password", edit_passwords_path %>
</div>

<div>
  <%= link_to "Cancel my account & delete my data", new_cancellations_path %>
</div>

<%= button_to "Log out", sign_out_path, method: :delete %>
```

And you'll need to set up the default URL options for the mailer in each environment. Here is a possible configuration for `config/environments/development.rb`:

```ruby
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

## Usage

```
$ rails generate authentication user
```

## Development

To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/lazaronixon/authentication-zero. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/lazaronixon/authentication-zero/blob/main/CODE_OF_CONDUCT.md).


## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the AuthenticationZero project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/lazaronixon/authentication-zero/blob/main/CODE_OF_CONDUCT.md).
