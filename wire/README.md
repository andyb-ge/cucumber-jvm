# Cucumber-Java-Wire

## Background

Cucumber-Java can be used in conjunction with a runner (such as JUnit)
to write feature steps and run tests solely in Java.

Cucumber-Wire can be used with Cucumber-CPP to write tests in C++ and use
Cucumber to run your feature files integrated using the wire protocol.

## What this library provides

There are cases where you will want to orchestrate tests of a library
which is implemented in both C++ and Java. Cucumber-Java-Wire is a port
of the Cucumber-CPP implementation (with obvious language-specific
differences) and supports running Java feature steps using the wire
protocol.

With this library, you can do things such as the following:

    Feature: Library Interoperability

      As a library user
      I want to be able to exchange data between two different language endpoints

      Background:

        Given a publisher & subscriber with compatible endpoint configurations

      Scenario Outline:  Interoperability test between endpoints

        Given a <api1> subscriber
        And a <api2> publisher
        When the <api1> publisher publishes <data>
        Then the <api2> subscriber receives <data>

        Examples:

          | api1 | api2 | data  |
          | C++  | Java | hello |
          | C++  | C++  | world |
          | Java | Java | foo   |
          | Java | C++  | bar   |

Obviously, the C++ steps will need to be implemented using Cucumber-CPP
and the Java steps implemented using Cucumber-Java-Wire.

## Test

An integration test using Cucumber-Java-Wire can be found under the `wire/src/test` folder.

To run the integration test:

1. In the `wire` folder, execute `mvn integration-test`.

   You should see output similar to the following:
   
        @foo
        Feature: Basic Arithmetic

        Background: A Calculator              # features/examples/java/calculator/basic_arithmetic.feature:4
          Given a calculator I just turned on # RpnCalculatorStepdefs.a_calculator_I_just_turned_on():0

        @foo
        Scenario: Addition     # features/examples/java/calculator/basic_arithmetic.feature:8
            # Try to change one of the values below to provoke a failure
          When I add 4 and 5   # RpnCalculatorStepdefs.adding(int,int):0
          Then the result is 9 # RpnCalculatorStepdefs.the_result_is(double):0

        @foo
        Scenario: Another Addition # features/examples/java/calculator/basic_arithmetic.feature:14
            # Try to change one of the values below to provoke a failure
          When I add 4 and 7       # RpnCalculatorStepdefs.adding(int,int):0
          Then the result is 11    # RpnCalculatorStepdefs.the_result_is(double):0

        Scenario Outline: Many additions # features/examples/java/calculator/basic_arithmetic.feature:19
          Given the previous entries:    # features/examples/java/calculator/basic_arithmetic.feature:20
            | first | second | operation |
            | 1     | 1      | +         |
            | 2     | 1      | +         |
          When I press +                 # features/examples/java/calculator/basic_arithmetic.feature:24
          And I add <a> and <b>          # features/examples/java/calculator/basic_arithmetic.feature:25
          And I press +                  # features/examples/java/calculator/basic_arithmetic.feature:26
          Then the result is <c>         # features/examples/java/calculator/basic_arithmetic.feature:27

          Examples: Single digits
            | a | b | c  |
            | 1 | 2 | 8  |
            | 2 | 3 | 10 |

          Examples: Double digits
            | a  | b  | c  |
            | 10 | 20 | 35 |
            | 20 | 30 | 55 |

        6 scenarios (6 passed)
        30 steps (30 passed)
        0m0.201s

## License

Since this project began as a port of Cucumber-CPP, the original Cucumber-CPP
license is included as part of the project to give credit where credit is due.

All original work falls under the Cucumber-JVM license.
