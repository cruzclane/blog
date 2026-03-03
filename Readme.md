# My Zola Blog

A personal blog built with Zola and styled with Tailwind CSS.

## Tech Stack

- **Static Site Generator:** [Zola](https://www.getzola.org/)
- **Styling:** [Tailwind CSS](https://tailwindcss.com/)
- **Package Manager:** [npm](https://www.npmjs.com/)

## Local Development

These are the steps to run the project locally.

### Prerequisites

- You need to have [Zola](https://www.getzola.org/documentation/getting-started/installation/) installed.
- You need to have [Node.js](https://nodejs.org/) and npm installed.

### Running the Project

1.  **Install dependencies:**

    ```bash
    npm install
    ```

2.  **Start the development servers:**

    You need to run two commands in parallel in separate terminal windows.

    - **Terminal 1: Start the Tailwind CSS watcher:**
      This command will watch for changes in the CSS files and automatically rebuild the stylesheet.

      ```bash
      npm run dev
      ```

    - **Terminal 2: Start the Zola server:**
      This command will start the local development server and automatically reload the browser when you make changes to content or templates.

      ```bash
      zola serve
      ```

    Now you can open your browser and go to `http://127.0.0.1:1111` to see the website.

## Building for Production

When you are ready to deploy, you need to build the optimized and minified CSS file.

```bash
npm run build
```

After that, you can build the static site with Zola.

```bash
zola build
```

The static files will be generated in the `public` directory.
