# Sinatra: Exchange Rates

Our goal in this project is to build an app that displays pairs of currencies and their current exchange rate, based on API data. For example, [this URL displays the current exchange rate between United States Dollars and Indian Rupees](https://exchange-rates.matchthetarget.com/USD/INR).

The list of currencies and their exchange rates are dynamic, coming from the [free exchangerate.host API](https://exchangerate.host/).

Let's use what we've learned about [dynamic path segments](https://learn.firstdraft.com/lessons/111-sinatra-dice-dynamic-routes), [APIs](https://learn.firstdraft.com/lessons/98#intro-to-apis), and [JSON parsing](https://learn.firstdraft.com/lessons/104-umbrella#useful-methods) to build it.

Here is your target: 

[exchange-rates.matchthetarget.com](https://exchange-rates.matchthetarget.com/)

Click around and explore to get a feel for how it works. What URLs are accessible in the target? How many different routes do you think there are? Remember that some of the routes might be flexible, so it's not a 1-to-1 with URLs that you're able to visit.

Then, start defining routes in your app and making it match the target!

## Setup a codespace

This project includes automated tests, so click here to get an access token:

LTI{Load Sinatra Exchange Rates assignment}(https://grades.firstdraft.com/launch)[S9ymPy6WCsn18gLbByVbZQ7k]{vfdtzJb5bLYqYwuqgeRKpc5d}(10)[Sinatra Exchange Rates Project]

Then fork the repo and set up a codespace as usual.

## Guidelines

You should be able to achieve this with what you've already learned! But here are a few hints:

### Hoppscotch

Remember, [Hoppscotch](https://learn.firstdraft.com/lessons/98#hoppscotch-a-client-designed-for-learning) is a useful tool for exploring API URLs (or "endpoints"). It will format and indent JSON to make it easier to understand the structure of nested arrays/hashes.

### Useful exchangerate.host endpoints

 - API endpoint that returns all available currencies: [https://api.exchangerate.host/symbols](https://api.exchangerate.host/symbols)
 - API endpoint to convert between two currencies (e.g. USD to INR): [https://api.exchangerate.host/convert?from=USD&to=EUR](https://api.exchangerate.host/convert?from=USD&to=INR)
 - [Full API documentation](https://exchangerate.host/#/docs)

### Homepage

The root URL of the target displays a list of all currencies that are available from the exchangerate.host API. This list of currencies changes, e.g. as new cryptocurrencies are added to their dataset. So, we can't just hard code the list of currencies into our homepage; we need to draw it dynamically, based on the data from [the `/symbols` endpoint](https://api.exchangerate.host/symbols).

- Use the `HTTP` class to `get` the `String` located at that URL.
- Then use the `JSON` class to `parse` the `String` into an `Array`.
- Then, in a view template, loop through the `Array` of symbols and display each symbol in a `<li>`.

#### Hash#each

Note: `Hash` has a method called `.each` also. Suppose you have a hash like this:

```ruby
my_hash = {
  :alice => "instructor",
  :bob => "student",
  :carol => "teaching assistant"
}
```

Then you could call `.each` on `my_hash`. Unlike the `Array` `.each` method, the `Hash` `.each` method provides _two_ block variables: the first for the key and the second for the value of each element:

```ruby
my_hash = {
  :alice => "instructor",
  :bob => "student",
  :carol => "teaching assistant"
}

my_hash.each do |zebra, giraffe|
  pp "Key: #{zebra}"
  pp "Value: #{giraffe}"
  pp "============"
end
```
{: .repl #hash_each title="Hash.each" points="1"}

### Route for symbol

Set up a route for requests of the pattern:

```http
GET /USD HTTP/1.1
```

```http
GET /INR HTTP/1.1
```

```http
GET /KES HTTP/1.1
```

Whatever comes after the `/` in the first path segment should be assumed to be a valid currency symbol.

Assume that the user submits a request for `GET /USD`. Then they should get back an HTML page that:

- Has an `<h1>` saying "Convert USD"
- For each currency that is available through the API, a list item that says "Convert USD to [other symbol]..."

    For example, "Convert 1 USD to KES..."

### Route for pair of symbols

Set up a route for requests of the pattern:

```http
GET /USD/INR HTTP/1.1
```

```http
GET /INR/EUR HTTP/1.1
```

```http
GET /KES/USD HTTP/1.1
```

The first and second path segments should be assumed to be valid currency symbols.

Assume that the user submits a request for `GET /USD/INR`. Then they should get back an HTML page that:

- Has an `<h1>` saying "Convert USD to INR"
- Has a `<p>` saying e.g. "1 USD equals 82.016658 INR."

### Links

Go back through each view template and add links to get from route to route.

---

- Ask lots of questions!
- Request office hours to discuss if you get stuck.
- Double-check you've got it working with `rake grade`.

---

- Approximately how long (in minutes) did this lesson take you to complete?
{: .free_text_number #time_taken title="Time taken" points="1" answer="any" }
