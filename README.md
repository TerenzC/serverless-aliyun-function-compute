# Aliyun Function Compute Serverless Plugin

This plugin enables Aliyun Function Compute support within the Serverless Framework.

- [![Build Status](https://travis-ci.org/aliyun/serverless-aliyun-function-compute.svg?branch=master)](https://travis-ci.org/aliyun/serverless-aliyun-function-compute)

## Getting started

### Pre-requisites

* Node.js v8.x for using the plugin.
* Serverless CLI v1.26.1+. You can get it by running `npm i -g serverless`.
* An Aliyun account.

### Example

You can install the following example via:

```sh
$ serverless install --url https://github.com/aliyun/serverless-function-compute-examples/tree/master/aliyun-nodejs
```

The structure of the project should look something like this:

```
├── index.js
├── node_modules
├── package.json
└── serverless.yml
```

Install `serverless-aliyun-function-compute` plugin for your service.

```yaml
$ serverless plugin install --name serverless-aliyun-function-compute
```

`serverless.yml`:

```yaml
service: serverless-aliyun-hello-world

provider:
  name: aliyun
  runtime: nodejs8
  credentials: ~/.aliyun_credentials # path must be absolute

plugins:
  - serverless-aliyun-function-compute

package:
  exclude:
    - package-lock.json
    - .gitignore
    - .git/**

functions:
  hello:
    handler: index.hello
    events:
      - http:
          path: /foo
          method: get
```

`package.json`:

```json
{
  "name": "serverless-aliyun-hello-world",
  "version": "0.1.0",
  "description": "Hello World example for aliyun provider with Serverless Framework.",
  "main": "index.js",
  "license": "MIT"
}
```

`index.js`:

```js
'use strict';

exports.hello = (event, context, callback) => {
  const response = {
    statusCode: 200,
    body: JSON.stringify({ message: 'Hello!' }),
  };

  callback(null, response);
};
```

You can just create a file with following informations:

```ini
[default]
aliyun_access_key_id = nA5hjMhbg9BOoVo
aliyun_access_key_secret = 098f6bcd4621d373cade4e832627b4f6
aliyun_account_id = 1504163990726
```

The `aliyun_access_key_secret` and `aliyun_access_key_id` you can found at https://ak-console.aliyun.com/?#/accesskey . If no any Access Key, you can create a Access Key or use subaccount Access Key. And `aliyun_account_id` can be found at https://account-intl.console.aliyun.com/?#/secure .

After create the credentials file, please make sure pointing the value of the `credentials` field in `serverless.yml` to it.

See [test/project](./test/project) for a more detailed example (including how to access other Aliyun services, how to set up a HTTP POST endpoint, how to set up OSS triggers, etc.).

### Workflow

* Deploy your service to Aliyun:

  ```sh
  $ serverless deploy
  ```

  If your service contains HTTP endpoints, you will see the URLs for invoking your functions after a successful deployment.

  Note: you can use `serverless deploy function --function <function name>` to deploy a single function instead of the entire service.
* Invoke a function directly (without going through the API gateway):

  ```sh
  $ serverless invoke --function hello
  ```
* Retrieve the LogHub logs generated by your function:

  ```sh
  $ serverless logs --function hello
  ```
* Get information on your deployed functions

  ```sh
  $ serverless info
  ```
* When you no longer needs your service, you can remove the service, functions, along with deployed endpoints and triggers using:

  ```sh
  $ serverless remove
  ```

  Note: by default RAM roles and policies created during the deployment are not removed. You can use `serverless remove --remove-roles` if you do want to remove them.

## Develop

```sh
# clone this repo
git clone git@github.com:aliyun/serverless-aliyun-function-compute.git

# link this module to global node_modules
cd serverless-aliyun-function-compute
npm install
npm link

# try it out by packaging the test project
cd test/project
npm install
npm link serverless-aliyun-function-compute
serverless package
```

## License

MIT
