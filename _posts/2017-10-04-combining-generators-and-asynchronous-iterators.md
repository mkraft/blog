I thought of this today and then my mind was blown when it worked.

Say you want to do some asynchronous stuff *in a particular order* and wait for each completion before proceeding. It can easily be done with generators and asynchronous iterators.

Here’s an example in which

`walkAPI` is a generator function (note the `*`after the `function` keyword) and the `app` function contains an asynchronous iterator using the `await` keyword (line 15):

app.js:
```js
const fetch = require("node-fetch");

function* walkAPI(postIDs) {
  for (let i = 0; i < postIDs.length; i++) {
    const url = `https://jsonplaceholder.typicode.com/posts/${postIDs[i]}`;
    yield fetch(url, {
      headers: {
        Accept: "application/json",
      },
    }).then(response => response.json());
  }
}

const app = async () => {
  for await (const json of walkAPI([12, 18, 33, 46, 59])) {
    console.log(json);
  }
};

app();
```

package.json:
```json
{
  "name": "",
  "version": "0.0.1",
  "description": "",
  "dependencies": {
    "node-fetch": "^1.7.3"
  },
  "devDependencies": {},
  "scripts": {
    "start": "node --harmony-async-iteration app.js"
  },
  "author": "",
  "license": ""
}
```

Run:
```bash
$ npm install
$ npm start
```

Output:
```json
{ "userId": 2,
  "id": 12,
  "title": "in quibusdam tempore odit est dolorem",
  "body": "itaque id aut magnam\npraesentium quia et ea odit et ea voluptas et\nsapiente quia nihil amet occaecati quia id voluptatem\nincidunt ea est distinctio odio" }
{ "userId": 2,
  "id": 18,
  "title": "voluptate et itaque vero tempora molestiae",
  "body": "eveniet quo quis\nlaborum totam consequatur non dolor\nut et est repudiandae\nest voluptatem vel debitis et magnam" }
{ "userId": 4,
  "id": 33,
  "title": "qui explicabo molestiae dolorem",
  "body": "rerum ut et numquam laborum odit est sit\nid qui sint in\nquasi tenetur tempore aperiam et quaerat qui in\nrerum officiis sequi cumque quod" }
{ "userId": 5,
  "id": 46,
  "title": "aut quo modi neque nostrum ducimus",
  "body": "voluptatem quisquam iste\nvoluptatibus natus officiis facilis dolorem\nquis quas ipsam\nvel et voluptatum in aliquid" }
{ "userId": 6,
  "id": 59,
  "title": "qui commodi dolor at maiores et quis id accusantium",
  "body": "perspiciatis et quam ea autem temporibus non voluptatibus qui\nbeatae a earum officia nesciunt dolores suscipit voluptas et\nanimi doloribus cum rerum quas et magni\net hic ut ut commodi expedita sunt" }
```

Note that the promises all resolve in the order specified by our `postIDs` parameter. This is not as easily done prior to the combination of the features and wouldn't be as readable.