---
title: "ts transform plugin"
---

Official support for TS Transformers look dubious:
https://github.com/microsoft/TypeScript/issues/14419
https://github.com/microsoft/TypeScript/issues/16607
https://github.com/microsoft/TypeScript/issues/54276

Might be better to do as an ESLint plugin which is stable.

Handbook on writing a transformer: https://github.com/itsdouges/typescript-transformer-handbook

https://dev.doctorevidence.com/how-to-write-a-typescript-transform-plugin-fc5308fdd943

Transformers are not officially supported in the build step of TypeScript.

But that should be a non issue? We can run our transformer before compilation? Well.. we need vite integration. Vite would need to run the transforms.

We can watch the TS files and run them. But if someone has many to run? They need to be pipelined.

---

`ts-patch`: https://github.com/nonara/ts-patch
replace the vite typescript compiler? https://github.com/vitejs/vite/discussions/7580

or `ts-patch install` to overwrite local node_modules typescript.