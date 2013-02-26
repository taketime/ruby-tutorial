Getting Sauced with Ruby
============

With Ruby 1.9.3 and Rails 3.2.x, write this:

```ruby
Sauce.config do |c|
  c.browsers = [["Windows 7", "Firefox", "18"]]
end

describe "Sauce Labs Browser Documentation" do
  it "Displays Ruby code only when Ruby selected" do
    visit "https://saucelabs.com/docs/browsers"
    select('ruby', :from => 'lang-chooser')
    caps = page.find(:xpath, "//p[contains(., 'caps = Selenium::WebDriver::Remote::Capabilities')]")
    caps.visible?.should be_true
  end
end
```

And get an integration test in Firefox on Windows (Which you don't have installed) with screenshots, video and a log of passes and failures.

We're using Capybara and RSpec, but Sauce Labs also works great with [Test::Unit](http://test-unit.rubyforge.org/), [Cucumber](http://cukes.info/), and most other testing frameworks... Right down to vanilla [WebDriver](http://code.google.com/p/selenium/wiki/RubyBindings).

What You'll Need
----------------

In your Gemfile:

```ruby
group :test, :development do
  # These are the target gems of this tutorial
  gem 'rspec-rails', '~> 3.2.0'
  gem 'sauce', '~> 2.2.2'
  gem 'capybara', '~> 1.0'
end
```

Grab a free Sauce Labs account [here](https://saucelabs.com/signup/plan/free).

Setting up RSpec
-----------

From your `$RAILS_ROOT`, generate a ./spec directory, a ./spec/spec_helper.rb file, and a warm, fuzzy feeling of productivity by executing:

    rails generate rspec:install

Inside the newly created spec_helper.rb, just under the other `require`s we'll add Capybara and the Sauce gem:

```ruby
require 'capybara/rails'
require 'capybara/rspec'
require 'sauce/capybara'
```

We also want to tell Capybara to use Sauce Labs for all tests (by default, it's only used for tests marked :type => :js):

```ruby
Capybara.default_driver = :sauce
```

Setting up the Sauce Gem
-------------------------

Keep your Sauce Labs credentials out of your repositories and available to all your Sauce Labs tools using projects by adding them as environment variables.

<!-- SAUCE:BEGIN_PLATFORM:MAC|LINUX -->

Open `~/.bash_profile` and add the following lines:

```bash
export SAUCE_USERNAME=<!-- SAUCE:USERNAME -->
export SAUCE_ACCESS_KEY=<!-- SAUCE:ACCESS_KEY -->
```

You'll then need to re-load that profile with `source ~/.bash_profile`
<!-- SAUCE:END_PLATFORM -->
<!-- SAUCE:BEGIN_PLATFORM:WIN -->
Open your environment variables settings window (Instructions [here](http://www.itechtalk.com/thread3595.html)) and set the following variables:

    Name: SAUCE_USERNAME
    Value: <!-- SAUCE:USERNAME -->

    Name: SAUCE_ACCESS_KEY
    Value:  <!-- SAUCE:ACCESS_KEY -->
<!-- SAUCE:END_PLATFORM -->

Now, open up your `./spec/spec_helper` file, and add the following block (after the requires) to configure which browsers you want to use

```ruby
Sauce.config do |c|
  c.browsers = [["Windows 7", "Firefox", "18"]]
end
```

Check out [this list of browser/OS platforms](http://saucelabs.com/docs/browsers) and pick which ones you'd like to test against.

Writing your test
-----------------

Phew!  That's all your setup done.  You're ready to write your tests.

We're going to put our test in the spec/requests directory so that rspec includes the Capybara DSL:

    mkdir ./spec/requests
    vim ./spec/requests/browser_docs_spec.rb

```ruby
require "spec_helper"

describe "Sauce Labs Browser Documentation" do
  it "Displays Ruby code only when Ruby selected" do
    visit "https://saucelabs.com/docs/browsers"
    select('ruby', :from => 'lang-chooser')
    caps = page.find(:xpath, "//p[contains(., 'caps = Selenium::WebDriver::Remote::Capabilities')]")
    caps.visible?.should be_true
  end
end
```

And that's everything!  Running the test (`rake spec:requests`) should give the following output:

    $ rake spec:requests
    .

    Finished in 24.31 seconds
    1 example, 0 failures

    Randomized with seed 6006

The `1 example, 0 failures` line means the test is passing, congratulations!

Check out the results, including a command log, screenshots, and video of the browser executing the test, on your [account page](https://saucelabs.com/account).

<!-- SAUCE:INCLUDE:get-support -->
