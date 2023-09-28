# CodeGPT

AI powered VS code plugin!

## Setup

Follow the steps below to set up the project:

To build the extension from source, clone the repository and run `npm install` to install the dependencies. You have to change some code in `chatgpt` module because VSCode runtime does not support `fetch`. Open `node_modules/chatgpt/dist/index.js` and add the following code at the top of the file:

```js
import fetch from 'node-fetch'
```

Then remove the following lines (around line 20):

```js
// src/fetch.ts
var fetch = globalThis.fetch;
```

You also need to replace the following part near the top of the file:

```js
// src/tokenizer.ts
import { encoding_for_model } from "@dqbd/tiktoken";
var tokenizer = encoding_for_model("text-davinci-003");
function encode(input) {
  return tokenizer.encode(input);
}
```

with

```js
// src/tokenizer.ts
import GPT3TokenizerImport from "gpt3-tokenizer";
var GPT3Tokenizer = typeof GPT3TokenizerImport === "function" ? GPT3TokenizerImport : GPT3TokenizerImport.default;
var tokenizer = new GPT3Tokenizer({ type: "gpt3" });
function encode(input) {
  return tokenizer.encode(input).bpe;
}
```

due to the fact that the `@dqbd/tiktoken` module is causing problems with the VSCode runtime. Delete `node_modules/@dqbd/tiktoken` directory as well. *If you know how to fix this, please let me know.*

In file `node_modules/chatgpt/build/index.d.ts`, change line 1 to

```js
import * as Keyv from 'keyv';
```

and line 4 to

```js
type FetchFn = any;
```
## Configuration

To configure the extension, please navigate to VS Code settings and update the following variables

1. `Chatgpt: Api Key`: OpenAI API key from https://platform.openai.com/account/api-keys

2. `Chatgpt: Model`: Which GPT model to use
  
## Contributing

Feel free to contribute to the project by creating issues or pull requests.

