# circle_entity
circle entity secret guide (Arc edition)

a step-by-step developer guide to generating, registering, and securing an Entity Secret using the Circle Developer-Controlled Wallets SDK.

this repo walks you through:
- generating an Entity Secret
- registering it with Circle
- storing recovery data safely
- setting up environment variables correctly

---

## ⚠️ prerequisites

before starting, ensure you have:

- Node.js (v18+ recommended)
- npm installed
- vscode (or any editor)
- a Circle account → https://console.circle.com
- a created API key from Circle dashboard

---

## 📁 project setup

clone or create a new folder:

```bash
mkdir circle-entity-secret-guide
cd circle-entity-secret-guide
npm init -y

---

## install dependencies:

```bash
npm install @circle-fin/developer-controlled-wallets dotenv
npm install -D tsx typescript

---

## environments variables

CIRCLE_API_KEY=your_api_key_here
CIRCLE_ENTITY_SECRET=your_entity_secret_here

---

step 1 - generate entity secret

create file : 1.ts


```bash
import { generateEntitySecret } from "@circle-fin/developer-controlled-wallets";

const result = generateEntitySecret();

console.log("Generated Entity Secret:");
console.log(result);

run - npx tsx 1.ts

output: entity Secret: xxxxxxxxxxxxx

put into .env

CIRCLE_ENTITY_SECRET=your_generated_secret

---

step 2 - register entity secret

create file: 2.ts

```
import dotenv from "dotenv";
dotenv.config();

import fs from "fs";
import { registerEntitySecretCiphertext } from "@circle-fin/developer-controlled-wallets";

(async function main() {
  const apiKey = process.env.CIRCLE_API_KEY;
  const entitySecret = process.env.CIRCLE_ENTITY_SECRET;

  if (!apiKey || !entitySecret) {
    throw new Error("Missing CIRCLE_API_KEY or CIRCLE_ENTITY_SECRET in .env");
  }

  const response = await registerEntitySecretCiphertext({
    apiKey,
    entitySecret,
    recoveryFileDownloadPath: "./",
  });

  fs.writeFileSync(
    "recovery_file_data",
    response.data?.recoveryFile ?? ""
  );

  console.log("Entity secret registered successfully");
  console.log("Recovery file saved: recovery_file_data");
})();

run registration - npx tsx 2.ts

output

entity secret registered successfully
recovery file saved: recovery_file_data

file created :recovery_file_data

---

step 3 - create API key

go to : https://console.circle.com


- sign in
- go to API Keys
- create Standard Key
- copy key
- add to .env
- CIRCLE_API_KEY=your_api_key_here

---

## what you did ??
- secure entity identity layer
- encrypted wallet registration flow
- recovery system for safety
- circle wallet base integration
  
-- what this enables
- programmatic wallets
- embedded crypto apps
- automated treasury systems
- wllet-as-a-service infra
---

end.
