# NextJS com Stylex facebook

Certifique-se de ter o Node.js e o npm instalados em sua máquina. Em seguida, siga os passos abaixo:

```bash
npx create-next-app@latest my-app
cd my-app
```

Ao criar o projeto Next.js, **não** selecione _tailwind_.

> Qual é o nome do seu projeto? my-app\
> Você gostaria de usar TypeScript? Não / **Sim**\
> Você gostaria de usar ESLint? Não / **Sim**\ 
> Você gostaria de usar Tailwind CSS? **Não** / Sim\
> Você gostaria de usar o diretório `src/`? Não / **Sim**\
>  Você gostaria de usar o App Router? (recomendado) Não / **Sim**\
> Gostaria de personalizar o alias de importação padrão (@/*)? Não / **Sim**

Agora, execute o projeto e verifique se está funcionando corretamente.

```bash
yarn dev
```

Se estiver funcionando corretamente, remova todo o estilo padrão e todo o código de modelo da página no arquivo **page.tsx**.

```tsx
export default function Home() {
  return (
    <main>
      <div>
        <h1>Stylex Demo</h1>
        <p>Hey Pushpaje, como você está?</p>
      </div>
    </main>
  );
}
```

Agora, instale todos os pacotes e configure o projeto Next.js para usar o **stylex**:

```bash
yarn add @stylexjs/stylex
yarn add -D @stylexjs/nextjs-plugin
```

Crie um arquivo _.babelrc.js_ na raiz do seu projeto, onde está localizado o arquivo **packae.json**.

```js 
module.exports = {
  presets: ['next/babel'],
  plugins: [
    [
      '@stylexjs/babel-plugin',
      {
        dev: process.env.NODE_ENV === 'development',
        runtimeInjection: false,
        genConditionalClasses: true,
        unstable_moduleResolution: {
          type: 'commonJS',
          rootDir: __dirname,
        },
      },
    ],
  ],
};
```

Modifique o arquivo _next.config.js_:

```js
/** @type {import('next').NextConfig} */
const stylexPlugin = require("@stylexjs/nextjs-plugin");

const nextConfig = {
  // Configure `pageExtensions` to include MDX files
  pageExtensions: ["js", "jsx", "mdx", "ts", "tsx"],
  // Optionally, add any other Next.js config below
};

module.exports = stylexPlugin({
  filename: "stylex-bundle.css",
  rootDir: __dirname,
})(nextConfig);
```

Agora, configure o ESLint para validar seus estilos durante a compilação:

```bash
yarn add -D @stylexjs/eslint-plugin
```

Crie um arquivo _.eslintrc.js_ na raiz do seu projeto, onde está localizado o arquivo **packae.json**.

```js
module.exports = {
  plugins: ["@stylexjs"],
  rules: {
    "@stylexjs/valid-styles": ["error", {...options}],
  },
};
```

Agora, implemente o uso do stylex em seu arquivo _page.tsx_:

```tsx
import stylex from "@stylexjs/stylex";

export default function Home() {
  return (
    <main {...stylex.props(s.main)}>
      <div>
        <h1 className={stylex(s.myHeading)}>Stylex Demo</h1>
        <p {...stylex.props(s.myPara)}>Hey Pushpaje, como você está?</p>
      </div>
    </main>
  );
}

const s = stylex.create({
  main: {
    height: "100vh",
    width: "100vw",
    display: "flex",
    justifyContent: "center",
    alignItems: "center",
  },
  myHeading: {
    color: "red",
    fontSize: 50,
    fontWeight: 700,
  },
  myPara: {
    color: "white",
    fontSize: 20,
  },
});
```

Execute o projeto:

```bash
yarn dev
```

Agora você configurou o stylex com o Next.js 14 em seu novo projeto.
