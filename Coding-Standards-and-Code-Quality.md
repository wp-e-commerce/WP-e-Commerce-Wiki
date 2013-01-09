This document provides guidelines for coding standards and code quality. These guidelines should be adhered to as much as possible when you contribute code to WPEC. But if something doesn't make sense, feel free to question or challenge it.

# First and foremost
We're striving to adhere to WordPress' coding standards as much as possible, so here are 3 must-read guidelines from WordPress Codex:

* [WordPress Coding Standards](http://codex.wordpress.org/WordPress_Coding_Standards)
* [Data Validation](http://codex.wordpress.org/Data_Validation)
* [Internationalization](http://codex.wordpress.org/I18n_for_WordPress_Developers)


# Some general guidelines

## Functions

### Size

Each function should only do one thing. If it's taking care of too many operations, it should be broken down into multiple smaller functions.

Example:

	class Person {
		public function can_drink_alcohol() {
			global $wpdb;

			$year_of_birth = $wpdb->query( $wpdb->prepare( "SELECT year FROM people WHERE id = %d", $this->id ) );

			$age = date('Y') - $year_of_birth;

			return $age >= 21;
		}
	}

should be broken down to:

	class Person {
		public function get_year_of_birth() {
			global $wpdb;

			return $wpdb->query( $wpdb->prepare( "SELECT year FROM people WHERE id = %d", $this->id ) );
		}

		public function get_age() {
			return date('Y') - $this->get_year_of_birth();
		}

		public function can_drink_alcohol() {
			return $this->get_age() >= 21;
		}
	}

## Arguments
Avoid having too many arguments. It usually means you need to break down the function further, or let the function accept an associative array of arguments.

Arguments that accept default values should be at the end of the argument list.

## Return values
It's a good habit to make sure each function returns something. Even functions such as "setters", or functions that interact with the database etc. should at least return a boolean value indicating whether the operation is successful or not.

## Exceptions
Limit the use of exceptions. It makes the code overly complicated, and it creates fatal error which is undesirable in a production environment. Instead, extend or use WP_Error.

## Separation of logic & template
Try your best to separate logic and template. If a function needs to output a huge HTML template, put the HTML code in a separate file and include it. If a function only needs to output a line or two in HTML, it might be ok to just leave that line in the function. However, don't "echo" it directly. Instead, create one function that returns the output string, then create another that echo the returned value.

## OOP
Object Oriented Programming is a good idea. But use it wisely. Not everything is supposed to be a class. Of course there's no absolute rules that define whether something should be coded as a class. Some guidelines:

* If a class doesn't maintain states (properties that are unique to each instances, and are mutable depending on the situation), it should be broken down into a bunch of functions.

* If the name of the class ends with "er" or "or", beware. It might indicate that the class you're about to create is just a wrapper of a  bunch of functions. E.g. Purchase_Log_Processor.

* Don't make a class do too many things. If you're coding a class "Person", it's fine to let it "eat", "play", "work", "study". It's not fine to make it ask itself whether it's legal to drink (it's something defined by law, not by the person), or tally up the total amount of monthly expenses (which should be handled by a Money_Book class).

Example:

This class should be broken down:
	class Person {
		public function get_year_of_birth() {
			return db_query( "SELECT year FROM people WHERE id=%d", $this->id );
		}

		public function get_age() {
			return date('Y') - $this->get_year_of_birth();
		}

		public function can_drink_alcohol() {
			return $this->get_age() >= 21;
		}

		public function get_last_month_expenses() {
			$purchases = $this->get_purchases();
			$total = 0;
			foreach ( $purchases as $purchase ) {
				$total += $purchase;
			}

			return $total;
		}
	}

It should be broken down into:
	class Person {
		public function get_year_of_birth() {
			return db_query( "SELECT year FROM people WHERE id=%d", $this->id );
		}

		public function get_age() {
			return date('Y') - $this->get_year_of_birth();
		}

		public function get_last_month_expenses() {
			$purchases = $this->get_purchases();
			$total = 0;
			foreach ( $purchases as $purchase ) {
				$total += $purchase;
			}

			return $total;
		}
	}

	function is_legal_for_drinking( $person ) {
		return $person->get_age() >= 21;
	}

	class Money_Book {
		public function __construct( $person ) {
			$this->person = $person;
			$this->fetch_data();
		}

		public function get_last_month_expenses() {
			// ...
		}
	}

### Avoid direct attribute access and magic methods
Try to design your class so that attributes are not directly accessed like this: `$object->attribute`. Yes, WordPress core does that a lot, but no, we don't want to do that in WPEC. The reason being, attributes are prone to changes. If we let third party developers utilize the class API by accessing these attributes directly, making changes and maintaining backward compatibility will be very difficult.

To avoid doing this, there are 2 ways:

* For data attributes (those that are pulled from the database), use setters and getters. Data model objects are usually used extensively by third party developers, so getter & setter functions for these particular attributes offer a certain degree of flexibility while still provide ample room for providing backward compatibility.

	class MyDataObject {
		...

		function get( $attribute ) {
			return $this->data[$attribute];
		}

		function set( $attribute, $value ) {
			$this->data[$attribute] = $value;
		}

		function save() {
			global $wpdb;
			$wpdb->query( 'UPDATE etc.' );
		}

		// or better, separate getters / setters for each attributes if these
		// attributes need guarding
		function get_price() {
			return (float) $this->data['price'];
		}

		// this is an instance where an attribute needs guarding
		function set_price( $price ) {
			// convert a formatted string like 12,345.67 to float first
			if ( is_string( $price ) )
				$price = convert_string_to_float( $price );
			else ( ! is_numeric( $price ) )
				return; // or return a WP_Error

			$this->data['price'] = $price;
		}
	}

* For attributes that do not represent data columns, avoid creating setters (getters might be fine in some cases), and instead focus on what you want to do with that attribute. For example, `$bank_account->withdraw()` and `$bank_account->deposit()` but avoid `$bank_account->set_balance()`.

# Debugging

There are two ways to debug a PHP application.

## IDE and Breakpoints
Debugging using breakpoints can be handy in some cases where you only need to track down a bug that occurs within only a few functions.

In order to use this, you need a full-fledged IDE like NetBeans or Eclipse. Docs on how to configure this below:
http://netbeans.org/kb/docs/php/debugging.html
http://yoopergeek.blogspot.com/2007/10/getting-breakpoints-to-work-in-eclipse.html

## Printing out debug traces
Another way that would surprisingly work well in complicated PHP app is simply printing out debug traces, or log them into a file. print_r(), var_dump() and exit() would be your best friends.

If you want to have nicely formatted debug trace, [install XDebug](http://xdebug.org).

IBM has a [comprehensive guide on PHP debugging](http://www.ibm.com/developerworks/library/os-debug/).

# Naming convention
All functions need to be prefixed with "wpsc" (in case of functions) or "WPSC" (in case of classes) to avoid collision with WP core and other plugins.

A distinction between "public" and "private" routines should also be made clear in the routine name. A function is "public" when it is meant to be available for 3rd party developers to use. It is "private" when it is meant to only be used by WPEC core (and other related plugins that we are in control of). We need this kind of distinction to discourage third party developer from relying too much on routines that are volatile.

A private function should be prefixed with an underscore "_".

Example:
`function wpsc_create_product()` is a good candidate to be a public function. On the other hand, function `_wpsc_ajax_change_purchase_log_status()` should be private.

### AJAX ###
Since 3.8.9, all AJAX requests follow a uniform standard. See wpsc-admin/ajax.php and ajax.js for examples. Documentation will be written on how to use this soon. In the mean time, read the code.

The point of this standard is to make sure all AJAX requests are handled the same way: nonce validation, properly passing error objects, JSON response.

### Actions and Filters ###
Any time you want to add a new hook or filter to support third party developers, think twice, or thrice, then create an issue ticket for that. It's a bad idea to add everything third party devs requested to make their plugins work. We would end up with a bunch of legacy actions / filters that we need to maintain backward compatibility. Not something to look forward to.

That's not to say we should be too conservative. It's natural that over the years we'll have a huge accumulation of filters and actions, but it's important to make sure these filters and actions are "maintainable", meaning:

- The filter or action should not be inside a "private" function that is likely to be changed.

- If the filter is supposed to be used only by WPEC core, make it clear by adding prefixing it with "_" .

# JavaScript
Only three things:

* [Read "JavaScript: The Good Parts"](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742).
* Make sure you [JSLint](http://www.jslint.com/) your JavaScript code before making a patch or commit.
* [Google JS style guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml) is also a good starting point.