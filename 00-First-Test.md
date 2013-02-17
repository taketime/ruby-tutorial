Getting Sauced with Ruby
============

So you’ve decided to functional test your web app.  That’s fantastic
news!  We’re here to help.  We’re going to assume you’re using Ruby
(v1.9.3-x) on Rails (v 3.2.x), you want to run all your integration tests
on multiple browsers, and you don’t have any integration tests yet.

We're using Capybara and RSpec, but Sauce Labs also works great with
Test::Unit, Cucumber, and most other testing frameworks... Right down to
vanilla WebDriver.

What's the end result?
--------------------------------

When you're set up, you can write something like this:

    Sauce.config do |c|
      c.browsers = [["Windows 2008","Firefox","18"]]
    end

    describe "The Homepage" do  
      it "Should log the user in" do  
        visit "/index.html"  
      end
    end
 
And our trusty robots will run in Chrome on Windows 7 (Neither of which you don't 
even have installed because that's how you roll).  Then we'll give you screenshots
of each step, video of the entire test, and a log of which tests failed.

What You'll Need
----------------

The respective gems in your Gemfile:

    group :test, :development do  
      # These are the target gems of this tutorial  
      gem 'rspec-rails', '~> 3.2.0'  
      gem 'sauce', '~> 2.2.2'  
      gem 'capybara', '~> 1.0'  
    end

A Sauce Labs account (which you can get for free [here]("https://saucelabs
.com/signup/plan/free"))

Setting up RSpec
-----------

From your `$RAILS\_ROOT`, executing `rails generate rspec:install`  
will generate a ./spec directory, a ./spec/spec_helper.rb file, and a warm,
fuzzy feeling of productivity.

Inside the newly created spec_helper.rb, just under the other `require's` we're
going to make sure we use Capybara and the Sauce gem:

`require ‘capybara/rails’  
require ‘capybara/rspec’  
require 'sauce/capybara'  `

We also want to tell Capybara to use Sauce for all tests (by default,
it's only used for tests marked :type => :js):

`Capybara.default_driver = :sauce`

Setting up the Sauce Gem
-------------------------

Keep your Sauce Labs credentials out of your repositories and available to
all your Sauce using projects by adding them as environment variables.

<!-- SAUCE:BEGIN_PLATFORM:MAC|LINUX -->  

Open `~/.bash_profile` and add the following lines:  
  
`export SAUCE_USERNAME=<!-- SAUCE:USERNAME -->  
export SAUCE_ACCESS_KEY=<!-- SAUCE:ACCESS_KEY -->`  

You'll then need to re-load that profile with `source ~/.bash_profile`  
<!-- SAUCE:END_PLATFORM -->  
<!-- SAUCE:BEGIN_PLATFORM:WIN -->  
Open your environment variables settings window (Instructions [here]
("http://www.itechtalk.com/thread3595.html")) and set the following variables:  

    Name: SAUCE_USERNAME  
    Value: <!-- SAUCE:USERNAME -->  
  
    Name: SAUCE_ACCESS_KEY  
    Value:  <!-- SAUCE:ACCESS_KEY -->`  
<!-- SAUCE:END_PLATFORM -->  

Now, open up your `./spec/spec_helper` file, and add the following block (after
 the requires) to configure which browsers you want to use

    Sauce.config do |c|  
      c.browsers = [["Windows 2008","Firefox", "18"]]  
    end

Check out [this]("http://www.saucelabs.com/browsers") list of browser/os
combinations and pick which you'd like to test against.

Writing your test
-----------------

Phew!  That's all your setup done.  You're ready to write your tests.

If you put your tests in a spec/requests directory, Rspec-rails will assume
they're Capybara tests and include the Capybara DSL.  So that's where we'll put
our test:

`mkdir ./spec/requests  
vim ./spec/requests/browser_docs_spec.rb  
  
require "spec_helper"

describe "Sauce Labs Browser Documentation" do  
    it "Displays Ruby code only when Ruby selected" do  
        visit "https://saucelabs.com/docs/browsers"  
        select('ruby', :from => 'lang-chooser')  
        caps = page.find(:xpath, "//p[contains(., 'caps = Selenium::WebDriver::Remote::Capabilities')]")  
        caps.visible?.should be_true  
    end  
end`