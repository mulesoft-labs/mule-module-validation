Mule Validation Module
======================

Introduction
------------

A common issue when receiving data either electronically or from user input is verifying the integrity of the data.
This work is repetitive and becomes even more complicated when different sets of validation rules need to be applied
to the same set of data based on locale. Error messages may also vary by locale. This module addresses some of
these issues to speed development and maintenance of validation rules.

Rules
=====

The following section describes available validation rules.

Pre-Built Rules
---------------

### ISBN10

Validates that its valid ISBN-10 code.

    <validation:validate-isbn10 isbnCode="85-359-0277-5"/>

### ISBN13

Validates that its valid ISBN-13 code.

    <validation:validate-isbn13 isbnCode="978-0-123-45678-6"/>

### Email

Validates that its a valid email address.

    <validation:validate-email emailAddress="john.doe@mulesoft.com"/>

### Credit Card Number

Validates that its a valid credit card number.

    <validation:validate-credit-card-number creditCardNumber="5555444433332222">
        <validation:credit-card-types>
            <validation:credit-card-type>MASTERCARD</validation:credit-card-type>
        </validation:credit-card-types>
    </validation:validate-credit-card-number>

The following is a list of credit card types supported:

* VISA
* DINERS
* MASTERCARD
* DISCOVER
* AMEX

### IP Addresses

Validates its a valid IP address.

    <validation:validate-ip-address ipAddress="127.0.0.1"/>

!!! Both IPv4 and IPv6 are supported.

### Domains

Validates its a valid domain name.

    <validation:validate-domain domain="mulesoft.com"/>

Custom Rules
------------

### Numbers

Validates numbers format and value.

    <validation:validate-float value="3.14" locale="US"/>

The locale is used to determine which symbol denotes de decimal mark and the decimal point.

You can also potentially use minValue and maxValue to specify a range, and if the input its not
within that range it will throw an exception.

     <validation:validate-float value="3.14" locale="US" minValue="1" maxValue="5"/>

The following kind of numbers are supported:

* Integer
* Short
* Long
* Double
* Float

### Regular Expressions

Strings can be validated using regular expressions. You can use one, or multiple ones. If multiple
ones are used, only one needs to match and not all.

    <validation:validate-using-regex value="94105">
        <validation:regexs>
            <!-- Matches Canadian PostalCode formats with or without spaces (e.g., "T2X 1V4" or "T2X1V4") -->
            <validation:regex>^[ABCEGHJKLMNPRSTVXY]{1}\d{1}[A-Z]{1} *\d{1}[A-Z]{1}\d{1}$</validation:regex>
            <!-- Matches all US format zip code formats (e.g., "94105-0011" or "94105")  -->
            <validation:regex>^\d{5}(-\d{4})?</validation:regex>
        </validation:regexs>
    </validation:validate-using-regex>

### Not Empty

Validates that the input is not empty. Empty has a different meaning depending on the input object. As an
example, a String will be denoted empty if its length equals zero, on the other hand a List will be
denoted empty if the size is zero.

    <validation:validate-not-empty object-ref="#[payload]"/>

Custom Exceptions
=================

The default exception being thrown by all validations is InvalidException. All the validations have the
option of specifying a custom exception class using the <code>customExceptionClassName</code>.