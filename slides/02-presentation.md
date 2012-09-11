!SLIDE subsection
# In our past...

* PO specified functionality as work packages on varying levels of technicality
* PO asked DEV for feedback or not
* DEV began implementing work packages
* DEV asked PO for feedback or not
* implementation was as intended or not

!SLIDE subsection
# In our present...

* features
* early collaboration on scenarios / examples
* stories
* acceptance criteria

!SLIDE subsection
# Definitions
## Feature

A coherent set of functionality from the user's point of view.

## Scenario

An example of intended usage in terms of *specific* user *activities*.

## Story

A vertical slice of functionality, that PO can give DEV feedback on and that
could technically be deployed to production.

!SLIDE subsection
# Our process (1/2)

* PO thinks and plans in terms of features (business domain), deferring
  lower level details
* DEV and PO sit down together
  * writing scenarios (business domain)
  * creating stories from scenarios
  * augmenting stories with further details as DEV considers necessary
    (acceptance criteria)

!SLIDE subsection
# Our process (2/2)

* DEV implements stories in specified order
  * implementing scenario steps with cucumber
  * adding rspec features for technical features (application domain)
* PO approves stories after completion
* DEV potentially deploys stories to production

!SLIDE subsection
# Cucumber Scenarios

* talk about the business domain
* specific wrt actions
* abstract wrt tasks
* living documentation for PO and new team members

!SLIDE code small

    Feature: Author publishes articles

      In order to give readers a coherent experience
      As UX expert
      I want authors to adhere to my style guide

      Scenario: Article conforms to style guide
        Given there is a properly styled article
        When the author publishes the article
        Then the article should appear on the site

      Scenario: Article doesn't conform to style guide
        Given there is an article without headline
        When the author publishes the article
        Then he should see a style violation error
         And the article should not appear on the site

!SLIDE subsection
# Cucumber Step Definitions

* like all methods, should be short and not too concrete
* similar language to step implemented

!SLIDE code small

    @@@ Ruby
    Given /^there is a properly styled article$/ do
      @article = make_styled_article
    end

    When /^the author publishes the article$/ do
      login_as(:author)
      publish(@article)
    end

    Then /^the article should appear on the site$/ do
      homepage.should have_article(@article)
    end

!SLIDE subsection
# RSpec Features

* `spec/features/`
* talk about the application domain
* capybara DSL for acceptance testing

!SLIDE code small

    @@@ Ruby
    feature 'Session expiration' do

      background do
        login(user)
      end

      def session_should_be_valid
        visit dashboard_path
        user_should_be_logged_in
      end

      scenario 'a session is valid for 24 hours' do
        Timecop.travel(24.hours.from_now - 1.second) do
          session_should_be_valid
        end
      end
    end

!SLIDE subsection
# Conclusion

* feature -> scenario -> story
* intensive collaboration DEV and PO
* cucumber features for business logic
* living documentation
* rspec features for application logic and regression tests
