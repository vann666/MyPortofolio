# Svelte Scroll Animation

This project is a Svelte application that demonstrates scroll animations using the IntersectionObserver API and Tailwind CSS for styling. The main component, `Main.svelte`, incorporates a `ScrollAnimation` component that applies animations to elements as they come into view while scrolling.

## Project Structure

```
svelte-scroll-animation
├── src
│   ├── components
│   │   ├── ScrollAnimation.svelte  # Component for scroll animations
│   │   └── Main.svelte              # Main layout component
│   ├── App.svelte                    # Entry point of the application
│   └── main.js                       # Initializes and mounts the Svelte app
├── public
│   └── index.html                    # Main HTML file for the app
├── package.json                       # npm configuration file
├── svelte.config.js                  # Svelte configuration settings
└── README.md                         # Project documentation
```

## Installation

To get started with this project, clone the repository and install the dependencies:

```bash
git clone <repository-url>
cd svelte-scroll-animation
npm install
```

## Running the Application

To run the application in development mode, use the following command:

```bash
npm run dev
```

This will start a local development server, and you can view the application in your browser at `http://localhost:5000`.

## Features

- **Scroll Animations**: Elements animate into view as you scroll down the page.
- **Responsive Design**: The application is styled using Tailwind CSS, ensuring a responsive layout across different screen sizes.
- **Svelte Components**: The project is structured using Svelte components for modularity and reusability.

## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request with your changes.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.