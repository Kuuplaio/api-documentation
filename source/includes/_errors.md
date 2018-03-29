# Errors

The Mobbty API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request was malformed or incomplete
401 | Unauthorized -- The provided API key is wrong
403 | Forbidden -- The requested endpoint is not part of your current subscription
404 | Not Found -- The specified endpoint could not be found
405 | Method Not Allowed -- You tried to access an endpoint with an invalid method
429 | Too Many Requests -- You're making too many requests! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
