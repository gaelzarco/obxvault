__A valid HTTP Response follows the following format__

Each new line is separated by a return carriage: `\r\n`
- Include the `HTTP/1.1` version type, followed by status code, i.e. `200 OK`
- Then include `Content-Length: <len>`
- Afterwards include `Content-Type: <type>` and add `\r\n\r\n` immediately after instead of just `\r\n`
	- This can be `text/html, text/plain, application/json, application/x-www-form-urlencoded` and all the image, video, audio, multipart, and other types
- Finally include the raw content

Here is a completed response:
```HTTP
HTTP/1.1 200 OK
Content-Length: 360
Content-Type: text/html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5</title>
    <link rel="stylesheet" href="style.css">
  </head>

  <body>
    <script src="index.js"></script>
  </body>
</html>
```

This