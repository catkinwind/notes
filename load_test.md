1. load test terms: What are Transactions, request, TPS, throughput?
  In some cases transaction can stand for a single request but more often it is a series of requests representing a piece of business logic like:

  User opens a site
  User opens login page
  User performs login
  User does some stuff
  User logs out
  With some think times in between of course as a real life user doesn't hammer application continuously, he needs some time to type credentials, wait for page load, read information from page, etc. Failure at any point (1 to 5) will lead to the whole transaction failure.

  See the following material which can shed some light on the terms being asked:

  JMeter Glossary
  The Load Reports
