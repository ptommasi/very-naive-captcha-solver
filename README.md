A very simple, very naive, sometime unreliable and very unsecure docker image which just takes in input a base64 encoded image and returns the text within.

Use case is very simple captchas, and actually it doesn't always work either, but it is cheaper to query this service first and then fallback on 2captcha in case of error.

Default port is 4184, but it can be changed using the value of the $PORT environment variable.

Only POST requests under the `/solve` path are served, and payload must be in the format `{ data: <base64image> }`. 

An example usage from another service on the same machine would be:
```javascript
  // Fetch the image base64 data (it could be an external service)
  const contents = fs.readFileSync('data/Captcha_alvgyhvdaj.jpg', { encoding: 'base64' });
  // Put the base64 image in the 'data' field 
  const response = await axios.post("http://localhost:4184/solve", { data: contents }))
  console.log(response.data);
```

And output would be something like:
```json
{ solution: 'JBKAMB' }
```

If you have an URL rather than a file (most probable), this async function could be used instead:
```
async function getBase64(url: string) {
  const response = await axios.get(url, { responseType: 'arraybuffer' });
  return Buffer.from(response.data, 'binary').toString('base64');
}
```