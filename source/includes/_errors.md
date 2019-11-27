# Errors

The iBuildApp API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your token is invalid (expired/incorrect).
403 | Forbidden -- The entry requested is hidden for administrators only.
404 | Not Found -- The specified entry could not be found.
405 | Method Not Allowed -- You tried to access an entry with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The requested entry has been removed from our servers.
429 | Too Many Requests -- You're sending too many requests.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
