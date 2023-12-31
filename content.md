# Sinatra: Exchange Rates

The target for this project can be accessed with the username and password found on Canvas:

[exchange-rates.matchthetarget.com](https://exchange-rates.matchthetarget.com/)

Our goal in this project is to build an app that displays pairs of currencies and their current exchange rate, based on API data. For example, this URL displays the current exchange rate between United States Dollars and Indian Rupees:

[https://exchange-rates.matchthetarget.com/USD/INR](https://exchange-rates.matchthetarget.com/USD/INR)

The list of currencies and their exchange rates are dynamic, coming from the [exchangerate.host API](https://exchangerate.host/).

Click around the target and explore to get a feel for how it works. What URLs are accessible in the target? How many different routes do you think there are? Remember that some of the routes might be flexible, so it's not a 1-to-1 with URLs that you're able to visit.

Then, use what we've learned about 

- [dynamic path segments](https://learn.firstdraft.com/lessons/111-sinatra-dice-dynamic-routes), 
- [APIs](https://learn.firstdraft.com/lessons/225-intro-to-apis) (including the [Umbrella project](https://learn.firstdraft.com/lessons/104-umbrella)), 
- and [JSON parsing](https://learn.firstdraft.com/lessons/104-umbrella#useful-methods) 

to start defining routes in your app and make it match the target!

## Setup a codespace

This project includes automated tests, so click here to get an access token:

LTI{Load Sinatra Exchange Rates assignment}(https://grades.firstdraft.com/launch)[S9ymPy6WCsn18gLbByVbZQ7k]{vfdtzJb5bLYqYwuqgeRKpc5d}(10)[Sinatra Exchange Rates Project]

Then fork the repo and set up a codespace as usual.

<div class="bg-blue-100 py-1 px-5" markdown="1">

The [exchangerate.host API](https://exchangerate.host/) requires an API access key. There is a free tier that you will need to sign up for and use in your app.

[Follow the link to the exchangerate.host API, then watch and follow along with this video](https://share.descript.com/view/wK0XpujLtJy) demonstrating how to get your own API key and store it securely in your GitHub ENV variables. Anytime you need an API key for a given service, the steps will be similar, so get some practice now getting your key and adding it to the repository before continuing.
</div>

## Guidelines

You should be able to achieve this with what you've already learned! Here are a few hints:

### Add the API access key

Follow the steps in the video above to get an API key, and [store it securely](https://learn.firstdraft.com/lessons/52-storing-credentials-securely) so that you can access it in your `app.rb` file like:

```ruby
api_url = "https://api.exchangerate.host/list?access_key=#{ENV["EXCHANGE_RATE_KEY"]}"
```

**This will prevent you from publishing the actual API key on GitHub!**

<div class="bg-blue-100 py-1 px-5" markdown="1">

After you add the environment variable be sure to [stop and restart the codespace](https://learn.firstdraft.com/lessons/47-codespaces-setup#closing-and-reopening-a-workspace) for access to the new secret.
</div>

### Hoppscotch

Remember, [Hoppscotch](https://learn.firstdraft.com/lessons/225-intro-to-apis#hoppscotch-a-client-designed-for-learning) is a useful tool for exploring API URLs (or "endpoints"). It will format and indent JSON to make it easier to understand the structure of nested arrays/hashes.

### Useful exchangerate.host endpoints

Remember to replace `EXCHANGE_RATE_KEY` with the key you got when you signed up in the video above. And **do not put the key directly in your code; [store them securely and access them from the `ENV` variable](https://learn.firstdraft.com/lessons/52-storing-credentials-securely).**

 - API endpoint + query string that returns all available currencies (`/list ? access_key`): 
      
```
https://api.exchangerate.host/list?access_key=EXCHANGE_RATE_KEY
```

 - API endpoint + query string to convert between two currencies (`/convert ? access_key & from & to & amount`):

```
https://api.exchangerate.host/convert?access_key=EXCHANGE_RATE_KEY&from=USD&to=INR&amount=1
```

 - [Full API documentation](https://exchangerate.host/documentation)

### Homepage

The root URL of the target displays a list of all currencies that are available from the exchangerate.host API. This list of currencies changes, e.g. as new cryptocurrencies are added to their dataset. So, we can't just hard code the list of currencies into our homepage; we need to draw it dynamically, based on the data from the `api.exchangerate.host/list` endpoint.

- Use the `HTTP` class to `get` the `String` located at that URL.
- Then use the `JSON` class to `parse` the `String` into an `Array`.
- Then, in a view template, loop through the `Array` of symbols and display each symbol in a `<li>`.

### Some pseudo-code to get you started

Here's how I might begin my project in the `app.rb` file to define the homepage with the list of symbols:

```ruby
# /app.rb

require "sinatra"
require "sinatra/reloader"
require "http"

# define a route
get("/") do

  # build the API url, including the API key in the query string
  api_url = "https://api.exchangerate.host/list?access_key=#{ENV["EXCHANGE_RATE_KEY"]}"

  # use HTTP.get to retrieve the API information
  raw_data = HTTP.get(api_url)

  # convert the raw request to a string
  raw_data_string = raw_data.to_s

  # convert the string to JSON
  parsed_data = JSON.parse(raw_data_string)

  # get the symbols from the JSON
  # @symbols = parsed_data ...

  # render a view template where I show the symbols
  # erb(:homepage)
end
```

Try and use that code to get you started! You will need to use dynamic path segments with the two currencies for the other routes, e.g.

```ruby
# /app.rb

# ...

get("/:from_currency") do
  @original_currency = params.fetch("from_currency")

  api_url = "https://api.exchangerate.host/list?access_key=#{ENV["EXCHANGE_RATE_KEY"]}"
  
  # some more code to parse the URL and render a view template
end

get("/:from_currency/:to_currency") do
  @original_currency = params.fetch("from_currency")
  @destination_currency = params.fetch("to_currency")

  api_url = "https://api.exchangerate.host/convert?access_key=#{ENV["EXCHANGE_RATE_KEY"]}&from=#{@original_currency}&to=#{@destination_currency}&amount=1"
  
  # some more code to parse the URL and render a view template
end
```

### Hash#each

<div class="bg-blue-100 py-1 px-5" markdown="1">

You don't need to use `Hash#each` to solve the project, but you may find it useful.
</div>

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

---
