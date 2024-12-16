# 🚀 Usage

## 1. Setup instruction

Install dependencies:
```vue
npm install
```
Run the code:
```vue
npm run dev
```
Run the server:
```vue
npm run serve
```

## 2. Project structure

```vue
src/
├── assets/               # Images, styles, etc.
├── components/           # Reusable Vue components
    ── CartTotal.vue      # Cart component
    ── Form.vue           # Form component
    ── OptionProducts.vue # Products components
    ── SelectCity.vue     # Selection of region component
    ── Stepper.vue        # Stepper component
├── App.vue               # Root component
├── main.js               # Application entry point
```

## 💿 Install

Set up your project using your preferred package manager. Use the corresponding command to install the dependencies:

| Package Manager                                                | Command        |
|---------------------------------------------------------------|----------------|
| [yarn](https://yarnpkg.com/getting-started)                   | `yarn install` |
| [npm](https://docs.npmjs.com/cli/v7/commands/npm-install)     | `npm install`  |
| [pnpm](https://pnpm.io/installation)                          | `pnpm install` |
| [bun](https://bun.sh/#getting-started)                        | `bun install`  |


## 💡 Usage

This section covers how to start the development server and build your project for production.

### Starting the Development Server

To start the development server with hot-reload, run the following command. The server will be accessible at [http://localhost:3000](http://localhost:3000):

```bash
yarn dev
```

(Repeat for npm, pnpm, and bun with respective commands.)

### Building for Production

To build your project for production, use:

```bash
yarn build
```

(Repeat for npm, pnpm, and bun with respective commands.)

Once the build process is completed, your application will be ready for deployment in a production environment.


