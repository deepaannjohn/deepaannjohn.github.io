---
title: BDD with Cucumber and Gherkin
date: "2020-02-04T23:50:37.121Z"
---

What is BDD? Why do we do BDD?
Business / Product Owner (specification / requirements) -> BA (Jira story)-> Developers (code) -> QA (test results)
In this process, the requirements can totally get misinterpreted.

BDD gives concrete exmaples rather than abstract specifications.    
A common language which all of the above personas can understand is used in BDD.

_Discovery phase / Illustrate phase_
BDD specifies concrete examples of how a user would interact with the software / application.
So its clear to everyone involved. 
Main tools would be white boards and Post-its.
The output would be Acceptance criteria and Business rules.

_Formulate Phase_
BA / tester would formulate these acceptance criteria / busines rules to a format of specifcations which 
both a business person can understand and something that an automated process can also execute as part of the test suite.
This is where cucumber comes into play. We write these executable specifications in a special language called Gherkin (which 
uses the Given-When-Then notation)

_Automate Phase_
We use these Gherkin specifications into automated acceptance tests

All these happens before development startes
And to show case how the software works, we just have to exeucte the tests which is what the business can understand 

BDD - a single source of truth, both specification and automated acceptance tests. Also these become the functional documentations.

**Gherkin**
Gherkin is the 'Given-When-Then' notation.
A business readbale notation for executable specifications.
As we can see from the example below, the language is 'Business Readable' and 'Plain English'.
And finally, this is an execuable piece of code as well!

example:
Feature: Guess the word

  # The first example has two steps
  Scenario: Maker starts a game
    When the Maker starts a game
    Then the Maker waits for a Breaker to join

  # The second example has three steps
  Scenario: Breaker joins a game
    Given the Maker has started a game with the word "silky"
    When the Breaker joins the Maker's game
    Then the Breaker must guess a word with 5 characters

**Structure of Gherkin Language:**
We start with Feature
Feature is ilustrated with a nuber of 'Scenarios'
Scenaris use 'Given When Then'


**Cucumber**
Automation ibrary used to automate / run our executable specifications
Scenario: Some cukes
  Given I have 48 cukes in my belly

package com.example;
import io.cucumber.java.en.Given;

public class StepDefinitions {
    @Given("I have {int} cukes in my belly")
    public void i_have_n_cukes_in_my_belly(int cukes) {
        System.out.format("Cukes: %n\n", cukes);
    }
}


Set up a Cucumber Test case:
_To be added_





